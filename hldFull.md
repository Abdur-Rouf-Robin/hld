# HLD

## Scalability

- Scalability is the ability to take more work by adding resources, without a rewrite.
- Vertical scale is a bigger machine. It is simple and it ends.
- Horizontal scale is more machines. The application must not keep the only copy of a session in local memory, or the next request may hit a machine that does not know the user.
- Stateless HTTP plus a shared database and Redis is how you add API machines.
- The database is usually the limit long before Node is.
- Vertical is a bigger pot. Horizontal is more pots. The soup has to be the kind you can split.
- careful: Putting sessions in a process-local Map and then adding a second process.
- What would you scale first on VMS?
- I would measure. If CPU on Node is hot and the SQL is fine, I add API processes behind nginx. If the SQL is hot, more Node makes it worse. I say the measurement before the drawing.

```
browser -> nginx -> any API process -> Postgres and Redis
```

- homework
```
browser -> nginx -> any API process -> Postgres and Redis
```

## Load balancing

- A load balancer spreads requests across healthy servers.
- Round robin is even. Least connections is better when some requests are slow.
- A health check removes a process that is not answering.
- Sticky sessions pin a user to one machine. They hide a state bug. Prefer a shared store.
- nginx in front of PM2 is this, on one machine or several.
- The balancer is a host at a restaurant who seats parties at tables that are actually staffed.
- careful: A health check that always returns 200.
- What is a health check?
- A request, often GET /health, that answers only when the process can also reach Postgres. A process that says 200 while the database is down should not receive traffic.

```
nginx upstream { server 127.0.0.1:3000; server 127.0.0.1:3001; }
```

- homework
```
nginx upstream { server 127.0.0.1:3000; server 127.0.0.1:3001; }
```

## Caching

- A cache holds a copy close to the reader. The hard part is knowing when the copy is a lie.
- Browser cache, CDN, Redis, and the database's own buffers are four different caches.
- Cache a read that is expensive and allowed to be a few seconds old.
- Invalidate on write, or set a short TTL.
- Do not cache a personal authorisation decision for minutes.
- A cache is a note on the fridge. When the shopping changes, throw the note away.
- careful: A cache with no expiry and no delete.
- Cache aside or write through?
- Cache aside: the application fills the cache on a miss. Write through: every write updates the cache too. I use cache aside with a delete on write, so I cannot forget to update a field.

```
key: port-call:42  ttl: 15 seconds  delete the key on PATCH
```

- homework
```
key: port-call:42  ttl: 15 seconds  delete the key on PATCH
```

## CDN

- A CDN stores static bytes in many cities so the user is not fetching a logo from another continent.
- HTML that is personal should not be cached as public.
- Hashed file names, app.a1b2.js, can be cached for a year because the name changes when the file changes.
- Video segments for HLS are a natural CDN fit.
- A CDN does not run your SQL.
- A CDN is copies of the poster in many towns. The ticket office, your API, is still one place.
- careful: Caching a logged-in HTML page on a shared CDN.
- Why is the old JavaScript still showing?
- The HTML was cached and still points at the old hashed file, or the HTML was not hashed and the browser kept it. I set HTML to revalidate and assets to immutable.

```http
app.a1b2.js  Cache-Control: public, max-age=31536000, immutable
index.html   Cache-Control: no-cache
```

- homework
```http
app.a1b2.js  Cache-Control: public, max-age=31536000, immutable
index.html   Cache-Control: no-cache
```

## Database sharding

- Sharding splits rows across database servers. You do it when one server cannot hold the writes.
- Choose a key that matches the query. Tournament id keeps one event together.
- Cross-shard queries become application code.
- Transactions across shards are a project, not a setting.
- Partitioning on one machine comes first.
- Several ledgers, and a rule for which ledger holds which customer. A question that does not name the customer opens every ledger.
- careful: Sharding by a key you do not have in the WHERE.
- Have you sharded production?
- No. I know the cost. I would partition by time and read replicas before I split the write path. I will not draw ten shards for a traffic number I have not measured.

```
one tournament, one shard
a user in many tournaments means a query per shard or a directory
```

- homework
```
one tournament, one shard
a user in many tournaments means a query per shard or a directory
```

## Replication

- A replica is a copy that follows the primary. It is for reads and for failover, not for fixing a bad write.
- Writes go to the primary. The primary ships the log.
- Reads can go to replicas if you tolerate lag.
- Promotion makes a replica the new primary. You must be sure the old primary cannot still accept writes. That is fencing.
- A replica copies mistakes too.
- A replica is an apprentice copying the ledger. If you write in both books, you have two truths.
- careful: Failover you have never rehearsed, described as if it were a button.
- What is split brain?
- Two nodes both believe they are the primary. They accept different writes. Fencing, a lock the old primary cannot renew, stops the old one.

```
writes -> primary -> WAL -> replica -> reads
```

- homework
```
writes -> primary -> WAL -> replica -> reads
```

## Consistent hashing

- Consistent hashing puts keys on a ring so adding a server moves only a fraction of the keys.
- Hash the key onto a circle. The server is the next mark clockwise.
- With ordinary modulo, adding a server remaps almost every key.
- Virtual nodes, many marks per server, keep the balance when servers differ in size.
- This is how caches and some shard maps avoid a total reshuffle.
- It is a clock. Each key sits at a minute. The next server mark owns it. Adding a mark takes only the minutes just behind it.
- careful: Forgetting virtual nodes and then one server owns half the ring.
- Why not hash % N?
- When N changes, almost every key changes bucket. Caches go cold at once. The ring moves only the keys that lived on the arc you handed to the new server.

```
ring position = hash(key)
server = next node clockwise
```

- homework
```
ring position = hash(key)
server = next node clockwise
```

## CAP theorem

- During a partition, when nodes cannot talk, you choose availability or consistency. You do not get both.
- C is every read seeing the latest write. A is every request getting a response. P is a cut in the network, which will happen.
- CP: refuse the write or the read rather than disagree. A bank balance leans this way.
- AP: answer anyway, and repair later. A hit counter can.
- When the network is healthy, you can have both. CAP is about the failure.
- CAP is the promise you break when the phone line is cut. You either stop speaking or you risk saying different things.
- careful: Saying we are CA. A real network partitions.
- Which one is laytime?
- The money number is CP in spirit. I would rather show an error than two different demurrage figures. A view counter on Anemi could be AP.

```
partition: the replica cannot hear the primary
CP: tell the client to wait
AP: serve the stale page
```

- homework
```
partition: the replica cannot hear the primary
CP: tell the client to wait
AP: serve the stale page
```

## PACELC

- PACELC adds the other half. If there is a partition, choose A or C. Else, even when healthy, choose latency or consistency.
- The E is 'else'. In normal operation a replica read is faster and slightly stale. That is latency over consistency.
- Waiting for every replica to confirm a write is consistency over latency.
- It stops the interview answer at 'we are CP' and asks what you do on a sunny day.
- Postgres synchronous commit waits for the standby. Asynchronous commit returns sooner and can lose the last transactions if the primary dies.
- CAP is the storm. PACELC also asks how you behave in good weather.
- careful: Reciting CAP and having nothing to say about replica lag on a normal day.
- How does this change a Postgres choice?
- Asynchronous replication is PA/EL in the loose telling: on a partition you may serve stale reads, and even when healthy you chose latency. Synchronous commit spends latency to know the standby has the write.

```
async replica: lower latency, a chance of lag
sync replica: higher latency, the standby has the commit
```

- homework
```
async replica: lower latency, a chance of lag
sync replica: higher latency, the standby has the commit
```

## Consistency models

- A consistency model is the promise about what a read returns after a write.
- Strong: the read sees the latest committed write. A primary read in Postgres.
- Eventual: if writes stop, replicas will agree. They may disagree now.
- Read-your-writes: this user sees their own update, even if others still see the old value.
- Name the one you need. A payment is strong. A view count can be eventual.
- Strong is one clock on the wall. Eventual is several clocks that drift and then catch up.
- careful: Saying eventual for a bank transfer because it 'scales'.
- What do you pick for a watch-together playhead?
- The room should see one order of events. That is closer to a single server or a log than to eventual replicas. A one-second disagreement in a playhead is a visible bug.

```
user saves a bracket
the next GET for that user hits the primary
other readers may hit a replica a second later
```

- homework
```
user saves a bracket
the next GET for that user hits the primary
other readers may hit a replica a second later
```

## Kafka and pub/sub

- Pub/sub is a publisher who does not know the subscribers. Kafka is a durable log that many groups can read.
- A topic is a named stream. A partition is an ordered slice of that topic.
- A consumer group splits the partitions. Two consumers in one group do not both get the same message.
- The log is kept, so a new consumer can replay.
- BullMQ is a job queue, not a replayable log. I say the difference. I do not call Redis a Kafka.
- A queue is a ticket destroyed when the meal is served. A log is a tape. Many people can listen, and you can rewind.
- careful: Using Kafka words for BullMQ. Know which one you operate.
- Queue or log?
- A queue deletes the job after one worker succeeds. A log keeps the event so a second service can read it later. Laytime today is a queue. An event other teams must replay would be a log.

```
topic laytime.calculated
group billing reads it
group notifications reads the same events
```

- homework
```
topic laytime.calculated
group billing reads it
group notifications reads the same events
```

## Event-driven architecture

- Services react to facts instead of calling each other for every step.
- The fact is past tense: PaymentReceived, LaytimeCalculated.
- The publisher does not know who cares.
- You need idempotent consumers, because delivery repeats.
- A chain of events is harder to debug. You need a correlation id in the log.
- An event is a notice on a board. A call is tapping someone on the shoulder and waiting.
- careful: Emitting an event and also depending on a secret order of listeners.
- When is a direct call better?
- When the user is waiting for the answer, such as 'is this login valid?'. Events are for work that can finish after the response, or for telling another system.

```
API commits the row
emits LaytimeCalculated { portCallId, correlationId }
worker and the socket gateway both hear it
```

- homework
```
API commits the row
emits LaytimeCalculated { portCallId, correlationId }
worker and the socket gateway both hear it
```

## Microservices

- A microservice is a separately deployed programme with its own data.
- Split on a business boundary you can name, not on a technical layer.
- Each service owns its tables. Another service does not reach into them.
- The cost is network, partial failure, and deployment. A modular monolith avoids that until the teams or the scale demand a split.
- VMS as modules inside one Nest API is a modular monolith. I will call it that.
- Microservices are separate shops. A modular monolith is one shop with labelled rooms. Do not build the mall first.
- careful: A 'microservice' that shares a database and calls five others for one click.
- Why not start with ten services?
- I would have ten deploys, ten failures, and no team to own them. Modules inside one process give me boundaries I can split later if a module has its own life.

```
one deployable API
modules: identity, port calls, laytime, billing
no shared tables across a future split
```

- homework
```
one deployable API
modules: identity, port calls, laytime, billing
no shared tables across a future split
```

## API gateway

- A gateway is the front door for clients. It routes, authenticates, and limits, and the services stay private.
- nginx already does routing and TLS for us.
- A gateway can check the JWT once and pass the user id inward.
- It should not contain laytime. Business rules behind the door rot when they live in the door.
- One public URL is easier for the Expo app than five origins.
- The gateway is the reception desk. It checks the appointment. It does not perform the surgery.
- careful: Two places that both think they own authorisation, and they disagree.
- What must not live in the gateway?
- The business rule and the database query. The gateway may reject a missing token. It should not decide demurrage.

```
app.example -> nginx -> web
app.example/api -> nginx -> nest
```

- homework
```
app.example -> nginx -> web
app.example/api -> nginx -> nest
```

## Rate limiter design

- A distributed rate limiter is a shared counter, because each API machine must see the same count.
- The key is IP or user, plus the route.
- Redis INCR is the shared counter. A limit inside one Node process is wrong once you have two processes.
- Token bucket: tokens refill over time, and a burst is allowed until the bucket is empty.
- Return 429. Log the key, not the password they were guessing.
- One tally board for every door, not a private tally in each guard's pocket.
- careful: An in-memory counter behind a load balancer.
- Fixed window or token bucket?
- Fixed window is what I would write first, and I can explain the burst at the boundary: 10 at the end of a minute and 10 at the start of the next. A token bucket smooths that. I name the weakness of the thing I shipped.

```
key login:ip
INCR
EXPIRE 60 on first hit
reject after 10
```

- homework
```
key login:ip
INCR
EXPIRE 60 on first hit
reject after 10
```

## Bloom filter

- A Bloom filter answers 'definitely not' or 'maybe' using a few bits. It never says 'definitely yes'.
- You hash the key several times and set those bits.
- If any bit is unset, the key was never added.
- If all bits are set, it might be a collision. You check the real store.
- It saves a database hit for things that are usually absent, such as 'have we seen this email'. You cannot delete easily from the basic version.
- A Bloom filter is a rumour mill that is allowed to say maybe, and is never allowed to forget someone who did arrive.
- careful: Treating a positive as proof.
- Can it tell me a user exists?
- It can tell me a user definitely does not. A 'yes' must be confirmed. I would not use it as the source of a login.

```
maybe in the filter? if no, skip the database
if yes, confirm in Postgres
```

- homework
```
maybe in the filter? if no, skip the database
if yes, confirm in Postgres
```

## Circuit breaker

- A circuit breaker stops calling a dependency that is already failing, so you do not pile timeouts on top of an outage.
- Closed: calls go through. Failures are counted.
- Open: calls fail immediately for a cool-off. The user gets a fast error.
- Half-open: one trial call. Success closes the breaker. Failure opens it again.
- Use it around Stripe or a remote encode service, not around your own pure function.
- It is a fuse. When the line is shorting, the fuse opens so the house does not burn. Later you test the line once.
- careful: Retrying forever with no backoff and no breaker.
- How is this different from a retry?
- A retry keeps hitting a sick service. A breaker stops hitting it, then tries once. I retry a timeout once. I open the breaker when the timeouts become the traffic.

```
5 failures in 10 seconds -> open for 30 seconds -> one trial
```

- homework
```
5 failures in 10 seconds -> open for 30 seconds -> one trial
```

## Saga pattern

- A saga is a long business action across several services, made of local transactions and compensations.
- There is no distributed lock that lasts for a human clicking through checkout.
- Each step commits locally. If a later step fails, you run a compensating action, such as a refund or a release of a seat.
- Orchestration is one conductor calling the steps. Choreography is events.
- Compensations must be idempotent too. A refund twice is its own bug.
- A saga is a trip with a plan for walking back each leg if a later leg is cancelled. It is not a time machine.
- careful: Holding a database transaction open while Stripe thinks.
- Is a Prisma transaction a saga?
- No. A database transaction is one ACID bracket. A saga is what you need when the steps are not in one database, such as our row plus Stripe. I do not wrap the Stripe HTTP call inside a database transaction.

```
take entry -> charge Stripe -> if charge fails, release the entry
```

- homework
```
take entry -> charge Stripe -> if charge fails, release the entry
```

## Blob storage

- Blob storage holds large bytes, videos and PDFs, outside the database.
- The database stores the key, the size, the type, and the owner.
- The object store stores the bytes.
- Access is a signed URL that expires, not a public bucket of private videos.
- Backups of the database do not include the blobs unless you copy those too.
- The database is the catalogue card. The warehouse holds the film can.
- careful: A public bucket for a private watch party.
- Why not a bytea column?
- A film does not belong in a row. Backups, replicas, and queries all carry it. The row should point at the object.

```
videos.key, videos.owner_id in Postgres
bytes in object storage
```

- homework
```
videos.key, videos.owner_id in Postgres
bytes in object storage
```

## Back-of-envelope estimation

- Estimation is arithmetic you can do on a board to see if a design is sane.
- Write the assumptions. Daily users, requests per user, size of a row.
- Turn them into per second. Divide by 100,000 for a rough peak if you do not have a better peak factor. Say the factor.
- Storage is rows times size times days, plus replication.
- The point is the unit, not a fake precise number.
- It is a napkin that shows you need a lorry or a pocket. It is not a quote.
- careful: A precise-looking number with no assumption beside it.
- How much storage for a year of port-call facts?
- I state the row size, say 200 bytes, and the count, say 1,000 facts a day. That is 200 KB a day, about 70 MB a year, before indexes and replicas. The number can be wrong. The method should not be.

```
50 tournaments a day
2000 reads each
100,000 reads a day
about 2 reads a second on average
a peak of 20 per second is still one Postgres
```

- homework
```
50 tournaments a day
2000 reads each
100,000 reads a day
about 2 reads a second on average
a peak of 20 per second is still one Postgres
```
