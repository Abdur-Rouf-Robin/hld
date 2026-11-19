# HLD

## chapter 1 Scalability

- scalability = the system still answers when there is more traffic or more data
- vertical = a bigger machine. more CPU, more RAM. simple, and it has a ceiling
- horizontal = more machines. the work has to be shareable
- a stateless API scales horizontally. the session is in the token or in Redis, not in the process memory
- a single Postgres does not scale by copying the process. writes have a primary
- we scale the thing that is actually busy. a CPU chart, a pool chart, a disk chart
- VMS, Anemi, and Bracket run as a small number of processes behind nginx. that is the real shape. the rest of this file is how the shape grows

```text
one Node process, CPU at 90 percent
-> a second process, nginx splits the requests
the database is still one primary
```

- more API processes do not make one hot row faster. that row is a Postgres problem
- a video file is bandwidth. more Node processes do not fix a missing cache in front of the bytes

- homework of chapter 1
```text
// 1a  vertical or horizontal: a second API container
// 1b  why that second container does not speed up one locked row
// 1c  where a JWT session lives, so either container can serve the next call
```

## chapter 2 Load balancing

- a load balancer spreads requests across healthy processes
- nginx is the one in front of our APIs
- round robin = take turns
- least connections = send to the one with fewer open requests
- a health check removes a process that stopped answering
- sticky sessions pin a user to one process. we avoid that by keeping state out of the process
- layer 4 balances TCP connections. layer 7 can look at the path and the host
- the balancer is not a backup of the data

```nginx
upstream api {
  server 127.0.0.1:3000;
  server 127.0.0.1:3001;
}
server {
  location /api/ {
    proxy_pass http://api;
  }
}
```

- two upstreams, one URL. the client does not pick the process
- a WebSocket is a long connection. the balancer must allow the upgrade and should not kill idle sockets too fast
- if both upstreams die, the client sees 502. the balancer has nobody to call

- homework of chapter 2
```text
// 2a  round robin versus least connections
// 2b  why we do not need a sticky cookie when the login is a JWT
// 2c  what the client sees when every upstream fails
```

## chapter 3 Caching and CDN

- a cache = a copy closer to the reader, so we do not recompute or refetch
- the key must include everything that changes the value
- when the row changes, delete the key or give the key a short TTL
- Redis is the cache and the queue store on VMS and Bracket. Anemi does not use Redis
- a CDN = a cache at the edge of the network, for bytes that many people read
- HLS segments and images are CDN material. a private laytime number is not
- cache aside = the app looks in the cache, and on a miss reads the database and fills the cache
- a stampede = many misses at once all hit the database. a short lock or a stale value reduces that

```ts
async function vesselName(id: string): Promise<string> {
  const key = `vessel-name:${id}`;
  const hit = await redis.get(key);
  if (hit) return hit;
  const row = await db.vessel.findUniqueOrThrow({ where: { id } });
  await redis.set(key, row.name, "EX", 60);
  return row.name;
}
```

- TTL 60 means a rename can be a minute late. if that is too late, delete the key in the update route
- do not cache "may this user edit" for a long TTL. a role change would be late
- the CDN caches by URL. a private URL with the token in the query string can leak via logs. put auth where the CDN is told not to cache

- homework of chapter 3
```ts
// 3a  the key for port call pc_12's vessel name
// 3b  what we do in the PATCH that renames the vessel
// 3c  a public HLS segment versus a demurrage figure. which one may sit on a CDN
```

## chapter 4 Sharding, replication, and consistent hashing

- replication = a copy. the primary writes. replicas replay. chapter material from the database notes, seen here as a design choice
- a replica helps reads and helps us survive a dead primary. it copies a bad DELETE too
- sharding = different rows on different servers, split by a key
- we have not sharded these products. one Postgres is the design until a measurement says it is not enough
- a shard key we always have keeps one tournament's matches together
- a query without the key hits every shard
- consistent hashing = the ring. a key goes to the next server clockwise
- when one server is added, only the keys that walked onto it move. the other keys stay
- virtual nodes keep one physical server from owning a huge arc of the ring
- this is how some caches avoid a total reshuffle. it is also a shard map

```text
ring: server A, server B, server C
key "t_9" hashes between A and B, so B stores it
add server D between A and B
"t_9" may move to D. keys on the other arcs stay
```

- replication and sharding solve different problems. a replica is the same rows. a shard is a subset of the rows
- do the replica first. shard when one primary's disk or write rate is the wall

- homework of chapter 4
```text
// 4a  replica or shard: the report can be a second behind
// 4b  replica or shard: the table no longer fits one disk
// 4c  what moves when we add a server on a consistent-hash ring
// 4d  a query "all matches for this user" if we sharded by tournament. what is the cost
```

## chapter 5 CAP, PACELC, and consistency models

- CAP = during a partition, we choose availability or consistency. we do not get a magical third
- partition = the network cut. some nodes cannot talk
- consistency here means every read sees the latest write
- availability means every request gets a response, even if it is stale
- CP = refuse to answer rather than answer with a stale or split brain. a money figure wants this
- AP = answer with a possibly old value. a view counter can live with it
- CA is not a real choice on a network that can cut
- PACELC adds the healthy day. if there is a partition, A or C. else, latency or consistency
- a synchronous replica waits. that is consistency over latency
- an asynchronous replica returns sooner. that is latency, and a chance of lag
- strong consistency = a read sees the latest committed write
- eventual consistency = if writes stop, copies catch up
- read-your-writes = this user sees their own write, even if other users can be a moment behind

```text
laytime money: rather fail the read than show two different figures
Anemi view counter: an old count for a second is acceptable
```

- Postgres synchronous commit waits for the standby. asynchronous commit can lose the last transactions if the primary dies before the replica gets them
- we pick per feature. one database can be the source of truth while a cache is eventual

- homework of chapter 5
```text
// 5a  CP or AP for the demurrage number
// 5b  CP or AP for a play count
// 5c  what the E in PACELC is asking on a normal day
// 5d  read-your-writes in one sentence
```

## chapter 6 Kafka, pub/sub, and events

- an event = a fact that already happened. LaytimeCalculated. PlaybackMoved. PaymentReceived
- the producer does not call the consumer. it publishes the fact
- pub/sub = many subscribers can hear one fact
- a queue usually gives a job to one worker. pub/sub gives a copy to each subscriber
- Kafka is a log of events, kept in order inside a partition
- consumers track an offset. they can replay
- we have not operated Kafka. BullMQ is the queue we have run. Socket.IO is the push to browsers
- a domain event inside one service can be a function call. an event that crosses a process is a message

```ts
type LaytimeCalculated = {
  type: "LaytimeCalculated";
  portCallId: string;
  demurrage: number;
  at: string;
};
```

- the name is past tense because it happened
- the payload has the id and the number. a consumer that needs more loads it from Postgres
- at-least-once delivery means the consumer must be idempotent. the same event can arrive twice
- ordering inside one partition is a Kafka promise. ordering across the whole system is not

- homework of chapter 6
```ts
// 6a  queue or pub/sub: one worker should recalculate laytime
// 6b  queue or pub/sub: every open screen should hear the new total
// 6c  why the consumer still checks "have I stored this event id"
// 6d  name the tool we have actually run for jobs, and the tool we have not
```

## chapter 7 Microservices and an API gateway

- a microservice = a process that owns one area and its data
- a modular monolith = one process, many modules, one database. Nest modules are this
- we split a service out when a module has its own scaling need or its own release cycle
- splitting first, without that reason, adds network failures and distributed transactions
- each service owns its tables. another service asks through an API or an event. it does not read the tables
- an API gateway = one front door. it routes, authenticates, and limits. nginx is that door for us
- the gateway is not where the laytime rule lives. the rule stays in the service

```text
browser -> nginx -> nest process
                 -> the same process, module port-calls, module auth
```

```text
later, only if a measurement says so
browser -> gateway -> auth service
                   -> laytime service
```

- a shared database between two "services" is still one system with two doors
- the failure mode of a split is a timeout. the caller needs a limit and a behavior when the other service is down

- homework of chapter 7
```text
// 7a  what we run today, a modular monolith or a fleet of services
// 7b  a reason that would justify a split
// 7c  why the gateway does not compute demurrage
```

## chapter 8 Rate limiter design

- a rate limiter protects a route from a burst
- fixed window = N hits per 60 seconds, counted from a clock bucket. the edge of two buckets can allow almost 2N
- sliding window = the count looks back over the last 60 seconds. fairer, a bit more bookkeeping
- token bucket = tokens drip in. a request spends one. a burst is allowed up to the bucket size
- the counter lives in Redis so every API process sees it
- the key is who we are limiting. an IP for login. a user id for a write. a route name is part of the key
- 429 is the response. a Retry-After header tells a polite client when to try
- this is the design picture. the small version we ship is INCR and EXPIRE

```ts
const key = `limit:${userId}:port-calls`;
const hits = await redis.incr(key);
if (hits === 1) await redis.expire(key, 60);
if (hits > 30) throw new HttpException("Too many requests", 429);
```

- login is stricter than a normal read. 10 per minute on login. a higher number on a list route
- do not use only the IP when many users share a NAT. a bad limit locks a whole office
- the limiter is not authentication. a valid user can still be limited

- homework of chapter 8
```ts
// 8a  fixed window's weakness at the boundary
// 8b  the Redis key for user u_3 writing port calls
// 8c  why the limit store is Redis and not a variable in the Node process
```

## chapter 9 Bloom filters and circuit breakers

- a bloom filter = a small bit array that says "definitely not" or "maybe"
- it can say maybe for an item that was never added. it must not say no for an item that was added
- we use it to skip a disk lookup. a "maybe" still checks the database
- a password-breach list or "have we seen this id" can be a bloom filter. a wrong "maybe" only costs a lookup
- a circuit breaker = stop calling a dependency that is failing
- closed = calls go through
- open = calls fail fast, without waiting for the timeout
- half-open = let a few calls through to see if it recovered
- we count failures. past a threshold, we open. after a cool-down, we try again
- our HTTP client to Stripe or to a webhook target is the place this fits. we have the idea. a library implements the states

```text
bits: a hash of the key sets 3 bits
lookup: if any of the 3 bits is 0, the key was never added
if all 3 are 1, check the real store
```

```text
5 timeouts in a row -> open for 30 seconds
then one trial call
success -> closed
failure -> open again
```

- a bloom filter is not a set. we cannot list what is in it, and we cannot delete one key without extra design
- an open circuit returns an error quickly. it does not invent a successful payment

- homework of chapter 9
```text
// 9a  the bloom filter says no. do we read Postgres
// 9b  the bloom filter says maybe. do we read Postgres
// 9c  what the caller gets while the circuit is open
```

## chapter 10 Sagas and blob storage

- a saga = a business action that spans steps which cannot share one database transaction
- each step has a forward action and a compensating action
- charge the card, then reserve the seat. if the seat fails, refund
- we do not use a saga inside one Postgres. one transaction is simpler and correct
- a saga is for when the steps are different systems. Stripe and our database
- the record of the saga is a row. pending, done, compensating. so a crash can resume
- blob storage = files as objects. a key, bytes, and metadata
- the picture's blob store is where an upload lands. S3 is the well-known product. the idea is the key and the bytes
- the database stores the key and the owner. the blob store stores the bytes
- a signed URL is a temporary right to PUT or GET that key

```text
1. create payment row: pending
2. Stripe charge
3. mark paid
if 3 never happens, a worker looks at pending rows and asks Stripe what occurred
```

```text
object key: videos/u_1/upload_9.mp4
database: video id, owner, key, status
```

- compensation must be idempotent too. a retry must not refund twice
- the object store is not the source of "who may watch". the database and the auth check are

- homework of chapter 10
```text
// 10a  why a port call and its first fact are a transaction, not a saga
// 10b  a saga step pair for "take payment, then confirm the team"
// 10c  what lives in Postgres versus what lives in the object store for a video
```

## chapter 11 Back-of-envelope estimation

- back of the envelope = a rough size so the design is in the right order of magnitude
- we want powers of ten, not a fake precise number
- useful anchors: a day has about 1e5 seconds. a million is 1e6. a request of 1 KB is 1e3 bytes
- write the assumption, then the multiply

```text
50 requests per second average
peak is 5 times average = 250 rps
each request is 2 KB in and 8 KB out
out bandwidth ~ 250 * 8 KB = 2 MB/s at peak
a day of access logs, one line 200 bytes, 50 * 1e5 * 200 ~ 1e9 bytes ~ 1 GB
```

- 1e5 seconds/day is 100,000. a real day is 86,400. close enough for this
- storage for video is a different sum. 500 uploads a day, 200 MB each, is 100 GB a day before we delete or transcode
- HLS does not shrink that sum by itself. transcoding choices do. the envelope tells us to put the bytes in object storage and a cache, not through the Node process
- if the envelope says 10 requests a second, we do not draw 40 services

```text
laytime recalculations: 1,000 port calls, each edited 20 times a day
20,000 jobs a day
20,000 / 1e5 seconds ~ 0.2 jobs per second
a single worker is enough until that number grows by orders of magnitude
```

- homework of chapter 11
```text
// 11a  20 rps, 10 KB response. peak bandwidth at 4 times average
// 11b  1,000 videos of 100 MB. the disk before any transcode
// 11c  which result would change a design, and which would not
```

## chapter 12 One picture for VMS

- put the chapters on the system we actually run
- browser and phone call nginx
- nginx talks to the Nest process
- Nest writes Postgres in a transaction
- Nest enqueues `laytime:` plus the port call id
- a worker runs the pure function
- Socket.IO tells the open screens
- Redis holds the queue and the rate limit. it is not the port-call table
- Sentry gets the exception. Prometheus gets the duration and the queue depth

```text
clients -> nginx -> Nest -> Postgres
                     |        ^
                     v        |
                   Redis -> worker
                     |
                  Socket.IO -> open screens
```

- Anemi removes Redis and the worker. the live path is the socket. the file path is FFmpeg and the playlist
- Bracket adds Stripe on the side, with the event id stored once
- we do not draw Kafka, a shard ring, or a CDN for the money number unless the envelope says the simple picture broke

- homework of chapter 12
```text
// 12a  redraw the VMS boxes from memory
// 12b  cross out the boxes Anemi does not have
// 12c  where the demurrage number is computed
// 12d  one number we would estimate before adding a second worker
```
