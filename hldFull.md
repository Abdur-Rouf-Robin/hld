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

- example 1. more traffic for port-calls
- vertical = a bigger machine. horizontal = another process
- a second Node process can serve GET /port-calls/pc_12 because the token is not stored in the process
- the second process does not make one hot row faster

```text
clients -> nginx -> node :3000
                 -> node :3001
both read Postgres for pc_12
```

- homework
```text
// 1a  horizontal step for port-calls
// 1b  the JWT for pc_12 works on either process
// 1c  a locked row is still one Postgres problem
```

- example 2. more traffic for matches
- vertical = a bigger machine. horizontal = another process
- a second Node process can serve GET /matches/m_1 because the token is not stored in the process
- the second process does not make one hot row faster

```text
clients -> nginx -> node :3000
                 -> node :3001
both read Postgres for m_1
```

- homework
```text
// 2a  horizontal step for matches
// 2b  the JWT for m_1 works on either process
// 2c  a locked row is still one Postgres problem
```

- example 3. more traffic for tournaments
- vertical = a bigger machine. horizontal = another process
- a second Node process can serve GET /tournaments/t_9 because the token is not stored in the process
- the second process does not make one hot row faster

```text
clients -> nginx -> node :3000
                 -> node :3001
both read Postgres for t_9
```

- homework
```text
// 3a  horizontal step for tournaments
// 3b  the JWT for t_9 works on either process
// 3c  a locked row is still one Postgres problem
```

- example 4. more traffic for videos
- vertical = a bigger machine. horizontal = another process
- a second Node process can serve GET /videos/v_4 because the token is not stored in the process
- the second process does not make one hot row faster

```text
clients -> nginx -> node :3000
                 -> node :3001
both read Postgres for v_4
```

- homework
```text
// 4a  horizontal step for videos
// 4b  the JWT for v_4 works on either process
// 4c  a locked row is still one Postgres problem
```

- example 5. more traffic for facts
- vertical = a bigger machine. horizontal = another process
- a second Node process can serve GET /facts/f_2 because the token is not stored in the process
- the second process does not make one hot row faster

```text
clients -> nginx -> node :3000
                 -> node :3001
both read Postgres for f_2
```

- homework
```text
// 5a  horizontal step for facts
// 5b  the JWT for f_2 works on either process
// 5c  a locked row is still one Postgres problem
```

- example 6. more traffic for users
- vertical = a bigger machine. horizontal = another process
- a second Node process can serve GET /users/u_3 because the token is not stored in the process
- the second process does not make one hot row faster

```text
clients -> nginx -> node :3000
                 -> node :3001
both read Postgres for u_3
```

- homework
```text
// 6a  horizontal step for users
// 6b  the JWT for u_3 works on either process
// 6c  a locked row is still one Postgres problem
```

- example 7. more traffic for payments
- vertical = a bigger machine. horizontal = another process
- a second Node process can serve GET /payments/pay_8 because the token is not stored in the process
- the second process does not make one hot row faster

```text
clients -> nginx -> node :3000
                 -> node :3001
both read Postgres for pay_8
```

- homework
```text
// 7a  horizontal step for payments
// 7b  the JWT for pay_8 works on either process
// 7c  a locked row is still one Postgres problem
```

- example 8. more traffic for rooms
- vertical = a bigger machine. horizontal = another process
- a second Node process can serve GET /rooms/room_2 because the token is not stored in the process
- the second process does not make one hot row faster

```text
clients -> nginx -> node :3000
                 -> node :3001
both read Postgres for room_2
```

- homework
```text
// 8a  horizontal step for rooms
// 8b  the JWT for room_2 works on either process
// 8c  a locked row is still one Postgres problem
```

- example 9. more traffic for vessels
- vertical = a bigger machine. horizontal = another process
- a second Node process can serve GET /vessels/vs_7 because the token is not stored in the process
- the second process does not make one hot row faster

```text
clients -> nginx -> node :3000
                 -> node :3001
both read Postgres for vs_7
```

- homework
```text
// 9a  horizontal step for vessels
// 9b  the JWT for vs_7 works on either process
// 9c  a locked row is still one Postgres problem
```

- example 10. more traffic for scores
- vertical = a bigger machine. horizontal = another process
- a second Node process can serve GET /scores/sc_3 because the token is not stored in the process
- the second process does not make one hot row faster

```text
clients -> nginx -> node :3000
                 -> node :3001
both read Postgres for sc_3
```

- homework
```text
// 10a  horizontal step for scores
// 10b  the JWT for sc_3 works on either process
// 10c  a locked row is still one Postgres problem
```

- example 11. more traffic for teams
- vertical = a bigger machine. horizontal = another process
- a second Node process can serve GET /teams/tm_2 because the token is not stored in the process
- the second process does not make one hot row faster

```text
clients -> nginx -> node :3000
                 -> node :3001
both read Postgres for tm_2
```

- homework
```text
// 11a  horizontal step for teams
// 11b  the JWT for tm_2 works on either process
// 11c  a locked row is still one Postgres problem
```

- example 12. more traffic for uploads
- vertical = a bigger machine. horizontal = another process
- a second Node process can serve GET /uploads/up_6 because the token is not stored in the process
- the second process does not make one hot row faster

```text
clients -> nginx -> node :3000
                 -> node :3001
both read Postgres for up_6
```

- homework
```text
// 12a  horizontal step for uploads
// 12b  the JWT for up_6 works on either process
// 12c  a locked row is still one Postgres problem
```

- example 13. more traffic for standings
- vertical = a bigger machine. horizontal = another process
- a second Node process can serve GET /standings/st_1 because the token is not stored in the process
- the second process does not make one hot row faster

```text
clients -> nginx -> node :3000
                 -> node :3001
both read Postgres for st_1
```

- homework
```text
// 13a  horizontal step for standings
// 13b  the JWT for st_1 works on either process
// 13c  a locked row is still one Postgres problem
```

- example 14. more traffic for notices
- vertical = a bigger machine. horizontal = another process
- a second Node process can serve GET /notices/n_2 because the token is not stored in the process
- the second process does not make one hot row faster

```text
clients -> nginx -> node :3000
                 -> node :3001
both read Postgres for n_2
```

- homework
```text
// 14a  horizontal step for notices
// 14b  the JWT for n_2 works on either process
// 14c  a locked row is still one Postgres problem
```

- example 15. more traffic for brackets
- vertical = a bigger machine. horizontal = another process
- a second Node process can serve GET /brackets/b_3 because the token is not stored in the process
- the second process does not make one hot row faster

```text
clients -> nginx -> node :3000
                 -> node :3001
both read Postgres for b_3
```

- homework
```text
// 15a  horizontal step for brackets
// 15b  the JWT for b_3 works on either process
// 15c  a locked row is still one Postgres problem
```

- example 16. more traffic for sessions
- vertical = a bigger machine. horizontal = another process
- a second Node process can serve GET /sessions/se_1 because the token is not stored in the process
- the second process does not make one hot row faster

```text
clients -> nginx -> node :3000
                 -> node :3001
both read Postgres for se_1
```

- homework
```text
// 16a  horizontal step for sessions
// 16b  the JWT for se_1 works on either process
// 16c  a locked row is still one Postgres problem
```

- example 17. more traffic for queues
- vertical = a bigger machine. horizontal = another process
- a second Node process can serve GET /queues/q_1 because the token is not stored in the process
- the second process does not make one hot row faster

```text
clients -> nginx -> node :3000
                 -> node :3001
both read Postgres for q_1
```

- homework
```text
// 17a  horizontal step for queues
// 17b  the JWT for q_1 works on either process
// 17c  a locked row is still one Postgres problem
```

- example 18. more traffic for roles
- vertical = a bigger machine. horizontal = another process
- a second Node process can serve GET /roles/r_1 because the token is not stored in the process
- the second process does not make one hot row faster

```text
clients -> nginx -> node :3000
                 -> node :3001
both read Postgres for r_1
```

- homework
```text
// 18a  horizontal step for roles
// 18b  the JWT for r_1 works on either process
// 18c  a locked row is still one Postgres problem
```

- example 19. more traffic for port-calls
- vertical = a bigger machine. horizontal = another process
- a second Node process can serve GET /port-calls/pc_12 because the token is not stored in the process
- the second process does not make one hot row faster

```text
clients -> nginx -> node :3000
                 -> node :3001
both read Postgres for pc_12
```

- homework
```text
// 19a  horizontal step for port-calls
// 19b  the JWT for pc_12 works on either process
// 19c  a locked row is still one Postgres problem
```

- example 20. more traffic for matches
- vertical = a bigger machine. horizontal = another process
- a second Node process can serve GET /matches/m_1 because the token is not stored in the process
- the second process does not make one hot row faster

```text
clients -> nginx -> node :3000
                 -> node :3001
both read Postgres for m_1
```

- homework
```text
// 20a  horizontal step for matches
// 20b  the JWT for m_1 works on either process
// 20c  a locked row is still one Postgres problem
```

- example 21. more traffic for tournaments
- vertical = a bigger machine. horizontal = another process
- a second Node process can serve GET /tournaments/t_9 because the token is not stored in the process
- the second process does not make one hot row faster

```text
clients -> nginx -> node :3000
                 -> node :3001
both read Postgres for t_9
```

- homework
```text
// 21a  horizontal step for tournaments
// 21b  the JWT for t_9 works on either process
// 21c  a locked row is still one Postgres problem
```

- example 22. more traffic for videos
- vertical = a bigger machine. horizontal = another process
- a second Node process can serve GET /videos/v_4 because the token is not stored in the process
- the second process does not make one hot row faster

```text
clients -> nginx -> node :3000
                 -> node :3001
both read Postgres for v_4
```

- homework
```text
// 22a  horizontal step for videos
// 22b  the JWT for v_4 works on either process
// 22c  a locked row is still one Postgres problem
```

- example 23. more traffic for facts
- vertical = a bigger machine. horizontal = another process
- a second Node process can serve GET /facts/f_2 because the token is not stored in the process
- the second process does not make one hot row faster

```text
clients -> nginx -> node :3000
                 -> node :3001
both read Postgres for f_2
```

- homework
```text
// 23a  horizontal step for facts
// 23b  the JWT for f_2 works on either process
// 23c  a locked row is still one Postgres problem
```

- example 24. more traffic for users
- vertical = a bigger machine. horizontal = another process
- a second Node process can serve GET /users/u_3 because the token is not stored in the process
- the second process does not make one hot row faster

```text
clients -> nginx -> node :3000
                 -> node :3001
both read Postgres for u_3
```

- homework
```text
// 24a  horizontal step for users
// 24b  the JWT for u_3 works on either process
// 24c  a locked row is still one Postgres problem
```

- example 25. more traffic for payments
- vertical = a bigger machine. horizontal = another process
- a second Node process can serve GET /payments/pay_8 because the token is not stored in the process
- the second process does not make one hot row faster

```text
clients -> nginx -> node :3000
                 -> node :3001
both read Postgres for pay_8
```

- homework
```text
// 25a  horizontal step for payments
// 25b  the JWT for pay_8 works on either process
// 25c  a locked row is still one Postgres problem
```

- example 26. more traffic for rooms
- vertical = a bigger machine. horizontal = another process
- a second Node process can serve GET /rooms/room_2 because the token is not stored in the process
- the second process does not make one hot row faster

```text
clients -> nginx -> node :3000
                 -> node :3001
both read Postgres for room_2
```

- homework
```text
// 26a  horizontal step for rooms
// 26b  the JWT for room_2 works on either process
// 26c  a locked row is still one Postgres problem
```

- example 27. more traffic for vessels
- vertical = a bigger machine. horizontal = another process
- a second Node process can serve GET /vessels/vs_7 because the token is not stored in the process
- the second process does not make one hot row faster

```text
clients -> nginx -> node :3000
                 -> node :3001
both read Postgres for vs_7
```

- homework
```text
// 27a  horizontal step for vessels
// 27b  the JWT for vs_7 works on either process
// 27c  a locked row is still one Postgres problem
```

- example 28. more traffic for scores
- vertical = a bigger machine. horizontal = another process
- a second Node process can serve GET /scores/sc_3 because the token is not stored in the process
- the second process does not make one hot row faster

```text
clients -> nginx -> node :3000
                 -> node :3001
both read Postgres for sc_3
```

- homework
```text
// 28a  horizontal step for scores
// 28b  the JWT for sc_3 works on either process
// 28c  a locked row is still one Postgres problem
```

- example 29. more traffic for teams
- vertical = a bigger machine. horizontal = another process
- a second Node process can serve GET /teams/tm_2 because the token is not stored in the process
- the second process does not make one hot row faster

```text
clients -> nginx -> node :3000
                 -> node :3001
both read Postgres for tm_2
```

- homework
```text
// 29a  horizontal step for teams
// 29b  the JWT for tm_2 works on either process
// 29c  a locked row is still one Postgres problem
```

- example 30. more traffic for uploads
- vertical = a bigger machine. horizontal = another process
- a second Node process can serve GET /uploads/up_6 because the token is not stored in the process
- the second process does not make one hot row faster

```text
clients -> nginx -> node :3000
                 -> node :3001
both read Postgres for up_6
```

- homework
```text
// 30a  horizontal step for uploads
// 30b  the JWT for up_6 works on either process
// 30c  a locked row is still one Postgres problem
```

- example 31. more traffic for standings
- vertical = a bigger machine. horizontal = another process
- a second Node process can serve GET /standings/st_1 because the token is not stored in the process
- the second process does not make one hot row faster

```text
clients -> nginx -> node :3000
                 -> node :3001
both read Postgres for st_1
```

- homework
```text
// 31a  horizontal step for standings
// 31b  the JWT for st_1 works on either process
// 31c  a locked row is still one Postgres problem
```

- example 32. more traffic for notices
- vertical = a bigger machine. horizontal = another process
- a second Node process can serve GET /notices/n_2 because the token is not stored in the process
- the second process does not make one hot row faster

```text
clients -> nginx -> node :3000
                 -> node :3001
both read Postgres for n_2
```

- homework
```text
// 32a  horizontal step for notices
// 32b  the JWT for n_2 works on either process
// 32c  a locked row is still one Postgres problem
```

- example 33. more traffic for brackets
- vertical = a bigger machine. horizontal = another process
- a second Node process can serve GET /brackets/b_3 because the token is not stored in the process
- the second process does not make one hot row faster

```text
clients -> nginx -> node :3000
                 -> node :3001
both read Postgres for b_3
```

- homework
```text
// 33a  horizontal step for brackets
// 33b  the JWT for b_3 works on either process
// 33c  a locked row is still one Postgres problem
```

- example 34. more traffic for sessions
- vertical = a bigger machine. horizontal = another process
- a second Node process can serve GET /sessions/se_1 because the token is not stored in the process
- the second process does not make one hot row faster

```text
clients -> nginx -> node :3000
                 -> node :3001
both read Postgres for se_1
```

- homework
```text
// 34a  horizontal step for sessions
// 34b  the JWT for se_1 works on either process
// 34c  a locked row is still one Postgres problem
```

- example 35. more traffic for queues
- vertical = a bigger machine. horizontal = another process
- a second Node process can serve GET /queues/q_1 because the token is not stored in the process
- the second process does not make one hot row faster

```text
clients -> nginx -> node :3000
                 -> node :3001
both read Postgres for q_1
```

- homework
```text
// 35a  horizontal step for queues
// 35b  the JWT for q_1 works on either process
// 35c  a locked row is still one Postgres problem
```

- example 36. more traffic for roles
- vertical = a bigger machine. horizontal = another process
- a second Node process can serve GET /roles/r_1 because the token is not stored in the process
- the second process does not make one hot row faster

```text
clients -> nginx -> node :3000
                 -> node :3001
both read Postgres for r_1
```

- homework
```text
// 36a  horizontal step for roles
// 36b  the JWT for r_1 works on either process
// 36c  a locked row is still one Postgres problem
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

- example 1. upstreams for port-calls
- round robin takes turns. least connections prefers the quieter process
- a dead upstream is removed by the health check
- sticky cookies are unnecessary when the session is the JWT
- both upstreams down means 502

```nginx
upstream api {
  server 127.0.0.1:3000;
  server 127.0.0.1:3001;
}
location /port-calls/ {
  proxy_pass http://api;
}
```

- homework
```text
// 1a  two upstreams, one URL /port-calls/pc_12
// 1b  a long socket should not be killed as idle too fast
// 1c  502 when nobody is left for pc_12
```

- example 2. upstreams for matches
- round robin takes turns. least connections prefers the quieter process
- a dead upstream is removed by the health check
- sticky cookies are unnecessary when the session is the JWT
- both upstreams down means 502

```nginx
upstream api {
  server 127.0.0.1:3000;
  server 127.0.0.1:3001;
}
location /matches/ {
  proxy_pass http://api;
}
```

- homework
```text
// 2a  two upstreams, one URL /matches/m_1
// 2b  a long socket should not be killed as idle too fast
// 2c  502 when nobody is left for m_1
```

- example 3. upstreams for tournaments
- round robin takes turns. least connections prefers the quieter process
- a dead upstream is removed by the health check
- sticky cookies are unnecessary when the session is the JWT
- both upstreams down means 502

```nginx
upstream api {
  server 127.0.0.1:3000;
  server 127.0.0.1:3001;
}
location /tournaments/ {
  proxy_pass http://api;
}
```

- homework
```text
// 3a  two upstreams, one URL /tournaments/t_9
// 3b  a long socket should not be killed as idle too fast
// 3c  502 when nobody is left for t_9
```

- example 4. upstreams for videos
- round robin takes turns. least connections prefers the quieter process
- a dead upstream is removed by the health check
- sticky cookies are unnecessary when the session is the JWT
- both upstreams down means 502

```nginx
upstream api {
  server 127.0.0.1:3000;
  server 127.0.0.1:3001;
}
location /videos/ {
  proxy_pass http://api;
}
```

- homework
```text
// 4a  two upstreams, one URL /videos/v_4
// 4b  a long socket should not be killed as idle too fast
// 4c  502 when nobody is left for v_4
```

- example 5. upstreams for facts
- round robin takes turns. least connections prefers the quieter process
- a dead upstream is removed by the health check
- sticky cookies are unnecessary when the session is the JWT
- both upstreams down means 502

```nginx
upstream api {
  server 127.0.0.1:3000;
  server 127.0.0.1:3001;
}
location /facts/ {
  proxy_pass http://api;
}
```

- homework
```text
// 5a  two upstreams, one URL /facts/f_2
// 5b  a long socket should not be killed as idle too fast
// 5c  502 when nobody is left for f_2
```

- example 6. upstreams for users
- round robin takes turns. least connections prefers the quieter process
- a dead upstream is removed by the health check
- sticky cookies are unnecessary when the session is the JWT
- both upstreams down means 502

```nginx
upstream api {
  server 127.0.0.1:3000;
  server 127.0.0.1:3001;
}
location /users/ {
  proxy_pass http://api;
}
```

- homework
```text
// 6a  two upstreams, one URL /users/u_3
// 6b  a long socket should not be killed as idle too fast
// 6c  502 when nobody is left for u_3
```

- example 7. upstreams for payments
- round robin takes turns. least connections prefers the quieter process
- a dead upstream is removed by the health check
- sticky cookies are unnecessary when the session is the JWT
- both upstreams down means 502

```nginx
upstream api {
  server 127.0.0.1:3000;
  server 127.0.0.1:3001;
}
location /payments/ {
  proxy_pass http://api;
}
```

- homework
```text
// 7a  two upstreams, one URL /payments/pay_8
// 7b  a long socket should not be killed as idle too fast
// 7c  502 when nobody is left for pay_8
```

- example 8. upstreams for rooms
- round robin takes turns. least connections prefers the quieter process
- a dead upstream is removed by the health check
- sticky cookies are unnecessary when the session is the JWT
- both upstreams down means 502

```nginx
upstream api {
  server 127.0.0.1:3000;
  server 127.0.0.1:3001;
}
location /rooms/ {
  proxy_pass http://api;
}
```

- homework
```text
// 8a  two upstreams, one URL /rooms/room_2
// 8b  a long socket should not be killed as idle too fast
// 8c  502 when nobody is left for room_2
```

- example 9. upstreams for vessels
- round robin takes turns. least connections prefers the quieter process
- a dead upstream is removed by the health check
- sticky cookies are unnecessary when the session is the JWT
- both upstreams down means 502

```nginx
upstream api {
  server 127.0.0.1:3000;
  server 127.0.0.1:3001;
}
location /vessels/ {
  proxy_pass http://api;
}
```

- homework
```text
// 9a  two upstreams, one URL /vessels/vs_7
// 9b  a long socket should not be killed as idle too fast
// 9c  502 when nobody is left for vs_7
```

- example 10. upstreams for scores
- round robin takes turns. least connections prefers the quieter process
- a dead upstream is removed by the health check
- sticky cookies are unnecessary when the session is the JWT
- both upstreams down means 502

```nginx
upstream api {
  server 127.0.0.1:3000;
  server 127.0.0.1:3001;
}
location /scores/ {
  proxy_pass http://api;
}
```

- homework
```text
// 10a  two upstreams, one URL /scores/sc_3
// 10b  a long socket should not be killed as idle too fast
// 10c  502 when nobody is left for sc_3
```

- example 11. upstreams for teams
- round robin takes turns. least connections prefers the quieter process
- a dead upstream is removed by the health check
- sticky cookies are unnecessary when the session is the JWT
- both upstreams down means 502

```nginx
upstream api {
  server 127.0.0.1:3000;
  server 127.0.0.1:3001;
}
location /teams/ {
  proxy_pass http://api;
}
```

- homework
```text
// 11a  two upstreams, one URL /teams/tm_2
// 11b  a long socket should not be killed as idle too fast
// 11c  502 when nobody is left for tm_2
```

- example 12. upstreams for uploads
- round robin takes turns. least connections prefers the quieter process
- a dead upstream is removed by the health check
- sticky cookies are unnecessary when the session is the JWT
- both upstreams down means 502

```nginx
upstream api {
  server 127.0.0.1:3000;
  server 127.0.0.1:3001;
}
location /uploads/ {
  proxy_pass http://api;
}
```

- homework
```text
// 12a  two upstreams, one URL /uploads/up_6
// 12b  a long socket should not be killed as idle too fast
// 12c  502 when nobody is left for up_6
```

- example 13. upstreams for standings
- round robin takes turns. least connections prefers the quieter process
- a dead upstream is removed by the health check
- sticky cookies are unnecessary when the session is the JWT
- both upstreams down means 502

```nginx
upstream api {
  server 127.0.0.1:3000;
  server 127.0.0.1:3001;
}
location /standings/ {
  proxy_pass http://api;
}
```

- homework
```text
// 13a  two upstreams, one URL /standings/st_1
// 13b  a long socket should not be killed as idle too fast
// 13c  502 when nobody is left for st_1
```

- example 14. upstreams for notices
- round robin takes turns. least connections prefers the quieter process
- a dead upstream is removed by the health check
- sticky cookies are unnecessary when the session is the JWT
- both upstreams down means 502

```nginx
upstream api {
  server 127.0.0.1:3000;
  server 127.0.0.1:3001;
}
location /notices/ {
  proxy_pass http://api;
}
```

- homework
```text
// 14a  two upstreams, one URL /notices/n_2
// 14b  a long socket should not be killed as idle too fast
// 14c  502 when nobody is left for n_2
```

- example 15. upstreams for brackets
- round robin takes turns. least connections prefers the quieter process
- a dead upstream is removed by the health check
- sticky cookies are unnecessary when the session is the JWT
- both upstreams down means 502

```nginx
upstream api {
  server 127.0.0.1:3000;
  server 127.0.0.1:3001;
}
location /brackets/ {
  proxy_pass http://api;
}
```

- homework
```text
// 15a  two upstreams, one URL /brackets/b_3
// 15b  a long socket should not be killed as idle too fast
// 15c  502 when nobody is left for b_3
```

- example 16. upstreams for sessions
- round robin takes turns. least connections prefers the quieter process
- a dead upstream is removed by the health check
- sticky cookies are unnecessary when the session is the JWT
- both upstreams down means 502

```nginx
upstream api {
  server 127.0.0.1:3000;
  server 127.0.0.1:3001;
}
location /sessions/ {
  proxy_pass http://api;
}
```

- homework
```text
// 16a  two upstreams, one URL /sessions/se_1
// 16b  a long socket should not be killed as idle too fast
// 16c  502 when nobody is left for se_1
```

- example 17. upstreams for queues
- round robin takes turns. least connections prefers the quieter process
- a dead upstream is removed by the health check
- sticky cookies are unnecessary when the session is the JWT
- both upstreams down means 502

```nginx
upstream api {
  server 127.0.0.1:3000;
  server 127.0.0.1:3001;
}
location /queues/ {
  proxy_pass http://api;
}
```

- homework
```text
// 17a  two upstreams, one URL /queues/q_1
// 17b  a long socket should not be killed as idle too fast
// 17c  502 when nobody is left for q_1
```

- example 18. upstreams for roles
- round robin takes turns. least connections prefers the quieter process
- a dead upstream is removed by the health check
- sticky cookies are unnecessary when the session is the JWT
- both upstreams down means 502

```nginx
upstream api {
  server 127.0.0.1:3000;
  server 127.0.0.1:3001;
}
location /roles/ {
  proxy_pass http://api;
}
```

- homework
```text
// 18a  two upstreams, one URL /roles/r_1
// 18b  a long socket should not be killed as idle too fast
// 18c  502 when nobody is left for r_1
```

- example 19. upstreams for port-calls
- round robin takes turns. least connections prefers the quieter process
- a dead upstream is removed by the health check
- sticky cookies are unnecessary when the session is the JWT
- both upstreams down means 502

```nginx
upstream api {
  server 127.0.0.1:3000;
  server 127.0.0.1:3001;
}
location /port-calls/ {
  proxy_pass http://api;
}
```

- homework
```text
// 19a  two upstreams, one URL /port-calls/pc_12
// 19b  a long socket should not be killed as idle too fast
// 19c  502 when nobody is left for pc_12
```

- example 20. upstreams for matches
- round robin takes turns. least connections prefers the quieter process
- a dead upstream is removed by the health check
- sticky cookies are unnecessary when the session is the JWT
- both upstreams down means 502

```nginx
upstream api {
  server 127.0.0.1:3000;
  server 127.0.0.1:3001;
}
location /matches/ {
  proxy_pass http://api;
}
```

- homework
```text
// 20a  two upstreams, one URL /matches/m_1
// 20b  a long socket should not be killed as idle too fast
// 20c  502 when nobody is left for m_1
```

- example 21. upstreams for tournaments
- round robin takes turns. least connections prefers the quieter process
- a dead upstream is removed by the health check
- sticky cookies are unnecessary when the session is the JWT
- both upstreams down means 502

```nginx
upstream api {
  server 127.0.0.1:3000;
  server 127.0.0.1:3001;
}
location /tournaments/ {
  proxy_pass http://api;
}
```

- homework
```text
// 21a  two upstreams, one URL /tournaments/t_9
// 21b  a long socket should not be killed as idle too fast
// 21c  502 when nobody is left for t_9
```

- example 22. upstreams for videos
- round robin takes turns. least connections prefers the quieter process
- a dead upstream is removed by the health check
- sticky cookies are unnecessary when the session is the JWT
- both upstreams down means 502

```nginx
upstream api {
  server 127.0.0.1:3000;
  server 127.0.0.1:3001;
}
location /videos/ {
  proxy_pass http://api;
}
```

- homework
```text
// 22a  two upstreams, one URL /videos/v_4
// 22b  a long socket should not be killed as idle too fast
// 22c  502 when nobody is left for v_4
```

- example 23. upstreams for facts
- round robin takes turns. least connections prefers the quieter process
- a dead upstream is removed by the health check
- sticky cookies are unnecessary when the session is the JWT
- both upstreams down means 502

```nginx
upstream api {
  server 127.0.0.1:3000;
  server 127.0.0.1:3001;
}
location /facts/ {
  proxy_pass http://api;
}
```

- homework
```text
// 23a  two upstreams, one URL /facts/f_2
// 23b  a long socket should not be killed as idle too fast
// 23c  502 when nobody is left for f_2
```

- example 24. upstreams for users
- round robin takes turns. least connections prefers the quieter process
- a dead upstream is removed by the health check
- sticky cookies are unnecessary when the session is the JWT
- both upstreams down means 502

```nginx
upstream api {
  server 127.0.0.1:3000;
  server 127.0.0.1:3001;
}
location /users/ {
  proxy_pass http://api;
}
```

- homework
```text
// 24a  two upstreams, one URL /users/u_3
// 24b  a long socket should not be killed as idle too fast
// 24c  502 when nobody is left for u_3
```

- example 25. upstreams for payments
- round robin takes turns. least connections prefers the quieter process
- a dead upstream is removed by the health check
- sticky cookies are unnecessary when the session is the JWT
- both upstreams down means 502

```nginx
upstream api {
  server 127.0.0.1:3000;
  server 127.0.0.1:3001;
}
location /payments/ {
  proxy_pass http://api;
}
```

- homework
```text
// 25a  two upstreams, one URL /payments/pay_8
// 25b  a long socket should not be killed as idle too fast
// 25c  502 when nobody is left for pay_8
```

- example 26. upstreams for rooms
- round robin takes turns. least connections prefers the quieter process
- a dead upstream is removed by the health check
- sticky cookies are unnecessary when the session is the JWT
- both upstreams down means 502

```nginx
upstream api {
  server 127.0.0.1:3000;
  server 127.0.0.1:3001;
}
location /rooms/ {
  proxy_pass http://api;
}
```

- homework
```text
// 26a  two upstreams, one URL /rooms/room_2
// 26b  a long socket should not be killed as idle too fast
// 26c  502 when nobody is left for room_2
```

- example 27. upstreams for vessels
- round robin takes turns. least connections prefers the quieter process
- a dead upstream is removed by the health check
- sticky cookies are unnecessary when the session is the JWT
- both upstreams down means 502

```nginx
upstream api {
  server 127.0.0.1:3000;
  server 127.0.0.1:3001;
}
location /vessels/ {
  proxy_pass http://api;
}
```

- homework
```text
// 27a  two upstreams, one URL /vessels/vs_7
// 27b  a long socket should not be killed as idle too fast
// 27c  502 when nobody is left for vs_7
```

- example 28. upstreams for scores
- round robin takes turns. least connections prefers the quieter process
- a dead upstream is removed by the health check
- sticky cookies are unnecessary when the session is the JWT
- both upstreams down means 502

```nginx
upstream api {
  server 127.0.0.1:3000;
  server 127.0.0.1:3001;
}
location /scores/ {
  proxy_pass http://api;
}
```

- homework
```text
// 28a  two upstreams, one URL /scores/sc_3
// 28b  a long socket should not be killed as idle too fast
// 28c  502 when nobody is left for sc_3
```

- example 29. upstreams for teams
- round robin takes turns. least connections prefers the quieter process
- a dead upstream is removed by the health check
- sticky cookies are unnecessary when the session is the JWT
- both upstreams down means 502

```nginx
upstream api {
  server 127.0.0.1:3000;
  server 127.0.0.1:3001;
}
location /teams/ {
  proxy_pass http://api;
}
```

- homework
```text
// 29a  two upstreams, one URL /teams/tm_2
// 29b  a long socket should not be killed as idle too fast
// 29c  502 when nobody is left for tm_2
```

- example 30. upstreams for uploads
- round robin takes turns. least connections prefers the quieter process
- a dead upstream is removed by the health check
- sticky cookies are unnecessary when the session is the JWT
- both upstreams down means 502

```nginx
upstream api {
  server 127.0.0.1:3000;
  server 127.0.0.1:3001;
}
location /uploads/ {
  proxy_pass http://api;
}
```

- homework
```text
// 30a  two upstreams, one URL /uploads/up_6
// 30b  a long socket should not be killed as idle too fast
// 30c  502 when nobody is left for up_6
```

- example 31. upstreams for standings
- round robin takes turns. least connections prefers the quieter process
- a dead upstream is removed by the health check
- sticky cookies are unnecessary when the session is the JWT
- both upstreams down means 502

```nginx
upstream api {
  server 127.0.0.1:3000;
  server 127.0.0.1:3001;
}
location /standings/ {
  proxy_pass http://api;
}
```

- homework
```text
// 31a  two upstreams, one URL /standings/st_1
// 31b  a long socket should not be killed as idle too fast
// 31c  502 when nobody is left for st_1
```

- example 32. upstreams for notices
- round robin takes turns. least connections prefers the quieter process
- a dead upstream is removed by the health check
- sticky cookies are unnecessary when the session is the JWT
- both upstreams down means 502

```nginx
upstream api {
  server 127.0.0.1:3000;
  server 127.0.0.1:3001;
}
location /notices/ {
  proxy_pass http://api;
}
```

- homework
```text
// 32a  two upstreams, one URL /notices/n_2
// 32b  a long socket should not be killed as idle too fast
// 32c  502 when nobody is left for n_2
```

- example 33. upstreams for brackets
- round robin takes turns. least connections prefers the quieter process
- a dead upstream is removed by the health check
- sticky cookies are unnecessary when the session is the JWT
- both upstreams down means 502

```nginx
upstream api {
  server 127.0.0.1:3000;
  server 127.0.0.1:3001;
}
location /brackets/ {
  proxy_pass http://api;
}
```

- homework
```text
// 33a  two upstreams, one URL /brackets/b_3
// 33b  a long socket should not be killed as idle too fast
// 33c  502 when nobody is left for b_3
```

- example 34. upstreams for sessions
- round robin takes turns. least connections prefers the quieter process
- a dead upstream is removed by the health check
- sticky cookies are unnecessary when the session is the JWT
- both upstreams down means 502

```nginx
upstream api {
  server 127.0.0.1:3000;
  server 127.0.0.1:3001;
}
location /sessions/ {
  proxy_pass http://api;
}
```

- homework
```text
// 34a  two upstreams, one URL /sessions/se_1
// 34b  a long socket should not be killed as idle too fast
// 34c  502 when nobody is left for se_1
```

- example 35. upstreams for queues
- round robin takes turns. least connections prefers the quieter process
- a dead upstream is removed by the health check
- sticky cookies are unnecessary when the session is the JWT
- both upstreams down means 502

```nginx
upstream api {
  server 127.0.0.1:3000;
  server 127.0.0.1:3001;
}
location /queues/ {
  proxy_pass http://api;
}
```

- homework
```text
// 35a  two upstreams, one URL /queues/q_1
// 35b  a long socket should not be killed as idle too fast
// 35c  502 when nobody is left for q_1
```

- example 36. upstreams for roles
- round robin takes turns. least connections prefers the quieter process
- a dead upstream is removed by the health check
- sticky cookies are unnecessary when the session is the JWT
- both upstreams down means 502

```nginx
upstream api {
  server 127.0.0.1:3000;
  server 127.0.0.1:3001;
}
location /roles/ {
  proxy_pass http://api;
}
```

- homework
```text
// 36a  two upstreams, one URL /roles/r_1
// 36b  a long socket should not be killed as idle too fast
// 36c  502 when nobody is left for r_1
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

- example 1. cache key port-calls:pc_12
- value Ocean Star, TTL 60 seconds
- delete the key when vesselName changes
- a public segment can sit on a CDN. the money figure for pc_12 should not
- Anemi does not use Redis. VMS and Bracket do

```ts
const key = "port-calls:pc_12";
const hit = await redis.get(key);
if (!hit) {
  const row = await db.item.findUniqueOrThrow({ where: { id: "pc_12" } });
  await redis.set(key, row.vesselName, "EX", 60);
}
```

- homework
```ts
// 1a  key for pc_12
// 1b  PATCH vesselName deletes the key
// 1c  do not cache "may this user edit pc_12" for a long TTL
```

- example 2. cache key matches:m_1
- value Ada, TTL 60 seconds
- delete the key when home changes
- a public segment can sit on a CDN. the money figure for m_1 should not
- Anemi does not use Redis. VMS and Bracket do

```ts
const key = "matches:m_1";
const hit = await redis.get(key);
if (!hit) {
  const row = await db.item.findUniqueOrThrow({ where: { id: "m_1" } });
  await redis.set(key, row.home, "EX", 60);
}
```

- homework
```ts
// 2a  key for m_1
// 2b  PATCH home deletes the key
// 2c  do not cache "may this user edit m_1" for a long TTL
```

- example 3. cache key tournaments:t_9
- value Rajshahi Open, TTL 60 seconds
- delete the key when name changes
- a public segment can sit on a CDN. the money figure for t_9 should not
- Anemi does not use Redis. VMS and Bracket do

```ts
const key = "tournaments:t_9";
const hit = await redis.get(key);
if (!hit) {
  const row = await db.item.findUniqueOrThrow({ where: { id: "t_9" } });
  await redis.set(key, row.name, "EX", 60);
}
```

- homework
```ts
// 3a  key for t_9
// 3b  PATCH name deletes the key
// 3c  do not cache "may this user edit t_9" for a long TTL
```

- example 4. cache key videos:v_4
- value Episode 1, TTL 60 seconds
- delete the key when title changes
- a public segment can sit on a CDN. the money figure for v_4 should not
- Anemi does not use Redis. VMS and Bracket do

```ts
const key = "videos:v_4";
const hit = await redis.get(key);
if (!hit) {
  const row = await db.item.findUniqueOrThrow({ where: { id: "v_4" } });
  await redis.set(key, row.title, "EX", 60);
}
```

- homework
```ts
// 4a  key for v_4
// 4b  PATCH title deletes the key
// 4c  do not cache "may this user edit v_4" for a long TTL
```

- example 5. cache key facts:f_2
- value NOR, TTL 60 seconds
- delete the key when kind changes
- a public segment can sit on a CDN. the money figure for f_2 should not
- Anemi does not use Redis. VMS and Bracket do

```ts
const key = "facts:f_2";
const hit = await redis.get(key);
if (!hit) {
  const row = await db.item.findUniqueOrThrow({ where: { id: "f_2" } });
  await redis.set(key, row.kind, "EX", 60);
}
```

- homework
```ts
// 5a  key for f_2
// 5b  PATCH kind deletes the key
// 5c  do not cache "may this user edit f_2" for a long TTL
```

- example 6. cache key users:u_3
- value a@example.com, TTL 60 seconds
- delete the key when email changes
- a public segment can sit on a CDN. the money figure for u_3 should not
- Anemi does not use Redis. VMS and Bracket do

```ts
const key = "users:u_3";
const hit = await redis.get(key);
if (!hit) {
  const row = await db.item.findUniqueOrThrow({ where: { id: "u_3" } });
  await redis.set(key, row.email, "EX", 60);
}
```

- homework
```ts
// 6a  key for u_3
// 6b  PATCH email deletes the key
// 6c  do not cache "may this user edit u_3" for a long TTL
```

- example 7. cache key payments:pay_8
- value evt_1, TTL 60 seconds
- delete the key when eventId changes
- a public segment can sit on a CDN. the money figure for pay_8 should not
- Anemi does not use Redis. VMS and Bracket do

```ts
const key = "payments:pay_8";
const hit = await redis.get(key);
if (!hit) {
  const row = await db.item.findUniqueOrThrow({ where: { id: "pay_8" } });
  await redis.set(key, row.eventId, "EX", 60);
}
```

- homework
```ts
// 7a  key for pay_8
// 7b  PATCH eventId deletes the key
// 7c  do not cache "may this user edit pay_8" for a long TTL
```

- example 8. cache key rooms:room_2
- value s_9, TTL 60 seconds
- delete the key when sessionId changes
- a public segment can sit on a CDN. the money figure for room_2 should not
- Anemi does not use Redis. VMS and Bracket do

```ts
const key = "rooms:room_2";
const hit = await redis.get(key);
if (!hit) {
  const row = await db.item.findUniqueOrThrow({ where: { id: "room_2" } });
  await redis.set(key, row.sessionId, "EX", 60);
}
```

- homework
```ts
// 8a  key for room_2
// 8b  PATCH sessionId deletes the key
// 8c  do not cache "may this user edit room_2" for a long TTL
```

- example 9. cache key vessels:vs_7
- value North Wind, TTL 60 seconds
- delete the key when name changes
- a public segment can sit on a CDN. the money figure for vs_7 should not
- Anemi does not use Redis. VMS and Bracket do

```ts
const key = "vessels:vs_7";
const hit = await redis.get(key);
if (!hit) {
  const row = await db.item.findUniqueOrThrow({ where: { id: "vs_7" } });
  await redis.set(key, row.name, "EX", 60);
}
```

- homework
```ts
// 9a  key for vs_7
// 9b  PATCH name deletes the key
// 9c  do not cache "may this user edit vs_7" for a long TTL
```

- example 10. cache key scores:sc_3
- value 11, TTL 60 seconds
- delete the key when points changes
- a public segment can sit on a CDN. the money figure for sc_3 should not
- Anemi does not use Redis. VMS and Bracket do

```ts
const key = "scores:sc_3";
const hit = await redis.get(key);
if (!hit) {
  const row = await db.item.findUniqueOrThrow({ where: { id: "sc_3" } });
  await redis.set(key, row.points, "EX", 60);
}
```

- homework
```ts
// 10a  key for sc_3
// 10b  PATCH points deletes the key
// 10c  do not cache "may this user edit sc_3" for a long TTL
```

- example 11. cache key teams:tm_2
- value Blue, TTL 60 seconds
- delete the key when name changes
- a public segment can sit on a CDN. the money figure for tm_2 should not
- Anemi does not use Redis. VMS and Bracket do

```ts
const key = "teams:tm_2";
const hit = await redis.get(key);
if (!hit) {
  const row = await db.item.findUniqueOrThrow({ where: { id: "tm_2" } });
  await redis.set(key, row.name, "EX", 60);
}
```

- homework
```ts
// 11a  key for tm_2
// 11b  PATCH name deletes the key
// 11c  do not cache "may this user edit tm_2" for a long TTL
```

- example 12. cache key uploads:up_6
- value videos/u_1/a.mp4, TTL 60 seconds
- delete the key when key changes
- a public segment can sit on a CDN. the money figure for up_6 should not
- Anemi does not use Redis. VMS and Bracket do

```ts
const key = "uploads:up_6";
const hit = await redis.get(key);
if (!hit) {
  const row = await db.item.findUniqueOrThrow({ where: { id: "up_6" } });
  await redis.set(key, row.key, "EX", 60);
}
```

- homework
```ts
// 12a  key for up_6
// 12b  PATCH key deletes the key
// 12c  do not cache "may this user edit up_6" for a long TTL
```

- example 13. cache key standings:st_1
- value player_a, TTL 60 seconds
- delete the key when player changes
- a public segment can sit on a CDN. the money figure for st_1 should not
- Anemi does not use Redis. VMS and Bracket do

```ts
const key = "standings:st_1";
const hit = await redis.get(key);
if (!hit) {
  const row = await db.item.findUniqueOrThrow({ where: { id: "st_1" } });
  await redis.set(key, row.player, "EX", 60);
}
```

- homework
```ts
// 13a  key for st_1
// 13b  PATCH player deletes the key
// 13c  do not cache "may this user edit st_1" for a long TTL
```

- example 14. cache key notices:n_2
- value Chattogram, TTL 60 seconds
- delete the key when port changes
- a public segment can sit on a CDN. the money figure for n_2 should not
- Anemi does not use Redis. VMS and Bracket do

```ts
const key = "notices:n_2";
const hit = await redis.get(key);
if (!hit) {
  const row = await db.item.findUniqueOrThrow({ where: { id: "n_2" } });
  await redis.set(key, row.port, "EX", 60);
}
```

- homework
```ts
// 14a  key for n_2
// 14b  PATCH port deletes the key
// 14c  do not cache "may this user edit n_2" for a long TTL
```

- example 15. cache key brackets:b_3
- value semi, TTL 60 seconds
- delete the key when round changes
- a public segment can sit on a CDN. the money figure for b_3 should not
- Anemi does not use Redis. VMS and Bracket do

```ts
const key = "brackets:b_3";
const hit = await redis.get(key);
if (!hit) {
  const row = await db.item.findUniqueOrThrow({ where: { id: "b_3" } });
  await redis.set(key, row.round, "EX", 60);
}
```

- homework
```ts
// 15a  key for b_3
// 15b  PATCH round deletes the key
// 15c  do not cache "may this user edit b_3" for a long TTL
```

- example 16. cache key sessions:se_1
- value u_3, TTL 60 seconds
- delete the key when userId changes
- a public segment can sit on a CDN. the money figure for se_1 should not
- Anemi does not use Redis. VMS and Bracket do

```ts
const key = "sessions:se_1";
const hit = await redis.get(key);
if (!hit) {
  const row = await db.item.findUniqueOrThrow({ where: { id: "se_1" } });
  await redis.set(key, row.userId, "EX", 60);
}
```

- homework
```ts
// 16a  key for se_1
// 16b  PATCH userId deletes the key
// 16c  do not cache "may this user edit se_1" for a long TTL
```

- example 17. cache key queues:q_1
- value laytime, TTL 60 seconds
- delete the key when name changes
- a public segment can sit on a CDN. the money figure for q_1 should not
- Anemi does not use Redis. VMS and Bracket do

```ts
const key = "queues:q_1";
const hit = await redis.get(key);
if (!hit) {
  const row = await db.item.findUniqueOrThrow({ where: { id: "q_1" } });
  await redis.set(key, row.name, "EX", 60);
}
```

- homework
```ts
// 17a  key for q_1
// 17b  PATCH name deletes the key
// 17c  do not cache "may this user edit q_1" for a long TTL
```

- example 18. cache key roles:r_1
- value operator, TTL 60 seconds
- delete the key when name changes
- a public segment can sit on a CDN. the money figure for r_1 should not
- Anemi does not use Redis. VMS and Bracket do

```ts
const key = "roles:r_1";
const hit = await redis.get(key);
if (!hit) {
  const row = await db.item.findUniqueOrThrow({ where: { id: "r_1" } });
  await redis.set(key, row.name, "EX", 60);
}
```

- homework
```ts
// 18a  key for r_1
// 18b  PATCH name deletes the key
// 18c  do not cache "may this user edit r_1" for a long TTL
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

- example 1. copy versus split for pc_12
- a replica is the same rows, including pc_12
- a shard would keep one tournament together. we have not sharded
- consistent hashing moves only the keys on the arc that gained a server
- add a replica before we talk about a shard ring

```text
primary writes pc_12
replica replays pc_12
shard key would be the tournament, not vesselName
ring: key pc_12 walks clockwise to the next server
```

- homework
```text
// 1a  replica for a report that can lag
// 1b  shard only after one disk cannot hold the rows
// 1c  adding a server moves some keys, not all keys
// 1d  a query without the shard key hits every shard
```

- example 2. copy versus split for m_1
- a replica is the same rows, including m_1
- a shard would keep one tournament together. we have not sharded
- consistent hashing moves only the keys on the arc that gained a server
- add a replica before we talk about a shard ring

```text
primary writes m_1
replica replays m_1
shard key would be the tournament, not home
ring: key m_1 walks clockwise to the next server
```

- homework
```text
// 2a  replica for a report that can lag
// 2b  shard only after one disk cannot hold the rows
// 2c  adding a server moves some keys, not all keys
// 2d  a query without the shard key hits every shard
```

- example 3. copy versus split for t_9
- a replica is the same rows, including t_9
- a shard would keep one tournament together. we have not sharded
- consistent hashing moves only the keys on the arc that gained a server
- add a replica before we talk about a shard ring

```text
primary writes t_9
replica replays t_9
shard key would be the tournament, not name
ring: key t_9 walks clockwise to the next server
```

- homework
```text
// 3a  replica for a report that can lag
// 3b  shard only after one disk cannot hold the rows
// 3c  adding a server moves some keys, not all keys
// 3d  a query without the shard key hits every shard
```

- example 4. copy versus split for v_4
- a replica is the same rows, including v_4
- a shard would keep one tournament together. we have not sharded
- consistent hashing moves only the keys on the arc that gained a server
- add a replica before we talk about a shard ring

```text
primary writes v_4
replica replays v_4
shard key would be the tournament, not title
ring: key v_4 walks clockwise to the next server
```

- homework
```text
// 4a  replica for a report that can lag
// 4b  shard only after one disk cannot hold the rows
// 4c  adding a server moves some keys, not all keys
// 4d  a query without the shard key hits every shard
```

- example 5. copy versus split for f_2
- a replica is the same rows, including f_2
- a shard would keep one tournament together. we have not sharded
- consistent hashing moves only the keys on the arc that gained a server
- add a replica before we talk about a shard ring

```text
primary writes f_2
replica replays f_2
shard key would be the tournament, not kind
ring: key f_2 walks clockwise to the next server
```

- homework
```text
// 5a  replica for a report that can lag
// 5b  shard only after one disk cannot hold the rows
// 5c  adding a server moves some keys, not all keys
// 5d  a query without the shard key hits every shard
```

- example 6. copy versus split for u_3
- a replica is the same rows, including u_3
- a shard would keep one tournament together. we have not sharded
- consistent hashing moves only the keys on the arc that gained a server
- add a replica before we talk about a shard ring

```text
primary writes u_3
replica replays u_3
shard key would be the tournament, not email
ring: key u_3 walks clockwise to the next server
```

- homework
```text
// 6a  replica for a report that can lag
// 6b  shard only after one disk cannot hold the rows
// 6c  adding a server moves some keys, not all keys
// 6d  a query without the shard key hits every shard
```

- example 7. copy versus split for pay_8
- a replica is the same rows, including pay_8
- a shard would keep one tournament together. we have not sharded
- consistent hashing moves only the keys on the arc that gained a server
- add a replica before we talk about a shard ring

```text
primary writes pay_8
replica replays pay_8
shard key would be the tournament, not eventId
ring: key pay_8 walks clockwise to the next server
```

- homework
```text
// 7a  replica for a report that can lag
// 7b  shard only after one disk cannot hold the rows
// 7c  adding a server moves some keys, not all keys
// 7d  a query without the shard key hits every shard
```

- example 8. copy versus split for room_2
- a replica is the same rows, including room_2
- a shard would keep one tournament together. we have not sharded
- consistent hashing moves only the keys on the arc that gained a server
- add a replica before we talk about a shard ring

```text
primary writes room_2
replica replays room_2
shard key would be the tournament, not sessionId
ring: key room_2 walks clockwise to the next server
```

- homework
```text
// 8a  replica for a report that can lag
// 8b  shard only after one disk cannot hold the rows
// 8c  adding a server moves some keys, not all keys
// 8d  a query without the shard key hits every shard
```

- example 9. copy versus split for vs_7
- a replica is the same rows, including vs_7
- a shard would keep one tournament together. we have not sharded
- consistent hashing moves only the keys on the arc that gained a server
- add a replica before we talk about a shard ring

```text
primary writes vs_7
replica replays vs_7
shard key would be the tournament, not name
ring: key vs_7 walks clockwise to the next server
```

- homework
```text
// 9a  replica for a report that can lag
// 9b  shard only after one disk cannot hold the rows
// 9c  adding a server moves some keys, not all keys
// 9d  a query without the shard key hits every shard
```

- example 10. copy versus split for sc_3
- a replica is the same rows, including sc_3
- a shard would keep one tournament together. we have not sharded
- consistent hashing moves only the keys on the arc that gained a server
- add a replica before we talk about a shard ring

```text
primary writes sc_3
replica replays sc_3
shard key would be the tournament, not points
ring: key sc_3 walks clockwise to the next server
```

- homework
```text
// 10a  replica for a report that can lag
// 10b  shard only after one disk cannot hold the rows
// 10c  adding a server moves some keys, not all keys
// 10d  a query without the shard key hits every shard
```

- example 11. copy versus split for tm_2
- a replica is the same rows, including tm_2
- a shard would keep one tournament together. we have not sharded
- consistent hashing moves only the keys on the arc that gained a server
- add a replica before we talk about a shard ring

```text
primary writes tm_2
replica replays tm_2
shard key would be the tournament, not name
ring: key tm_2 walks clockwise to the next server
```

- homework
```text
// 11a  replica for a report that can lag
// 11b  shard only after one disk cannot hold the rows
// 11c  adding a server moves some keys, not all keys
// 11d  a query without the shard key hits every shard
```

- example 12. copy versus split for up_6
- a replica is the same rows, including up_6
- a shard would keep one tournament together. we have not sharded
- consistent hashing moves only the keys on the arc that gained a server
- add a replica before we talk about a shard ring

```text
primary writes up_6
replica replays up_6
shard key would be the tournament, not key
ring: key up_6 walks clockwise to the next server
```

- homework
```text
// 12a  replica for a report that can lag
// 12b  shard only after one disk cannot hold the rows
// 12c  adding a server moves some keys, not all keys
// 12d  a query without the shard key hits every shard
```

- example 13. copy versus split for st_1
- a replica is the same rows, including st_1
- a shard would keep one tournament together. we have not sharded
- consistent hashing moves only the keys on the arc that gained a server
- add a replica before we talk about a shard ring

```text
primary writes st_1
replica replays st_1
shard key would be the tournament, not player
ring: key st_1 walks clockwise to the next server
```

- homework
```text
// 13a  replica for a report that can lag
// 13b  shard only after one disk cannot hold the rows
// 13c  adding a server moves some keys, not all keys
// 13d  a query without the shard key hits every shard
```

- example 14. copy versus split for n_2
- a replica is the same rows, including n_2
- a shard would keep one tournament together. we have not sharded
- consistent hashing moves only the keys on the arc that gained a server
- add a replica before we talk about a shard ring

```text
primary writes n_2
replica replays n_2
shard key would be the tournament, not port
ring: key n_2 walks clockwise to the next server
```

- homework
```text
// 14a  replica for a report that can lag
// 14b  shard only after one disk cannot hold the rows
// 14c  adding a server moves some keys, not all keys
// 14d  a query without the shard key hits every shard
```

- example 15. copy versus split for b_3
- a replica is the same rows, including b_3
- a shard would keep one tournament together. we have not sharded
- consistent hashing moves only the keys on the arc that gained a server
- add a replica before we talk about a shard ring

```text
primary writes b_3
replica replays b_3
shard key would be the tournament, not round
ring: key b_3 walks clockwise to the next server
```

- homework
```text
// 15a  replica for a report that can lag
// 15b  shard only after one disk cannot hold the rows
// 15c  adding a server moves some keys, not all keys
// 15d  a query without the shard key hits every shard
```

- example 16. copy versus split for se_1
- a replica is the same rows, including se_1
- a shard would keep one tournament together. we have not sharded
- consistent hashing moves only the keys on the arc that gained a server
- add a replica before we talk about a shard ring

```text
primary writes se_1
replica replays se_1
shard key would be the tournament, not userId
ring: key se_1 walks clockwise to the next server
```

- homework
```text
// 16a  replica for a report that can lag
// 16b  shard only after one disk cannot hold the rows
// 16c  adding a server moves some keys, not all keys
// 16d  a query without the shard key hits every shard
```

- example 17. copy versus split for q_1
- a replica is the same rows, including q_1
- a shard would keep one tournament together. we have not sharded
- consistent hashing moves only the keys on the arc that gained a server
- add a replica before we talk about a shard ring

```text
primary writes q_1
replica replays q_1
shard key would be the tournament, not name
ring: key q_1 walks clockwise to the next server
```

- homework
```text
// 17a  replica for a report that can lag
// 17b  shard only after one disk cannot hold the rows
// 17c  adding a server moves some keys, not all keys
// 17d  a query without the shard key hits every shard
```

- example 18. copy versus split for r_1
- a replica is the same rows, including r_1
- a shard would keep one tournament together. we have not sharded
- consistent hashing moves only the keys on the arc that gained a server
- add a replica before we talk about a shard ring

```text
primary writes r_1
replica replays r_1
shard key would be the tournament, not name
ring: key r_1 walks clockwise to the next server
```

- homework
```text
// 18a  replica for a report that can lag
// 18b  shard only after one disk cannot hold the rows
// 18c  adding a server moves some keys, not all keys
// 18d  a query without the shard key hits every shard
```

- example 19. copy versus split for pc_12
- a replica is the same rows, including pc_12
- a shard would keep one tournament together. we have not sharded
- consistent hashing moves only the keys on the arc that gained a server
- add a replica before we talk about a shard ring

```text
primary writes pc_12
replica replays pc_12
shard key would be the tournament, not vesselName
ring: key pc_12 walks clockwise to the next server
```

- homework
```text
// 19a  replica for a report that can lag
// 19b  shard only after one disk cannot hold the rows
// 19c  adding a server moves some keys, not all keys
// 19d  a query without the shard key hits every shard
```

- example 20. copy versus split for m_1
- a replica is the same rows, including m_1
- a shard would keep one tournament together. we have not sharded
- consistent hashing moves only the keys on the arc that gained a server
- add a replica before we talk about a shard ring

```text
primary writes m_1
replica replays m_1
shard key would be the tournament, not home
ring: key m_1 walks clockwise to the next server
```

- homework
```text
// 20a  replica for a report that can lag
// 20b  shard only after one disk cannot hold the rows
// 20c  adding a server moves some keys, not all keys
// 20d  a query without the shard key hits every shard
```

- example 21. copy versus split for t_9
- a replica is the same rows, including t_9
- a shard would keep one tournament together. we have not sharded
- consistent hashing moves only the keys on the arc that gained a server
- add a replica before we talk about a shard ring

```text
primary writes t_9
replica replays t_9
shard key would be the tournament, not name
ring: key t_9 walks clockwise to the next server
```

- homework
```text
// 21a  replica for a report that can lag
// 21b  shard only after one disk cannot hold the rows
// 21c  adding a server moves some keys, not all keys
// 21d  a query without the shard key hits every shard
```

- example 22. copy versus split for v_4
- a replica is the same rows, including v_4
- a shard would keep one tournament together. we have not sharded
- consistent hashing moves only the keys on the arc that gained a server
- add a replica before we talk about a shard ring

```text
primary writes v_4
replica replays v_4
shard key would be the tournament, not title
ring: key v_4 walks clockwise to the next server
```

- homework
```text
// 22a  replica for a report that can lag
// 22b  shard only after one disk cannot hold the rows
// 22c  adding a server moves some keys, not all keys
// 22d  a query without the shard key hits every shard
```

- example 23. copy versus split for f_2
- a replica is the same rows, including f_2
- a shard would keep one tournament together. we have not sharded
- consistent hashing moves only the keys on the arc that gained a server
- add a replica before we talk about a shard ring

```text
primary writes f_2
replica replays f_2
shard key would be the tournament, not kind
ring: key f_2 walks clockwise to the next server
```

- homework
```text
// 23a  replica for a report that can lag
// 23b  shard only after one disk cannot hold the rows
// 23c  adding a server moves some keys, not all keys
// 23d  a query without the shard key hits every shard
```

- example 24. copy versus split for u_3
- a replica is the same rows, including u_3
- a shard would keep one tournament together. we have not sharded
- consistent hashing moves only the keys on the arc that gained a server
- add a replica before we talk about a shard ring

```text
primary writes u_3
replica replays u_3
shard key would be the tournament, not email
ring: key u_3 walks clockwise to the next server
```

- homework
```text
// 24a  replica for a report that can lag
// 24b  shard only after one disk cannot hold the rows
// 24c  adding a server moves some keys, not all keys
// 24d  a query without the shard key hits every shard
```

- example 25. copy versus split for pay_8
- a replica is the same rows, including pay_8
- a shard would keep one tournament together. we have not sharded
- consistent hashing moves only the keys on the arc that gained a server
- add a replica before we talk about a shard ring

```text
primary writes pay_8
replica replays pay_8
shard key would be the tournament, not eventId
ring: key pay_8 walks clockwise to the next server
```

- homework
```text
// 25a  replica for a report that can lag
// 25b  shard only after one disk cannot hold the rows
// 25c  adding a server moves some keys, not all keys
// 25d  a query without the shard key hits every shard
```

- example 26. copy versus split for room_2
- a replica is the same rows, including room_2
- a shard would keep one tournament together. we have not sharded
- consistent hashing moves only the keys on the arc that gained a server
- add a replica before we talk about a shard ring

```text
primary writes room_2
replica replays room_2
shard key would be the tournament, not sessionId
ring: key room_2 walks clockwise to the next server
```

- homework
```text
// 26a  replica for a report that can lag
// 26b  shard only after one disk cannot hold the rows
// 26c  adding a server moves some keys, not all keys
// 26d  a query without the shard key hits every shard
```

- example 27. copy versus split for vs_7
- a replica is the same rows, including vs_7
- a shard would keep one tournament together. we have not sharded
- consistent hashing moves only the keys on the arc that gained a server
- add a replica before we talk about a shard ring

```text
primary writes vs_7
replica replays vs_7
shard key would be the tournament, not name
ring: key vs_7 walks clockwise to the next server
```

- homework
```text
// 27a  replica for a report that can lag
// 27b  shard only after one disk cannot hold the rows
// 27c  adding a server moves some keys, not all keys
// 27d  a query without the shard key hits every shard
```

- example 28. copy versus split for sc_3
- a replica is the same rows, including sc_3
- a shard would keep one tournament together. we have not sharded
- consistent hashing moves only the keys on the arc that gained a server
- add a replica before we talk about a shard ring

```text
primary writes sc_3
replica replays sc_3
shard key would be the tournament, not points
ring: key sc_3 walks clockwise to the next server
```

- homework
```text
// 28a  replica for a report that can lag
// 28b  shard only after one disk cannot hold the rows
// 28c  adding a server moves some keys, not all keys
// 28d  a query without the shard key hits every shard
```

- example 29. copy versus split for tm_2
- a replica is the same rows, including tm_2
- a shard would keep one tournament together. we have not sharded
- consistent hashing moves only the keys on the arc that gained a server
- add a replica before we talk about a shard ring

```text
primary writes tm_2
replica replays tm_2
shard key would be the tournament, not name
ring: key tm_2 walks clockwise to the next server
```

- homework
```text
// 29a  replica for a report that can lag
// 29b  shard only after one disk cannot hold the rows
// 29c  adding a server moves some keys, not all keys
// 29d  a query without the shard key hits every shard
```

- example 30. copy versus split for up_6
- a replica is the same rows, including up_6
- a shard would keep one tournament together. we have not sharded
- consistent hashing moves only the keys on the arc that gained a server
- add a replica before we talk about a shard ring

```text
primary writes up_6
replica replays up_6
shard key would be the tournament, not key
ring: key up_6 walks clockwise to the next server
```

- homework
```text
// 30a  replica for a report that can lag
// 30b  shard only after one disk cannot hold the rows
// 30c  adding a server moves some keys, not all keys
// 30d  a query without the shard key hits every shard
```

- example 31. copy versus split for st_1
- a replica is the same rows, including st_1
- a shard would keep one tournament together. we have not sharded
- consistent hashing moves only the keys on the arc that gained a server
- add a replica before we talk about a shard ring

```text
primary writes st_1
replica replays st_1
shard key would be the tournament, not player
ring: key st_1 walks clockwise to the next server
```

- homework
```text
// 31a  replica for a report that can lag
// 31b  shard only after one disk cannot hold the rows
// 31c  adding a server moves some keys, not all keys
// 31d  a query without the shard key hits every shard
```

- example 32. copy versus split for n_2
- a replica is the same rows, including n_2
- a shard would keep one tournament together. we have not sharded
- consistent hashing moves only the keys on the arc that gained a server
- add a replica before we talk about a shard ring

```text
primary writes n_2
replica replays n_2
shard key would be the tournament, not port
ring: key n_2 walks clockwise to the next server
```

- homework
```text
// 32a  replica for a report that can lag
// 32b  shard only after one disk cannot hold the rows
// 32c  adding a server moves some keys, not all keys
// 32d  a query without the shard key hits every shard
```

- example 33. copy versus split for b_3
- a replica is the same rows, including b_3
- a shard would keep one tournament together. we have not sharded
- consistent hashing moves only the keys on the arc that gained a server
- add a replica before we talk about a shard ring

```text
primary writes b_3
replica replays b_3
shard key would be the tournament, not round
ring: key b_3 walks clockwise to the next server
```

- homework
```text
// 33a  replica for a report that can lag
// 33b  shard only after one disk cannot hold the rows
// 33c  adding a server moves some keys, not all keys
// 33d  a query without the shard key hits every shard
```

- example 34. copy versus split for se_1
- a replica is the same rows, including se_1
- a shard would keep one tournament together. we have not sharded
- consistent hashing moves only the keys on the arc that gained a server
- add a replica before we talk about a shard ring

```text
primary writes se_1
replica replays se_1
shard key would be the tournament, not userId
ring: key se_1 walks clockwise to the next server
```

- homework
```text
// 34a  replica for a report that can lag
// 34b  shard only after one disk cannot hold the rows
// 34c  adding a server moves some keys, not all keys
// 34d  a query without the shard key hits every shard
```

- example 35. copy versus split for q_1
- a replica is the same rows, including q_1
- a shard would keep one tournament together. we have not sharded
- consistent hashing moves only the keys on the arc that gained a server
- add a replica before we talk about a shard ring

```text
primary writes q_1
replica replays q_1
shard key would be the tournament, not name
ring: key q_1 walks clockwise to the next server
```

- homework
```text
// 35a  replica for a report that can lag
// 35b  shard only after one disk cannot hold the rows
// 35c  adding a server moves some keys, not all keys
// 35d  a query without the shard key hits every shard
```

- example 36. copy versus split for r_1
- a replica is the same rows, including r_1
- a shard would keep one tournament together. we have not sharded
- consistent hashing moves only the keys on the arc that gained a server
- add a replica before we talk about a shard ring

```text
primary writes r_1
replica replays r_1
shard key would be the tournament, not name
ring: key r_1 walks clockwise to the next server
```

- homework
```text
// 36a  replica for a report that can lag
// 36b  shard only after one disk cannot hold the rows
// 36c  adding a server moves some keys, not all keys
// 36d  a query without the shard key hits every shard
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

- example 1. consistency for the number 7500.0
- a money figure would rather fail than show two values
- a play count can be a second old
- PACELC asks what we do on a healthy day as well as during a partition
- read-your-writes: the user who just saved pc_12 reads the primary

```text
figure 7500.0 for inputs 72, 84, 15000
CP-style: do not serve a second copy that disagrees
AP-style: a counter may lag
async replica: lower latency, possible lag on pc_12
```

- homework
```text
// 1a  CP or AP for 7500.0
// 1b  the E in PACELC on a normal day
// 1c  the save of pc_12 is read back from the primary
```

- example 2. consistency for the number 0
- a money figure would rather fail than show two values
- a play count can be a second old
- PACELC asks what we do on a healthy day as well as during a partition
- read-your-writes: the user who just saved m_1 reads the primary

```text
figure 0 for inputs 3, 1, 32
CP-style: do not serve a second copy that disagrees
AP-style: a counter may lag
async replica: lower latency, possible lag on m_1
```

- homework
```text
// 2a  CP or AP for 0
// 2b  the E in PACELC on a normal day
// 2c  the save of m_1 is read back from the primary
```

- example 3. consistency for the number 7500.0
- a money figure would rather fail than show two values
- a play count can be a second old
- PACELC asks what we do on a healthy day as well as during a partition
- read-your-writes: the user who just saved t_9 reads the primary

```text
figure 7500.0 for inputs 16, 8, 24
CP-style: do not serve a second copy that disagrees
AP-style: a counter may lag
async replica: lower latency, possible lag on t_9
```

- homework
```text
// 3a  CP or AP for 7500.0
// 3b  the E in PACELC on a normal day
// 3c  the save of t_9 is read back from the primary
```

- example 4. consistency for the number 0
- a money figure would rather fail than show two values
- a play count can be a second old
- PACELC asks what we do on a healthy day as well as during a partition
- read-your-writes: the user who just saved v_4 reads the primary

```text
figure 0 for inputs 6, 10, 6
CP-style: do not serve a second copy that disagrees
AP-style: a counter may lag
async replica: lower latency, possible lag on v_4
```

- homework
```text
// 4a  CP or AP for 0
// 4b  the E in PACELC on a normal day
// 4c  the save of v_4 is read back from the primary
```

- example 5. consistency for the number 0
- a money figure would rather fail than show two values
- a play count can be a second old
- PACELC asks what we do on a healthy day as well as during a partition
- read-your-writes: the user who just saved f_2 reads the primary

```text
figure 0 for inputs 1, 2, 500
CP-style: do not serve a second copy that disagrees
AP-style: a counter may lag
async replica: lower latency, possible lag on f_2
```

- homework
```text
// 5a  CP or AP for 0
// 5b  the E in PACELC on a normal day
// 5c  the save of f_2 is read back from the primary
```

- example 6. consistency for the number 0
- a money figure would rather fail than show two values
- a play count can be a second old
- PACELC asks what we do on a healthy day as well as during a partition
- read-your-writes: the user who just saved u_3 reads the primary

```text
figure 0 for inputs 1, 1, 12
CP-style: do not serve a second copy that disagrees
AP-style: a counter may lag
async replica: lower latency, possible lag on u_3
```

- homework
```text
// 6a  CP or AP for 0
// 6b  the E in PACELC on a normal day
// 6c  the save of u_3 is read back from the primary
```

- example 7. consistency for the number 7500.0
- a money figure would rather fail than show two values
- a play count can be a second old
- PACELC asks what we do on a healthy day as well as during a partition
- read-your-writes: the user who just saved pay_8 reads the primary

```text
figure 7500.0 for inputs 15000, 7500, 1
CP-style: do not serve a second copy that disagrees
AP-style: a counter may lag
async replica: lower latency, possible lag on pay_8
```

- homework
```text
// 7a  CP or AP for 7500.0
// 7b  the E in PACELC on a normal day
// 7c  the save of pay_8 is read back from the primary
```

- example 8. consistency for the number 17500.0
- a money figure would rather fail than show two values
- a play count can be a second old
- PACELC asks what we do on a healthy day as well as during a partition
- read-your-writes: the user who just saved room_2 reads the primary

```text
figure 17500.0 for inputs 12, 40, 1
CP-style: do not serve a second copy that disagrees
AP-style: a counter may lag
async replica: lower latency, possible lag on room_2
```

- homework
```text
// 8a  CP or AP for 17500.0
// 8b  the E in PACELC on a normal day
// 8c  the save of room_2 is read back from the primary
```

- example 9. consistency for the number 4000.0
- a money figure would rather fail than show two values
- a play count can be a second old
- PACELC asks what we do on a healthy day as well as during a partition
- read-your-writes: the user who just saved vs_7 reads the primary

```text
figure 4000.0 for inputs 48, 60, 8000
CP-style: do not serve a second copy that disagrees
AP-style: a counter may lag
async replica: lower latency, possible lag on vs_7
```

- homework
```text
// 9a  CP or AP for 4000.0
// 9b  the E in PACELC on a normal day
// 9c  the save of vs_7 is read back from the primary
```

- example 10. consistency for the number 7500.0
- a money figure would rather fail than show two values
- a play count can be a second old
- PACELC asks what we do on a healthy day as well as during a partition
- read-your-writes: the user who just saved sc_3 reads the primary

```text
figure 7500.0 for inputs 11, 7, 32
CP-style: do not serve a second copy that disagrees
AP-style: a counter may lag
async replica: lower latency, possible lag on sc_3
```

- homework
```text
// 10a  CP or AP for 7500.0
// 10b  the E in PACELC on a normal day
// 10c  the save of sc_3 is read back from the primary
```

- example 11. consistency for the number 0
- a money figure would rather fail than show two values
- a play count can be a second old
- PACELC asks what we do on a healthy day as well as during a partition
- read-your-writes: the user who just saved tm_2 reads the primary

```text
figure 0 for inputs 2, 1, 10
CP-style: do not serve a second copy that disagrees
AP-style: a counter may lag
async replica: lower latency, possible lag on tm_2
```

- homework
```text
// 11a  CP or AP for 0
// 11b  the E in PACELC on a normal day
// 11c  the save of tm_2 is read back from the primary
```

- example 12. consistency for the number 7500.0
- a money figure would rather fail than show two values
- a play count can be a second old
- PACELC asks what we do on a healthy day as well as during a partition
- read-your-writes: the user who just saved up_6 reads the primary

```text
figure 7500.0 for inputs 200, 6, 1
CP-style: do not serve a second copy that disagrees
AP-style: a counter may lag
async replica: lower latency, possible lag on up_6
```

- homework
```text
// 12a  CP or AP for 7500.0
// 12b  the E in PACELC on a normal day
// 12c  the save of up_6 is read back from the primary
```

- example 13. consistency for the number 125000.0
- a money figure would rather fail than show two values
- a play count can be a second old
- PACELC asks what we do on a healthy day as well as during a partition
- read-your-writes: the user who just saved st_1 reads the primary

```text
figure 125000.0 for inputs 1500, 1700, 32
CP-style: do not serve a second copy that disagrees
AP-style: a counter may lag
async replica: lower latency, possible lag on st_1
```

- homework
```text
// 13a  CP or AP for 125000.0
// 13b  the E in PACELC on a normal day
// 13c  the save of st_1 is read back from the primary
```

- example 14. consistency for the number 3000.0
- a money figure would rather fail than show two values
- a play count can be a second old
- PACELC asks what we do on a healthy day as well as during a partition
- read-your-writes: the user who just saved n_2 reads the primary

```text
figure 3000.0 for inputs 24, 30, 12000
CP-style: do not serve a second copy that disagrees
AP-style: a counter may lag
async replica: lower latency, possible lag on n_2
```

- homework
```text
// 14a  CP or AP for 3000.0
// 14b  the E in PACELC on a normal day
// 14c  the save of n_2 is read back from the primary
```

- example 15. consistency for the number 0
- a money figure would rather fail than show two values
- a play count can be a second old
- PACELC asks what we do on a healthy day as well as during a partition
- read-your-writes: the user who just saved b_3 reads the primary

```text
figure 0 for inputs 4, 2, 16
CP-style: do not serve a second copy that disagrees
AP-style: a counter may lag
async replica: lower latency, possible lag on b_3
```

- homework
```text
// 15a  CP or AP for 0
// 15b  the E in PACELC on a normal day
// 15c  the save of b_3 is read back from the primary
```

- example 16. consistency for the number 28125.0
- a money figure would rather fail than show two values
- a play count can be a second old
- PACELC asks what we do on a healthy day as well as during a partition
- read-your-writes: the user who just saved se_1 reads the primary

```text
figure 28125.0 for inputs 15, 60, 1
CP-style: do not serve a second copy that disagrees
AP-style: a counter may lag
async replica: lower latency, possible lag on se_1
```

- homework
```text
// 16a  CP or AP for 28125.0
// 16b  the E in PACELC on a normal day
// 16c  the save of se_1 is read back from the primary
```

- example 17. consistency for the number 0
- a money figure would rather fail than show two values
- a play count can be a second old
- PACELC asks what we do on a healthy day as well as during a partition
- read-your-writes: the user who just saved q_1 reads the primary

```text
figure 0 for inputs 10, 3, 1
CP-style: do not serve a second copy that disagrees
AP-style: a counter may lag
async replica: lower latency, possible lag on q_1
```

- homework
```text
// 17a  CP or AP for 0
// 17b  the E in PACELC on a normal day
// 17c  the save of q_1 is read back from the primary
```

- example 18. consistency for the number 0
- a money figure would rather fail than show two values
- a play count can be a second old
- PACELC asks what we do on a healthy day as well as during a partition
- read-your-writes: the user who just saved r_1 reads the primary

```text
figure 0 for inputs 2, 4, 1
CP-style: do not serve a second copy that disagrees
AP-style: a counter may lag
async replica: lower latency, possible lag on r_1
```

- homework
```text
// 18a  CP or AP for 0
// 18b  the E in PACELC on a normal day
// 18c  the save of r_1 is read back from the primary
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

- example 1. an event that already happened for pc_12
- the name is past tense
- a queue gives the job to one worker. pub/sub gives a copy to each screen
- BullMQ is the queue we run. Kafka is the log we have not operated
- at-least-once means the consumer checks the event id

```ts
type Event = {
  type: "LaytimeCalculated";
  portCallId: "pc_12";
  demurrage: 7500.0;
};
```

- homework
```ts
// 1a  one worker recalculates pc_12: a queue
// 1b  every open screen hears it: pub/sub or Socket.IO
// 1c  store the event id once
// 1d  we have run BullMQ, not Kafka
```

- example 2. an event that already happened for m_1
- the name is past tense
- a queue gives the job to one worker. pub/sub gives a copy to each screen
- BullMQ is the queue we run. Kafka is the log we have not operated
- at-least-once means the consumer checks the event id

```ts
type Event = {
  type: "LaytimeCalculated";
  portCallId: "m_1";
  demurrage: 0;
};
```

- homework
```ts
// 2a  one worker recalculates m_1: a queue
// 2b  every open screen hears it: pub/sub or Socket.IO
// 2c  store the event id once
// 2d  we have run BullMQ, not Kafka
```

- example 3. an event that already happened for t_9
- the name is past tense
- a queue gives the job to one worker. pub/sub gives a copy to each screen
- BullMQ is the queue we run. Kafka is the log we have not operated
- at-least-once means the consumer checks the event id

```ts
type Event = {
  type: "LaytimeCalculated";
  portCallId: "t_9";
  demurrage: 7500.0;
};
```

- homework
```ts
// 3a  one worker recalculates t_9: a queue
// 3b  every open screen hears it: pub/sub or Socket.IO
// 3c  store the event id once
// 3d  we have run BullMQ, not Kafka
```

- example 4. an event that already happened for v_4
- the name is past tense
- a queue gives the job to one worker. pub/sub gives a copy to each screen
- BullMQ is the queue we run. Kafka is the log we have not operated
- at-least-once means the consumer checks the event id

```ts
type Event = {
  type: "LaytimeCalculated";
  portCallId: "v_4";
  demurrage: 0;
};
```

- homework
```ts
// 4a  one worker recalculates v_4: a queue
// 4b  every open screen hears it: pub/sub or Socket.IO
// 4c  store the event id once
// 4d  we have run BullMQ, not Kafka
```

- example 5. an event that already happened for f_2
- the name is past tense
- a queue gives the job to one worker. pub/sub gives a copy to each screen
- BullMQ is the queue we run. Kafka is the log we have not operated
- at-least-once means the consumer checks the event id

```ts
type Event = {
  type: "LaytimeCalculated";
  portCallId: "f_2";
  demurrage: 0;
};
```

- homework
```ts
// 5a  one worker recalculates f_2: a queue
// 5b  every open screen hears it: pub/sub or Socket.IO
// 5c  store the event id once
// 5d  we have run BullMQ, not Kafka
```

- example 6. an event that already happened for u_3
- the name is past tense
- a queue gives the job to one worker. pub/sub gives a copy to each screen
- BullMQ is the queue we run. Kafka is the log we have not operated
- at-least-once means the consumer checks the event id

```ts
type Event = {
  type: "LaytimeCalculated";
  portCallId: "u_3";
  demurrage: 0;
};
```

- homework
```ts
// 6a  one worker recalculates u_3: a queue
// 6b  every open screen hears it: pub/sub or Socket.IO
// 6c  store the event id once
// 6d  we have run BullMQ, not Kafka
```

- example 7. an event that already happened for pay_8
- the name is past tense
- a queue gives the job to one worker. pub/sub gives a copy to each screen
- BullMQ is the queue we run. Kafka is the log we have not operated
- at-least-once means the consumer checks the event id

```ts
type Event = {
  type: "LaytimeCalculated";
  portCallId: "pay_8";
  demurrage: 7500.0;
};
```

- homework
```ts
// 7a  one worker recalculates pay_8: a queue
// 7b  every open screen hears it: pub/sub or Socket.IO
// 7c  store the event id once
// 7d  we have run BullMQ, not Kafka
```

- example 8. an event that already happened for room_2
- the name is past tense
- a queue gives the job to one worker. pub/sub gives a copy to each screen
- BullMQ is the queue we run. Kafka is the log we have not operated
- at-least-once means the consumer checks the event id

```ts
type Event = {
  type: "LaytimeCalculated";
  portCallId: "room_2";
  demurrage: 17500.0;
};
```

- homework
```ts
// 8a  one worker recalculates room_2: a queue
// 8b  every open screen hears it: pub/sub or Socket.IO
// 8c  store the event id once
// 8d  we have run BullMQ, not Kafka
```

- example 9. an event that already happened for vs_7
- the name is past tense
- a queue gives the job to one worker. pub/sub gives a copy to each screen
- BullMQ is the queue we run. Kafka is the log we have not operated
- at-least-once means the consumer checks the event id

```ts
type Event = {
  type: "LaytimeCalculated";
  portCallId: "vs_7";
  demurrage: 4000.0;
};
```

- homework
```ts
// 9a  one worker recalculates vs_7: a queue
// 9b  every open screen hears it: pub/sub or Socket.IO
// 9c  store the event id once
// 9d  we have run BullMQ, not Kafka
```

- example 10. an event that already happened for sc_3
- the name is past tense
- a queue gives the job to one worker. pub/sub gives a copy to each screen
- BullMQ is the queue we run. Kafka is the log we have not operated
- at-least-once means the consumer checks the event id

```ts
type Event = {
  type: "LaytimeCalculated";
  portCallId: "sc_3";
  demurrage: 7500.0;
};
```

- homework
```ts
// 10a  one worker recalculates sc_3: a queue
// 10b  every open screen hears it: pub/sub or Socket.IO
// 10c  store the event id once
// 10d  we have run BullMQ, not Kafka
```

- example 11. an event that already happened for tm_2
- the name is past tense
- a queue gives the job to one worker. pub/sub gives a copy to each screen
- BullMQ is the queue we run. Kafka is the log we have not operated
- at-least-once means the consumer checks the event id

```ts
type Event = {
  type: "LaytimeCalculated";
  portCallId: "tm_2";
  demurrage: 0;
};
```

- homework
```ts
// 11a  one worker recalculates tm_2: a queue
// 11b  every open screen hears it: pub/sub or Socket.IO
// 11c  store the event id once
// 11d  we have run BullMQ, not Kafka
```

- example 12. an event that already happened for up_6
- the name is past tense
- a queue gives the job to one worker. pub/sub gives a copy to each screen
- BullMQ is the queue we run. Kafka is the log we have not operated
- at-least-once means the consumer checks the event id

```ts
type Event = {
  type: "LaytimeCalculated";
  portCallId: "up_6";
  demurrage: 7500.0;
};
```

- homework
```ts
// 12a  one worker recalculates up_6: a queue
// 12b  every open screen hears it: pub/sub or Socket.IO
// 12c  store the event id once
// 12d  we have run BullMQ, not Kafka
```

- example 13. an event that already happened for st_1
- the name is past tense
- a queue gives the job to one worker. pub/sub gives a copy to each screen
- BullMQ is the queue we run. Kafka is the log we have not operated
- at-least-once means the consumer checks the event id

```ts
type Event = {
  type: "LaytimeCalculated";
  portCallId: "st_1";
  demurrage: 125000.0;
};
```

- homework
```ts
// 13a  one worker recalculates st_1: a queue
// 13b  every open screen hears it: pub/sub or Socket.IO
// 13c  store the event id once
// 13d  we have run BullMQ, not Kafka
```

- example 14. an event that already happened for n_2
- the name is past tense
- a queue gives the job to one worker. pub/sub gives a copy to each screen
- BullMQ is the queue we run. Kafka is the log we have not operated
- at-least-once means the consumer checks the event id

```ts
type Event = {
  type: "LaytimeCalculated";
  portCallId: "n_2";
  demurrage: 3000.0;
};
```

- homework
```ts
// 14a  one worker recalculates n_2: a queue
// 14b  every open screen hears it: pub/sub or Socket.IO
// 14c  store the event id once
// 14d  we have run BullMQ, not Kafka
```

- example 15. an event that already happened for b_3
- the name is past tense
- a queue gives the job to one worker. pub/sub gives a copy to each screen
- BullMQ is the queue we run. Kafka is the log we have not operated
- at-least-once means the consumer checks the event id

```ts
type Event = {
  type: "LaytimeCalculated";
  portCallId: "b_3";
  demurrage: 0;
};
```

- homework
```ts
// 15a  one worker recalculates b_3: a queue
// 15b  every open screen hears it: pub/sub or Socket.IO
// 15c  store the event id once
// 15d  we have run BullMQ, not Kafka
```

- example 16. an event that already happened for se_1
- the name is past tense
- a queue gives the job to one worker. pub/sub gives a copy to each screen
- BullMQ is the queue we run. Kafka is the log we have not operated
- at-least-once means the consumer checks the event id

```ts
type Event = {
  type: "LaytimeCalculated";
  portCallId: "se_1";
  demurrage: 28125.0;
};
```

- homework
```ts
// 16a  one worker recalculates se_1: a queue
// 16b  every open screen hears it: pub/sub or Socket.IO
// 16c  store the event id once
// 16d  we have run BullMQ, not Kafka
```

- example 17. an event that already happened for q_1
- the name is past tense
- a queue gives the job to one worker. pub/sub gives a copy to each screen
- BullMQ is the queue we run. Kafka is the log we have not operated
- at-least-once means the consumer checks the event id

```ts
type Event = {
  type: "LaytimeCalculated";
  portCallId: "q_1";
  demurrage: 0;
};
```

- homework
```ts
// 17a  one worker recalculates q_1: a queue
// 17b  every open screen hears it: pub/sub or Socket.IO
// 17c  store the event id once
// 17d  we have run BullMQ, not Kafka
```

- example 18. an event that already happened for r_1
- the name is past tense
- a queue gives the job to one worker. pub/sub gives a copy to each screen
- BullMQ is the queue we run. Kafka is the log we have not operated
- at-least-once means the consumer checks the event id

```ts
type Event = {
  type: "LaytimeCalculated";
  portCallId: "r_1";
  demurrage: 0;
};
```

- homework
```ts
// 18a  one worker recalculates r_1: a queue
// 18b  every open screen hears it: pub/sub or Socket.IO
// 18c  store the event id once
// 18d  we have run BullMQ, not Kafka
```

- example 19. an event that already happened for pc_12
- the name is past tense
- a queue gives the job to one worker. pub/sub gives a copy to each screen
- BullMQ is the queue we run. Kafka is the log we have not operated
- at-least-once means the consumer checks the event id

```ts
type Event = {
  type: "LaytimeCalculated";
  portCallId: "pc_12";
  demurrage: 7500.0;
};
```

- homework
```ts
// 19a  one worker recalculates pc_12: a queue
// 19b  every open screen hears it: pub/sub or Socket.IO
// 19c  store the event id once
// 19d  we have run BullMQ, not Kafka
```

- example 20. an event that already happened for m_1
- the name is past tense
- a queue gives the job to one worker. pub/sub gives a copy to each screen
- BullMQ is the queue we run. Kafka is the log we have not operated
- at-least-once means the consumer checks the event id

```ts
type Event = {
  type: "LaytimeCalculated";
  portCallId: "m_1";
  demurrage: 0;
};
```

- homework
```ts
// 20a  one worker recalculates m_1: a queue
// 20b  every open screen hears it: pub/sub or Socket.IO
// 20c  store the event id once
// 20d  we have run BullMQ, not Kafka
```

- example 21. an event that already happened for t_9
- the name is past tense
- a queue gives the job to one worker. pub/sub gives a copy to each screen
- BullMQ is the queue we run. Kafka is the log we have not operated
- at-least-once means the consumer checks the event id

```ts
type Event = {
  type: "LaytimeCalculated";
  portCallId: "t_9";
  demurrage: 7500.0;
};
```

- homework
```ts
// 21a  one worker recalculates t_9: a queue
// 21b  every open screen hears it: pub/sub or Socket.IO
// 21c  store the event id once
// 21d  we have run BullMQ, not Kafka
```

- example 22. an event that already happened for v_4
- the name is past tense
- a queue gives the job to one worker. pub/sub gives a copy to each screen
- BullMQ is the queue we run. Kafka is the log we have not operated
- at-least-once means the consumer checks the event id

```ts
type Event = {
  type: "LaytimeCalculated";
  portCallId: "v_4";
  demurrage: 0;
};
```

- homework
```ts
// 22a  one worker recalculates v_4: a queue
// 22b  every open screen hears it: pub/sub or Socket.IO
// 22c  store the event id once
// 22d  we have run BullMQ, not Kafka
```

- example 23. an event that already happened for f_2
- the name is past tense
- a queue gives the job to one worker. pub/sub gives a copy to each screen
- BullMQ is the queue we run. Kafka is the log we have not operated
- at-least-once means the consumer checks the event id

```ts
type Event = {
  type: "LaytimeCalculated";
  portCallId: "f_2";
  demurrage: 0;
};
```

- homework
```ts
// 23a  one worker recalculates f_2: a queue
// 23b  every open screen hears it: pub/sub or Socket.IO
// 23c  store the event id once
// 23d  we have run BullMQ, not Kafka
```

- example 24. an event that already happened for u_3
- the name is past tense
- a queue gives the job to one worker. pub/sub gives a copy to each screen
- BullMQ is the queue we run. Kafka is the log we have not operated
- at-least-once means the consumer checks the event id

```ts
type Event = {
  type: "LaytimeCalculated";
  portCallId: "u_3";
  demurrage: 0;
};
```

- homework
```ts
// 24a  one worker recalculates u_3: a queue
// 24b  every open screen hears it: pub/sub or Socket.IO
// 24c  store the event id once
// 24d  we have run BullMQ, not Kafka
```

- example 25. an event that already happened for pay_8
- the name is past tense
- a queue gives the job to one worker. pub/sub gives a copy to each screen
- BullMQ is the queue we run. Kafka is the log we have not operated
- at-least-once means the consumer checks the event id

```ts
type Event = {
  type: "LaytimeCalculated";
  portCallId: "pay_8";
  demurrage: 7500.0;
};
```

- homework
```ts
// 25a  one worker recalculates pay_8: a queue
// 25b  every open screen hears it: pub/sub or Socket.IO
// 25c  store the event id once
// 25d  we have run BullMQ, not Kafka
```

- example 26. an event that already happened for room_2
- the name is past tense
- a queue gives the job to one worker. pub/sub gives a copy to each screen
- BullMQ is the queue we run. Kafka is the log we have not operated
- at-least-once means the consumer checks the event id

```ts
type Event = {
  type: "LaytimeCalculated";
  portCallId: "room_2";
  demurrage: 17500.0;
};
```

- homework
```ts
// 26a  one worker recalculates room_2: a queue
// 26b  every open screen hears it: pub/sub or Socket.IO
// 26c  store the event id once
// 26d  we have run BullMQ, not Kafka
```

- example 27. an event that already happened for vs_7
- the name is past tense
- a queue gives the job to one worker. pub/sub gives a copy to each screen
- BullMQ is the queue we run. Kafka is the log we have not operated
- at-least-once means the consumer checks the event id

```ts
type Event = {
  type: "LaytimeCalculated";
  portCallId: "vs_7";
  demurrage: 4000.0;
};
```

- homework
```ts
// 27a  one worker recalculates vs_7: a queue
// 27b  every open screen hears it: pub/sub or Socket.IO
// 27c  store the event id once
// 27d  we have run BullMQ, not Kafka
```

- example 28. an event that already happened for sc_3
- the name is past tense
- a queue gives the job to one worker. pub/sub gives a copy to each screen
- BullMQ is the queue we run. Kafka is the log we have not operated
- at-least-once means the consumer checks the event id

```ts
type Event = {
  type: "LaytimeCalculated";
  portCallId: "sc_3";
  demurrage: 7500.0;
};
```

- homework
```ts
// 28a  one worker recalculates sc_3: a queue
// 28b  every open screen hears it: pub/sub or Socket.IO
// 28c  store the event id once
// 28d  we have run BullMQ, not Kafka
```

- example 29. an event that already happened for tm_2
- the name is past tense
- a queue gives the job to one worker. pub/sub gives a copy to each screen
- BullMQ is the queue we run. Kafka is the log we have not operated
- at-least-once means the consumer checks the event id

```ts
type Event = {
  type: "LaytimeCalculated";
  portCallId: "tm_2";
  demurrage: 0;
};
```

- homework
```ts
// 29a  one worker recalculates tm_2: a queue
// 29b  every open screen hears it: pub/sub or Socket.IO
// 29c  store the event id once
// 29d  we have run BullMQ, not Kafka
```

- example 30. an event that already happened for up_6
- the name is past tense
- a queue gives the job to one worker. pub/sub gives a copy to each screen
- BullMQ is the queue we run. Kafka is the log we have not operated
- at-least-once means the consumer checks the event id

```ts
type Event = {
  type: "LaytimeCalculated";
  portCallId: "up_6";
  demurrage: 7500.0;
};
```

- homework
```ts
// 30a  one worker recalculates up_6: a queue
// 30b  every open screen hears it: pub/sub or Socket.IO
// 30c  store the event id once
// 30d  we have run BullMQ, not Kafka
```

- example 31. an event that already happened for st_1
- the name is past tense
- a queue gives the job to one worker. pub/sub gives a copy to each screen
- BullMQ is the queue we run. Kafka is the log we have not operated
- at-least-once means the consumer checks the event id

```ts
type Event = {
  type: "LaytimeCalculated";
  portCallId: "st_1";
  demurrage: 125000.0;
};
```

- homework
```ts
// 31a  one worker recalculates st_1: a queue
// 31b  every open screen hears it: pub/sub or Socket.IO
// 31c  store the event id once
// 31d  we have run BullMQ, not Kafka
```

- example 32. an event that already happened for n_2
- the name is past tense
- a queue gives the job to one worker. pub/sub gives a copy to each screen
- BullMQ is the queue we run. Kafka is the log we have not operated
- at-least-once means the consumer checks the event id

```ts
type Event = {
  type: "LaytimeCalculated";
  portCallId: "n_2";
  demurrage: 3000.0;
};
```

- homework
```ts
// 32a  one worker recalculates n_2: a queue
// 32b  every open screen hears it: pub/sub or Socket.IO
// 32c  store the event id once
// 32d  we have run BullMQ, not Kafka
```

- example 33. an event that already happened for b_3
- the name is past tense
- a queue gives the job to one worker. pub/sub gives a copy to each screen
- BullMQ is the queue we run. Kafka is the log we have not operated
- at-least-once means the consumer checks the event id

```ts
type Event = {
  type: "LaytimeCalculated";
  portCallId: "b_3";
  demurrage: 0;
};
```

- homework
```ts
// 33a  one worker recalculates b_3: a queue
// 33b  every open screen hears it: pub/sub or Socket.IO
// 33c  store the event id once
// 33d  we have run BullMQ, not Kafka
```

- example 34. an event that already happened for se_1
- the name is past tense
- a queue gives the job to one worker. pub/sub gives a copy to each screen
- BullMQ is the queue we run. Kafka is the log we have not operated
- at-least-once means the consumer checks the event id

```ts
type Event = {
  type: "LaytimeCalculated";
  portCallId: "se_1";
  demurrage: 28125.0;
};
```

- homework
```ts
// 34a  one worker recalculates se_1: a queue
// 34b  every open screen hears it: pub/sub or Socket.IO
// 34c  store the event id once
// 34d  we have run BullMQ, not Kafka
```

- example 35. an event that already happened for q_1
- the name is past tense
- a queue gives the job to one worker. pub/sub gives a copy to each screen
- BullMQ is the queue we run. Kafka is the log we have not operated
- at-least-once means the consumer checks the event id

```ts
type Event = {
  type: "LaytimeCalculated";
  portCallId: "q_1";
  demurrage: 0;
};
```

- homework
```ts
// 35a  one worker recalculates q_1: a queue
// 35b  every open screen hears it: pub/sub or Socket.IO
// 35c  store the event id once
// 35d  we have run BullMQ, not Kafka
```

- example 36. an event that already happened for r_1
- the name is past tense
- a queue gives the job to one worker. pub/sub gives a copy to each screen
- BullMQ is the queue we run. Kafka is the log we have not operated
- at-least-once means the consumer checks the event id

```ts
type Event = {
  type: "LaytimeCalculated";
  portCallId: "r_1";
  demurrage: 0;
};
```

- homework
```ts
// 36a  one worker recalculates r_1: a queue
// 36b  every open screen hears it: pub/sub or Socket.IO
// 36c  store the event id once
// 36d  we have run BullMQ, not Kafka
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

- example 1. one process, module for port-calls
- today the gateway is nginx and the rule stays in Nest
- a split into another service is for a real scaling or release reason
- another service does not read our tables. it calls an API or hears an event

```text
browser -> nginx -> Nest module port-calls
id pc_12 is loaded inside that module
the gateway does not compute the figure
```

- homework
```text
// 1a  modular monolith for pc_12
// 1b  a reason that would justify a split
// 1c  a shared database with two doors is still one system
```

- example 2. one process, module for matches
- today the gateway is nginx and the rule stays in Nest
- a split into another service is for a real scaling or release reason
- another service does not read our tables. it calls an API or hears an event

```text
browser -> nginx -> Nest module matches
id m_1 is loaded inside that module
the gateway does not compute the figure
```

- homework
```text
// 2a  modular monolith for m_1
// 2b  a reason that would justify a split
// 2c  a shared database with two doors is still one system
```

- example 3. one process, module for tournaments
- today the gateway is nginx and the rule stays in Nest
- a split into another service is for a real scaling or release reason
- another service does not read our tables. it calls an API or hears an event

```text
browser -> nginx -> Nest module tournaments
id t_9 is loaded inside that module
the gateway does not compute the figure
```

- homework
```text
// 3a  modular monolith for t_9
// 3b  a reason that would justify a split
// 3c  a shared database with two doors is still one system
```

- example 4. one process, module for videos
- today the gateway is nginx and the rule stays in Nest
- a split into another service is for a real scaling or release reason
- another service does not read our tables. it calls an API or hears an event

```text
browser -> nginx -> Nest module videos
id v_4 is loaded inside that module
the gateway does not compute the figure
```

- homework
```text
// 4a  modular monolith for v_4
// 4b  a reason that would justify a split
// 4c  a shared database with two doors is still one system
```

- example 5. one process, module for facts
- today the gateway is nginx and the rule stays in Nest
- a split into another service is for a real scaling or release reason
- another service does not read our tables. it calls an API or hears an event

```text
browser -> nginx -> Nest module facts
id f_2 is loaded inside that module
the gateway does not compute the figure
```

- homework
```text
// 5a  modular monolith for f_2
// 5b  a reason that would justify a split
// 5c  a shared database with two doors is still one system
```

- example 6. one process, module for users
- today the gateway is nginx and the rule stays in Nest
- a split into another service is for a real scaling or release reason
- another service does not read our tables. it calls an API or hears an event

```text
browser -> nginx -> Nest module users
id u_3 is loaded inside that module
the gateway does not compute the figure
```

- homework
```text
// 6a  modular monolith for u_3
// 6b  a reason that would justify a split
// 6c  a shared database with two doors is still one system
```

- example 7. one process, module for payments
- today the gateway is nginx and the rule stays in Nest
- a split into another service is for a real scaling or release reason
- another service does not read our tables. it calls an API or hears an event

```text
browser -> nginx -> Nest module payments
id pay_8 is loaded inside that module
the gateway does not compute the figure
```

- homework
```text
// 7a  modular monolith for pay_8
// 7b  a reason that would justify a split
// 7c  a shared database with two doors is still one system
```

- example 8. one process, module for rooms
- today the gateway is nginx and the rule stays in Nest
- a split into another service is for a real scaling or release reason
- another service does not read our tables. it calls an API or hears an event

```text
browser -> nginx -> Nest module rooms
id room_2 is loaded inside that module
the gateway does not compute the figure
```

- homework
```text
// 8a  modular monolith for room_2
// 8b  a reason that would justify a split
// 8c  a shared database with two doors is still one system
```

- example 9. one process, module for vessels
- today the gateway is nginx and the rule stays in Nest
- a split into another service is for a real scaling or release reason
- another service does not read our tables. it calls an API or hears an event

```text
browser -> nginx -> Nest module vessels
id vs_7 is loaded inside that module
the gateway does not compute the figure
```

- homework
```text
// 9a  modular monolith for vs_7
// 9b  a reason that would justify a split
// 9c  a shared database with two doors is still one system
```

- example 10. one process, module for scores
- today the gateway is nginx and the rule stays in Nest
- a split into another service is for a real scaling or release reason
- another service does not read our tables. it calls an API or hears an event

```text
browser -> nginx -> Nest module scores
id sc_3 is loaded inside that module
the gateway does not compute the figure
```

- homework
```text
// 10a  modular monolith for sc_3
// 10b  a reason that would justify a split
// 10c  a shared database with two doors is still one system
```

- example 11. one process, module for teams
- today the gateway is nginx and the rule stays in Nest
- a split into another service is for a real scaling or release reason
- another service does not read our tables. it calls an API or hears an event

```text
browser -> nginx -> Nest module teams
id tm_2 is loaded inside that module
the gateway does not compute the figure
```

- homework
```text
// 11a  modular monolith for tm_2
// 11b  a reason that would justify a split
// 11c  a shared database with two doors is still one system
```

- example 12. one process, module for uploads
- today the gateway is nginx and the rule stays in Nest
- a split into another service is for a real scaling or release reason
- another service does not read our tables. it calls an API or hears an event

```text
browser -> nginx -> Nest module uploads
id up_6 is loaded inside that module
the gateway does not compute the figure
```

- homework
```text
// 12a  modular monolith for up_6
// 12b  a reason that would justify a split
// 12c  a shared database with two doors is still one system
```

- example 13. one process, module for standings
- today the gateway is nginx and the rule stays in Nest
- a split into another service is for a real scaling or release reason
- another service does not read our tables. it calls an API or hears an event

```text
browser -> nginx -> Nest module standings
id st_1 is loaded inside that module
the gateway does not compute the figure
```

- homework
```text
// 13a  modular monolith for st_1
// 13b  a reason that would justify a split
// 13c  a shared database with two doors is still one system
```

- example 14. one process, module for notices
- today the gateway is nginx and the rule stays in Nest
- a split into another service is for a real scaling or release reason
- another service does not read our tables. it calls an API or hears an event

```text
browser -> nginx -> Nest module notices
id n_2 is loaded inside that module
the gateway does not compute the figure
```

- homework
```text
// 14a  modular monolith for n_2
// 14b  a reason that would justify a split
// 14c  a shared database with two doors is still one system
```

- example 15. one process, module for brackets
- today the gateway is nginx and the rule stays in Nest
- a split into another service is for a real scaling or release reason
- another service does not read our tables. it calls an API or hears an event

```text
browser -> nginx -> Nest module brackets
id b_3 is loaded inside that module
the gateway does not compute the figure
```

- homework
```text
// 15a  modular monolith for b_3
// 15b  a reason that would justify a split
// 15c  a shared database with two doors is still one system
```

- example 16. one process, module for sessions
- today the gateway is nginx and the rule stays in Nest
- a split into another service is for a real scaling or release reason
- another service does not read our tables. it calls an API or hears an event

```text
browser -> nginx -> Nest module sessions
id se_1 is loaded inside that module
the gateway does not compute the figure
```

- homework
```text
// 16a  modular monolith for se_1
// 16b  a reason that would justify a split
// 16c  a shared database with two doors is still one system
```

- example 17. one process, module for queues
- today the gateway is nginx and the rule stays in Nest
- a split into another service is for a real scaling or release reason
- another service does not read our tables. it calls an API or hears an event

```text
browser -> nginx -> Nest module queues
id q_1 is loaded inside that module
the gateway does not compute the figure
```

- homework
```text
// 17a  modular monolith for q_1
// 17b  a reason that would justify a split
// 17c  a shared database with two doors is still one system
```

- example 18. one process, module for roles
- today the gateway is nginx and the rule stays in Nest
- a split into another service is for a real scaling or release reason
- another service does not read our tables. it calls an API or hears an event

```text
browser -> nginx -> Nest module roles
id r_1 is loaded inside that module
the gateway does not compute the figure
```

- homework
```text
// 18a  modular monolith for r_1
// 18b  a reason that would justify a split
// 18c  a shared database with two doors is still one system
```

- example 19. one process, module for port-calls
- today the gateway is nginx and the rule stays in Nest
- a split into another service is for a real scaling or release reason
- another service does not read our tables. it calls an API or hears an event

```text
browser -> nginx -> Nest module port-calls
id pc_12 is loaded inside that module
the gateway does not compute the figure
```

- homework
```text
// 19a  modular monolith for pc_12
// 19b  a reason that would justify a split
// 19c  a shared database with two doors is still one system
```

- example 20. one process, module for matches
- today the gateway is nginx and the rule stays in Nest
- a split into another service is for a real scaling or release reason
- another service does not read our tables. it calls an API or hears an event

```text
browser -> nginx -> Nest module matches
id m_1 is loaded inside that module
the gateway does not compute the figure
```

- homework
```text
// 20a  modular monolith for m_1
// 20b  a reason that would justify a split
// 20c  a shared database with two doors is still one system
```

- example 21. one process, module for tournaments
- today the gateway is nginx and the rule stays in Nest
- a split into another service is for a real scaling or release reason
- another service does not read our tables. it calls an API or hears an event

```text
browser -> nginx -> Nest module tournaments
id t_9 is loaded inside that module
the gateway does not compute the figure
```

- homework
```text
// 21a  modular monolith for t_9
// 21b  a reason that would justify a split
// 21c  a shared database with two doors is still one system
```

- example 22. one process, module for videos
- today the gateway is nginx and the rule stays in Nest
- a split into another service is for a real scaling or release reason
- another service does not read our tables. it calls an API or hears an event

```text
browser -> nginx -> Nest module videos
id v_4 is loaded inside that module
the gateway does not compute the figure
```

- homework
```text
// 22a  modular monolith for v_4
// 22b  a reason that would justify a split
// 22c  a shared database with two doors is still one system
```

- example 23. one process, module for facts
- today the gateway is nginx and the rule stays in Nest
- a split into another service is for a real scaling or release reason
- another service does not read our tables. it calls an API or hears an event

```text
browser -> nginx -> Nest module facts
id f_2 is loaded inside that module
the gateway does not compute the figure
```

- homework
```text
// 23a  modular monolith for f_2
// 23b  a reason that would justify a split
// 23c  a shared database with two doors is still one system
```

- example 24. one process, module for users
- today the gateway is nginx and the rule stays in Nest
- a split into another service is for a real scaling or release reason
- another service does not read our tables. it calls an API or hears an event

```text
browser -> nginx -> Nest module users
id u_3 is loaded inside that module
the gateway does not compute the figure
```

- homework
```text
// 24a  modular monolith for u_3
// 24b  a reason that would justify a split
// 24c  a shared database with two doors is still one system
```

- example 25. one process, module for payments
- today the gateway is nginx and the rule stays in Nest
- a split into another service is for a real scaling or release reason
- another service does not read our tables. it calls an API or hears an event

```text
browser -> nginx -> Nest module payments
id pay_8 is loaded inside that module
the gateway does not compute the figure
```

- homework
```text
// 25a  modular monolith for pay_8
// 25b  a reason that would justify a split
// 25c  a shared database with two doors is still one system
```

- example 26. one process, module for rooms
- today the gateway is nginx and the rule stays in Nest
- a split into another service is for a real scaling or release reason
- another service does not read our tables. it calls an API or hears an event

```text
browser -> nginx -> Nest module rooms
id room_2 is loaded inside that module
the gateway does not compute the figure
```

- homework
```text
// 26a  modular monolith for room_2
// 26b  a reason that would justify a split
// 26c  a shared database with two doors is still one system
```

- example 27. one process, module for vessels
- today the gateway is nginx and the rule stays in Nest
- a split into another service is for a real scaling or release reason
- another service does not read our tables. it calls an API or hears an event

```text
browser -> nginx -> Nest module vessels
id vs_7 is loaded inside that module
the gateway does not compute the figure
```

- homework
```text
// 27a  modular monolith for vs_7
// 27b  a reason that would justify a split
// 27c  a shared database with two doors is still one system
```

- example 28. one process, module for scores
- today the gateway is nginx and the rule stays in Nest
- a split into another service is for a real scaling or release reason
- another service does not read our tables. it calls an API or hears an event

```text
browser -> nginx -> Nest module scores
id sc_3 is loaded inside that module
the gateway does not compute the figure
```

- homework
```text
// 28a  modular monolith for sc_3
// 28b  a reason that would justify a split
// 28c  a shared database with two doors is still one system
```

- example 29. one process, module for teams
- today the gateway is nginx and the rule stays in Nest
- a split into another service is for a real scaling or release reason
- another service does not read our tables. it calls an API or hears an event

```text
browser -> nginx -> Nest module teams
id tm_2 is loaded inside that module
the gateway does not compute the figure
```

- homework
```text
// 29a  modular monolith for tm_2
// 29b  a reason that would justify a split
// 29c  a shared database with two doors is still one system
```

- example 30. one process, module for uploads
- today the gateway is nginx and the rule stays in Nest
- a split into another service is for a real scaling or release reason
- another service does not read our tables. it calls an API or hears an event

```text
browser -> nginx -> Nest module uploads
id up_6 is loaded inside that module
the gateway does not compute the figure
```

- homework
```text
// 30a  modular monolith for up_6
// 30b  a reason that would justify a split
// 30c  a shared database with two doors is still one system
```

- example 31. one process, module for standings
- today the gateway is nginx and the rule stays in Nest
- a split into another service is for a real scaling or release reason
- another service does not read our tables. it calls an API or hears an event

```text
browser -> nginx -> Nest module standings
id st_1 is loaded inside that module
the gateway does not compute the figure
```

- homework
```text
// 31a  modular monolith for st_1
// 31b  a reason that would justify a split
// 31c  a shared database with two doors is still one system
```

- example 32. one process, module for notices
- today the gateway is nginx and the rule stays in Nest
- a split into another service is for a real scaling or release reason
- another service does not read our tables. it calls an API or hears an event

```text
browser -> nginx -> Nest module notices
id n_2 is loaded inside that module
the gateway does not compute the figure
```

- homework
```text
// 32a  modular monolith for n_2
// 32b  a reason that would justify a split
// 32c  a shared database with two doors is still one system
```

- example 33. one process, module for brackets
- today the gateway is nginx and the rule stays in Nest
- a split into another service is for a real scaling or release reason
- another service does not read our tables. it calls an API or hears an event

```text
browser -> nginx -> Nest module brackets
id b_3 is loaded inside that module
the gateway does not compute the figure
```

- homework
```text
// 33a  modular monolith for b_3
// 33b  a reason that would justify a split
// 33c  a shared database with two doors is still one system
```

- example 34. one process, module for sessions
- today the gateway is nginx and the rule stays in Nest
- a split into another service is for a real scaling or release reason
- another service does not read our tables. it calls an API or hears an event

```text
browser -> nginx -> Nest module sessions
id se_1 is loaded inside that module
the gateway does not compute the figure
```

- homework
```text
// 34a  modular monolith for se_1
// 34b  a reason that would justify a split
// 34c  a shared database with two doors is still one system
```

- example 35. one process, module for queues
- today the gateway is nginx and the rule stays in Nest
- a split into another service is for a real scaling or release reason
- another service does not read our tables. it calls an API or hears an event

```text
browser -> nginx -> Nest module queues
id q_1 is loaded inside that module
the gateway does not compute the figure
```

- homework
```text
// 35a  modular monolith for q_1
// 35b  a reason that would justify a split
// 35c  a shared database with two doors is still one system
```

- example 36. one process, module for roles
- today the gateway is nginx and the rule stays in Nest
- a split into another service is for a real scaling or release reason
- another service does not read our tables. it calls an API or hears an event

```text
browser -> nginx -> Nest module roles
id r_1 is loaded inside that module
the gateway does not compute the figure
```

- homework
```text
// 36a  modular monolith for r_1
// 36b  a reason that would justify a split
// 36c  a shared database with two doors is still one system
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

- example 1. limit key limit:pc_12:port-calls
- fixed window can allow almost two budgets at the boundary
- token bucket allows a burst then refills
- INCR and EXPIRE is the small version we can type
- the key includes the user and the route, not only the IP

```ts
const key = "limit:pc_12:port-calls";
const hits = await redis.incr(key);
if (hits === 1) await redis.expire(key, 60);
if (hits > 30) throw new HttpException("Too many requests", 429);
```

- homework
```ts
// 1a  key limit:pc_12:port-calls
// 1b  hit 31 in the window is 429
// 1c  a variable in one Node process is the wrong store when two processes run
```

- example 2. limit key limit:m_1:matches
- fixed window can allow almost two budgets at the boundary
- token bucket allows a burst then refills
- INCR and EXPIRE is the small version we can type
- the key includes the user and the route, not only the IP

```ts
const key = "limit:m_1:matches";
const hits = await redis.incr(key);
if (hits === 1) await redis.expire(key, 60);
if (hits > 30) throw new HttpException("Too many requests", 429);
```

- homework
```ts
// 2a  key limit:m_1:matches
// 2b  hit 31 in the window is 429
// 2c  a variable in one Node process is the wrong store when two processes run
```

- example 3. limit key limit:t_9:tournaments
- fixed window can allow almost two budgets at the boundary
- token bucket allows a burst then refills
- INCR and EXPIRE is the small version we can type
- the key includes the user and the route, not only the IP

```ts
const key = "limit:t_9:tournaments";
const hits = await redis.incr(key);
if (hits === 1) await redis.expire(key, 60);
if (hits > 30) throw new HttpException("Too many requests", 429);
```

- homework
```ts
// 3a  key limit:t_9:tournaments
// 3b  hit 31 in the window is 429
// 3c  a variable in one Node process is the wrong store when two processes run
```

- example 4. limit key limit:v_4:videos
- fixed window can allow almost two budgets at the boundary
- token bucket allows a burst then refills
- INCR and EXPIRE is the small version we can type
- the key includes the user and the route, not only the IP

```ts
const key = "limit:v_4:videos";
const hits = await redis.incr(key);
if (hits === 1) await redis.expire(key, 60);
if (hits > 30) throw new HttpException("Too many requests", 429);
```

- homework
```ts
// 4a  key limit:v_4:videos
// 4b  hit 31 in the window is 429
// 4c  a variable in one Node process is the wrong store when two processes run
```

- example 5. limit key limit:f_2:facts
- fixed window can allow almost two budgets at the boundary
- token bucket allows a burst then refills
- INCR and EXPIRE is the small version we can type
- the key includes the user and the route, not only the IP

```ts
const key = "limit:f_2:facts";
const hits = await redis.incr(key);
if (hits === 1) await redis.expire(key, 60);
if (hits > 30) throw new HttpException("Too many requests", 429);
```

- homework
```ts
// 5a  key limit:f_2:facts
// 5b  hit 31 in the window is 429
// 5c  a variable in one Node process is the wrong store when two processes run
```

- example 6. limit key limit:u_3:users
- fixed window can allow almost two budgets at the boundary
- token bucket allows a burst then refills
- INCR and EXPIRE is the small version we can type
- the key includes the user and the route, not only the IP

```ts
const key = "limit:u_3:users";
const hits = await redis.incr(key);
if (hits === 1) await redis.expire(key, 60);
if (hits > 30) throw new HttpException("Too many requests", 429);
```

- homework
```ts
// 6a  key limit:u_3:users
// 6b  hit 31 in the window is 429
// 6c  a variable in one Node process is the wrong store when two processes run
```

- example 7. limit key limit:pay_8:payments
- fixed window can allow almost two budgets at the boundary
- token bucket allows a burst then refills
- INCR and EXPIRE is the small version we can type
- the key includes the user and the route, not only the IP

```ts
const key = "limit:pay_8:payments";
const hits = await redis.incr(key);
if (hits === 1) await redis.expire(key, 60);
if (hits > 30) throw new HttpException("Too many requests", 429);
```

- homework
```ts
// 7a  key limit:pay_8:payments
// 7b  hit 31 in the window is 429
// 7c  a variable in one Node process is the wrong store when two processes run
```

- example 8. limit key limit:room_2:rooms
- fixed window can allow almost two budgets at the boundary
- token bucket allows a burst then refills
- INCR and EXPIRE is the small version we can type
- the key includes the user and the route, not only the IP

```ts
const key = "limit:room_2:rooms";
const hits = await redis.incr(key);
if (hits === 1) await redis.expire(key, 60);
if (hits > 30) throw new HttpException("Too many requests", 429);
```

- homework
```ts
// 8a  key limit:room_2:rooms
// 8b  hit 31 in the window is 429
// 8c  a variable in one Node process is the wrong store when two processes run
```

- example 9. limit key limit:vs_7:vessels
- fixed window can allow almost two budgets at the boundary
- token bucket allows a burst then refills
- INCR and EXPIRE is the small version we can type
- the key includes the user and the route, not only the IP

```ts
const key = "limit:vs_7:vessels";
const hits = await redis.incr(key);
if (hits === 1) await redis.expire(key, 60);
if (hits > 30) throw new HttpException("Too many requests", 429);
```

- homework
```ts
// 9a  key limit:vs_7:vessels
// 9b  hit 31 in the window is 429
// 9c  a variable in one Node process is the wrong store when two processes run
```

- example 10. limit key limit:sc_3:scores
- fixed window can allow almost two budgets at the boundary
- token bucket allows a burst then refills
- INCR and EXPIRE is the small version we can type
- the key includes the user and the route, not only the IP

```ts
const key = "limit:sc_3:scores";
const hits = await redis.incr(key);
if (hits === 1) await redis.expire(key, 60);
if (hits > 30) throw new HttpException("Too many requests", 429);
```

- homework
```ts
// 10a  key limit:sc_3:scores
// 10b  hit 31 in the window is 429
// 10c  a variable in one Node process is the wrong store when two processes run
```

- example 11. limit key limit:tm_2:teams
- fixed window can allow almost two budgets at the boundary
- token bucket allows a burst then refills
- INCR and EXPIRE is the small version we can type
- the key includes the user and the route, not only the IP

```ts
const key = "limit:tm_2:teams";
const hits = await redis.incr(key);
if (hits === 1) await redis.expire(key, 60);
if (hits > 30) throw new HttpException("Too many requests", 429);
```

- homework
```ts
// 11a  key limit:tm_2:teams
// 11b  hit 31 in the window is 429
// 11c  a variable in one Node process is the wrong store when two processes run
```

- example 12. limit key limit:up_6:uploads
- fixed window can allow almost two budgets at the boundary
- token bucket allows a burst then refills
- INCR and EXPIRE is the small version we can type
- the key includes the user and the route, not only the IP

```ts
const key = "limit:up_6:uploads";
const hits = await redis.incr(key);
if (hits === 1) await redis.expire(key, 60);
if (hits > 30) throw new HttpException("Too many requests", 429);
```

- homework
```ts
// 12a  key limit:up_6:uploads
// 12b  hit 31 in the window is 429
// 12c  a variable in one Node process is the wrong store when two processes run
```

- example 13. limit key limit:st_1:standings
- fixed window can allow almost two budgets at the boundary
- token bucket allows a burst then refills
- INCR and EXPIRE is the small version we can type
- the key includes the user and the route, not only the IP

```ts
const key = "limit:st_1:standings";
const hits = await redis.incr(key);
if (hits === 1) await redis.expire(key, 60);
if (hits > 30) throw new HttpException("Too many requests", 429);
```

- homework
```ts
// 13a  key limit:st_1:standings
// 13b  hit 31 in the window is 429
// 13c  a variable in one Node process is the wrong store when two processes run
```

- example 14. limit key limit:n_2:notices
- fixed window can allow almost two budgets at the boundary
- token bucket allows a burst then refills
- INCR and EXPIRE is the small version we can type
- the key includes the user and the route, not only the IP

```ts
const key = "limit:n_2:notices";
const hits = await redis.incr(key);
if (hits === 1) await redis.expire(key, 60);
if (hits > 30) throw new HttpException("Too many requests", 429);
```

- homework
```ts
// 14a  key limit:n_2:notices
// 14b  hit 31 in the window is 429
// 14c  a variable in one Node process is the wrong store when two processes run
```

- example 15. limit key limit:b_3:brackets
- fixed window can allow almost two budgets at the boundary
- token bucket allows a burst then refills
- INCR and EXPIRE is the small version we can type
- the key includes the user and the route, not only the IP

```ts
const key = "limit:b_3:brackets";
const hits = await redis.incr(key);
if (hits === 1) await redis.expire(key, 60);
if (hits > 30) throw new HttpException("Too many requests", 429);
```

- homework
```ts
// 15a  key limit:b_3:brackets
// 15b  hit 31 in the window is 429
// 15c  a variable in one Node process is the wrong store when two processes run
```

- example 16. limit key limit:se_1:sessions
- fixed window can allow almost two budgets at the boundary
- token bucket allows a burst then refills
- INCR and EXPIRE is the small version we can type
- the key includes the user and the route, not only the IP

```ts
const key = "limit:se_1:sessions";
const hits = await redis.incr(key);
if (hits === 1) await redis.expire(key, 60);
if (hits > 30) throw new HttpException("Too many requests", 429);
```

- homework
```ts
// 16a  key limit:se_1:sessions
// 16b  hit 31 in the window is 429
// 16c  a variable in one Node process is the wrong store when two processes run
```

- example 17. limit key limit:q_1:queues
- fixed window can allow almost two budgets at the boundary
- token bucket allows a burst then refills
- INCR and EXPIRE is the small version we can type
- the key includes the user and the route, not only the IP

```ts
const key = "limit:q_1:queues";
const hits = await redis.incr(key);
if (hits === 1) await redis.expire(key, 60);
if (hits > 30) throw new HttpException("Too many requests", 429);
```

- homework
```ts
// 17a  key limit:q_1:queues
// 17b  hit 31 in the window is 429
// 17c  a variable in one Node process is the wrong store when two processes run
```

- example 18. limit key limit:r_1:roles
- fixed window can allow almost two budgets at the boundary
- token bucket allows a burst then refills
- INCR and EXPIRE is the small version we can type
- the key includes the user and the route, not only the IP

```ts
const key = "limit:r_1:roles";
const hits = await redis.incr(key);
if (hits === 1) await redis.expire(key, 60);
if (hits > 30) throw new HttpException("Too many requests", 429);
```

- homework
```ts
// 18a  key limit:r_1:roles
// 18b  hit 31 in the window is 429
// 18c  a variable in one Node process is the wrong store when two processes run
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

- example 1. bloom bit and circuit for pc_12
- if any of the hashed bits is 0, pc_12 was never added. skip the disk
- if all are 1, maybe. check Postgres
- five timeouts open the circuit. calls fail fast. they do not invent success
- half-open lets one trial through

```text
bits for pc_12: if a bit is 0, definitely absent
circuit: 5 failures -> open 30s -> one trial -> closed or open again
```

- homework
```text
// 1a  bloom says no for pc_12. do we read Postgres
// 1b  bloom says maybe. we read Postgres
// 1c  an open circuit does not mark payment pc_12 as paid
```

- example 2. bloom bit and circuit for m_1
- if any of the hashed bits is 0, m_1 was never added. skip the disk
- if all are 1, maybe. check Postgres
- five timeouts open the circuit. calls fail fast. they do not invent success
- half-open lets one trial through

```text
bits for m_1: if a bit is 0, definitely absent
circuit: 5 failures -> open 30s -> one trial -> closed or open again
```

- homework
```text
// 2a  bloom says no for m_1. do we read Postgres
// 2b  bloom says maybe. we read Postgres
// 2c  an open circuit does not mark payment m_1 as paid
```

- example 3. bloom bit and circuit for t_9
- if any of the hashed bits is 0, t_9 was never added. skip the disk
- if all are 1, maybe. check Postgres
- five timeouts open the circuit. calls fail fast. they do not invent success
- half-open lets one trial through

```text
bits for t_9: if a bit is 0, definitely absent
circuit: 5 failures -> open 30s -> one trial -> closed or open again
```

- homework
```text
// 3a  bloom says no for t_9. do we read Postgres
// 3b  bloom says maybe. we read Postgres
// 3c  an open circuit does not mark payment t_9 as paid
```

- example 4. bloom bit and circuit for v_4
- if any of the hashed bits is 0, v_4 was never added. skip the disk
- if all are 1, maybe. check Postgres
- five timeouts open the circuit. calls fail fast. they do not invent success
- half-open lets one trial through

```text
bits for v_4: if a bit is 0, definitely absent
circuit: 5 failures -> open 30s -> one trial -> closed or open again
```

- homework
```text
// 4a  bloom says no for v_4. do we read Postgres
// 4b  bloom says maybe. we read Postgres
// 4c  an open circuit does not mark payment v_4 as paid
```

- example 5. bloom bit and circuit for f_2
- if any of the hashed bits is 0, f_2 was never added. skip the disk
- if all are 1, maybe. check Postgres
- five timeouts open the circuit. calls fail fast. they do not invent success
- half-open lets one trial through

```text
bits for f_2: if a bit is 0, definitely absent
circuit: 5 failures -> open 30s -> one trial -> closed or open again
```

- homework
```text
// 5a  bloom says no for f_2. do we read Postgres
// 5b  bloom says maybe. we read Postgres
// 5c  an open circuit does not mark payment f_2 as paid
```

- example 6. bloom bit and circuit for u_3
- if any of the hashed bits is 0, u_3 was never added. skip the disk
- if all are 1, maybe. check Postgres
- five timeouts open the circuit. calls fail fast. they do not invent success
- half-open lets one trial through

```text
bits for u_3: if a bit is 0, definitely absent
circuit: 5 failures -> open 30s -> one trial -> closed or open again
```

- homework
```text
// 6a  bloom says no for u_3. do we read Postgres
// 6b  bloom says maybe. we read Postgres
// 6c  an open circuit does not mark payment u_3 as paid
```

- example 7. bloom bit and circuit for pay_8
- if any of the hashed bits is 0, pay_8 was never added. skip the disk
- if all are 1, maybe. check Postgres
- five timeouts open the circuit. calls fail fast. they do not invent success
- half-open lets one trial through

```text
bits for pay_8: if a bit is 0, definitely absent
circuit: 5 failures -> open 30s -> one trial -> closed or open again
```

- homework
```text
// 7a  bloom says no for pay_8. do we read Postgres
// 7b  bloom says maybe. we read Postgres
// 7c  an open circuit does not mark payment pay_8 as paid
```

- example 8. bloom bit and circuit for room_2
- if any of the hashed bits is 0, room_2 was never added. skip the disk
- if all are 1, maybe. check Postgres
- five timeouts open the circuit. calls fail fast. they do not invent success
- half-open lets one trial through

```text
bits for room_2: if a bit is 0, definitely absent
circuit: 5 failures -> open 30s -> one trial -> closed or open again
```

- homework
```text
// 8a  bloom says no for room_2. do we read Postgres
// 8b  bloom says maybe. we read Postgres
// 8c  an open circuit does not mark payment room_2 as paid
```

- example 9. bloom bit and circuit for vs_7
- if any of the hashed bits is 0, vs_7 was never added. skip the disk
- if all are 1, maybe. check Postgres
- five timeouts open the circuit. calls fail fast. they do not invent success
- half-open lets one trial through

```text
bits for vs_7: if a bit is 0, definitely absent
circuit: 5 failures -> open 30s -> one trial -> closed or open again
```

- homework
```text
// 9a  bloom says no for vs_7. do we read Postgres
// 9b  bloom says maybe. we read Postgres
// 9c  an open circuit does not mark payment vs_7 as paid
```

- example 10. bloom bit and circuit for sc_3
- if any of the hashed bits is 0, sc_3 was never added. skip the disk
- if all are 1, maybe. check Postgres
- five timeouts open the circuit. calls fail fast. they do not invent success
- half-open lets one trial through

```text
bits for sc_3: if a bit is 0, definitely absent
circuit: 5 failures -> open 30s -> one trial -> closed or open again
```

- homework
```text
// 10a  bloom says no for sc_3. do we read Postgres
// 10b  bloom says maybe. we read Postgres
// 10c  an open circuit does not mark payment sc_3 as paid
```

- example 11. bloom bit and circuit for tm_2
- if any of the hashed bits is 0, tm_2 was never added. skip the disk
- if all are 1, maybe. check Postgres
- five timeouts open the circuit. calls fail fast. they do not invent success
- half-open lets one trial through

```text
bits for tm_2: if a bit is 0, definitely absent
circuit: 5 failures -> open 30s -> one trial -> closed or open again
```

- homework
```text
// 11a  bloom says no for tm_2. do we read Postgres
// 11b  bloom says maybe. we read Postgres
// 11c  an open circuit does not mark payment tm_2 as paid
```

- example 12. bloom bit and circuit for up_6
- if any of the hashed bits is 0, up_6 was never added. skip the disk
- if all are 1, maybe. check Postgres
- five timeouts open the circuit. calls fail fast. they do not invent success
- half-open lets one trial through

```text
bits for up_6: if a bit is 0, definitely absent
circuit: 5 failures -> open 30s -> one trial -> closed or open again
```

- homework
```text
// 12a  bloom says no for up_6. do we read Postgres
// 12b  bloom says maybe. we read Postgres
// 12c  an open circuit does not mark payment up_6 as paid
```

- example 13. bloom bit and circuit for st_1
- if any of the hashed bits is 0, st_1 was never added. skip the disk
- if all are 1, maybe. check Postgres
- five timeouts open the circuit. calls fail fast. they do not invent success
- half-open lets one trial through

```text
bits for st_1: if a bit is 0, definitely absent
circuit: 5 failures -> open 30s -> one trial -> closed or open again
```

- homework
```text
// 13a  bloom says no for st_1. do we read Postgres
// 13b  bloom says maybe. we read Postgres
// 13c  an open circuit does not mark payment st_1 as paid
```

- example 14. bloom bit and circuit for n_2
- if any of the hashed bits is 0, n_2 was never added. skip the disk
- if all are 1, maybe. check Postgres
- five timeouts open the circuit. calls fail fast. they do not invent success
- half-open lets one trial through

```text
bits for n_2: if a bit is 0, definitely absent
circuit: 5 failures -> open 30s -> one trial -> closed or open again
```

- homework
```text
// 14a  bloom says no for n_2. do we read Postgres
// 14b  bloom says maybe. we read Postgres
// 14c  an open circuit does not mark payment n_2 as paid
```

- example 15. bloom bit and circuit for b_3
- if any of the hashed bits is 0, b_3 was never added. skip the disk
- if all are 1, maybe. check Postgres
- five timeouts open the circuit. calls fail fast. they do not invent success
- half-open lets one trial through

```text
bits for b_3: if a bit is 0, definitely absent
circuit: 5 failures -> open 30s -> one trial -> closed or open again
```

- homework
```text
// 15a  bloom says no for b_3. do we read Postgres
// 15b  bloom says maybe. we read Postgres
// 15c  an open circuit does not mark payment b_3 as paid
```

- example 16. bloom bit and circuit for se_1
- if any of the hashed bits is 0, se_1 was never added. skip the disk
- if all are 1, maybe. check Postgres
- five timeouts open the circuit. calls fail fast. they do not invent success
- half-open lets one trial through

```text
bits for se_1: if a bit is 0, definitely absent
circuit: 5 failures -> open 30s -> one trial -> closed or open again
```

- homework
```text
// 16a  bloom says no for se_1. do we read Postgres
// 16b  bloom says maybe. we read Postgres
// 16c  an open circuit does not mark payment se_1 as paid
```

- example 17. bloom bit and circuit for q_1
- if any of the hashed bits is 0, q_1 was never added. skip the disk
- if all are 1, maybe. check Postgres
- five timeouts open the circuit. calls fail fast. they do not invent success
- half-open lets one trial through

```text
bits for q_1: if a bit is 0, definitely absent
circuit: 5 failures -> open 30s -> one trial -> closed or open again
```

- homework
```text
// 17a  bloom says no for q_1. do we read Postgres
// 17b  bloom says maybe. we read Postgres
// 17c  an open circuit does not mark payment q_1 as paid
```

- example 18. bloom bit and circuit for r_1
- if any of the hashed bits is 0, r_1 was never added. skip the disk
- if all are 1, maybe. check Postgres
- five timeouts open the circuit. calls fail fast. they do not invent success
- half-open lets one trial through

```text
bits for r_1: if a bit is 0, definitely absent
circuit: 5 failures -> open 30s -> one trial -> closed or open again
```

- homework
```text
// 18a  bloom says no for r_1. do we read Postgres
// 18b  bloom says maybe. we read Postgres
// 18c  an open circuit does not mark payment r_1 as paid
```

- example 19. bloom bit and circuit for pc_12
- if any of the hashed bits is 0, pc_12 was never added. skip the disk
- if all are 1, maybe. check Postgres
- five timeouts open the circuit. calls fail fast. they do not invent success
- half-open lets one trial through

```text
bits for pc_12: if a bit is 0, definitely absent
circuit: 5 failures -> open 30s -> one trial -> closed or open again
```

- homework
```text
// 19a  bloom says no for pc_12. do we read Postgres
// 19b  bloom says maybe. we read Postgres
// 19c  an open circuit does not mark payment pc_12 as paid
```

- example 20. bloom bit and circuit for m_1
- if any of the hashed bits is 0, m_1 was never added. skip the disk
- if all are 1, maybe. check Postgres
- five timeouts open the circuit. calls fail fast. they do not invent success
- half-open lets one trial through

```text
bits for m_1: if a bit is 0, definitely absent
circuit: 5 failures -> open 30s -> one trial -> closed or open again
```

- homework
```text
// 20a  bloom says no for m_1. do we read Postgres
// 20b  bloom says maybe. we read Postgres
// 20c  an open circuit does not mark payment m_1 as paid
```

- example 21. bloom bit and circuit for t_9
- if any of the hashed bits is 0, t_9 was never added. skip the disk
- if all are 1, maybe. check Postgres
- five timeouts open the circuit. calls fail fast. they do not invent success
- half-open lets one trial through

```text
bits for t_9: if a bit is 0, definitely absent
circuit: 5 failures -> open 30s -> one trial -> closed or open again
```

- homework
```text
// 21a  bloom says no for t_9. do we read Postgres
// 21b  bloom says maybe. we read Postgres
// 21c  an open circuit does not mark payment t_9 as paid
```

- example 22. bloom bit and circuit for v_4
- if any of the hashed bits is 0, v_4 was never added. skip the disk
- if all are 1, maybe. check Postgres
- five timeouts open the circuit. calls fail fast. they do not invent success
- half-open lets one trial through

```text
bits for v_4: if a bit is 0, definitely absent
circuit: 5 failures -> open 30s -> one trial -> closed or open again
```

- homework
```text
// 22a  bloom says no for v_4. do we read Postgres
// 22b  bloom says maybe. we read Postgres
// 22c  an open circuit does not mark payment v_4 as paid
```

- example 23. bloom bit and circuit for f_2
- if any of the hashed bits is 0, f_2 was never added. skip the disk
- if all are 1, maybe. check Postgres
- five timeouts open the circuit. calls fail fast. they do not invent success
- half-open lets one trial through

```text
bits for f_2: if a bit is 0, definitely absent
circuit: 5 failures -> open 30s -> one trial -> closed or open again
```

- homework
```text
// 23a  bloom says no for f_2. do we read Postgres
// 23b  bloom says maybe. we read Postgres
// 23c  an open circuit does not mark payment f_2 as paid
```

- example 24. bloom bit and circuit for u_3
- if any of the hashed bits is 0, u_3 was never added. skip the disk
- if all are 1, maybe. check Postgres
- five timeouts open the circuit. calls fail fast. they do not invent success
- half-open lets one trial through

```text
bits for u_3: if a bit is 0, definitely absent
circuit: 5 failures -> open 30s -> one trial -> closed or open again
```

- homework
```text
// 24a  bloom says no for u_3. do we read Postgres
// 24b  bloom says maybe. we read Postgres
// 24c  an open circuit does not mark payment u_3 as paid
```

- example 25. bloom bit and circuit for pay_8
- if any of the hashed bits is 0, pay_8 was never added. skip the disk
- if all are 1, maybe. check Postgres
- five timeouts open the circuit. calls fail fast. they do not invent success
- half-open lets one trial through

```text
bits for pay_8: if a bit is 0, definitely absent
circuit: 5 failures -> open 30s -> one trial -> closed or open again
```

- homework
```text
// 25a  bloom says no for pay_8. do we read Postgres
// 25b  bloom says maybe. we read Postgres
// 25c  an open circuit does not mark payment pay_8 as paid
```

- example 26. bloom bit and circuit for room_2
- if any of the hashed bits is 0, room_2 was never added. skip the disk
- if all are 1, maybe. check Postgres
- five timeouts open the circuit. calls fail fast. they do not invent success
- half-open lets one trial through

```text
bits for room_2: if a bit is 0, definitely absent
circuit: 5 failures -> open 30s -> one trial -> closed or open again
```

- homework
```text
// 26a  bloom says no for room_2. do we read Postgres
// 26b  bloom says maybe. we read Postgres
// 26c  an open circuit does not mark payment room_2 as paid
```

- example 27. bloom bit and circuit for vs_7
- if any of the hashed bits is 0, vs_7 was never added. skip the disk
- if all are 1, maybe. check Postgres
- five timeouts open the circuit. calls fail fast. they do not invent success
- half-open lets one trial through

```text
bits for vs_7: if a bit is 0, definitely absent
circuit: 5 failures -> open 30s -> one trial -> closed or open again
```

- homework
```text
// 27a  bloom says no for vs_7. do we read Postgres
// 27b  bloom says maybe. we read Postgres
// 27c  an open circuit does not mark payment vs_7 as paid
```

- example 28. bloom bit and circuit for sc_3
- if any of the hashed bits is 0, sc_3 was never added. skip the disk
- if all are 1, maybe. check Postgres
- five timeouts open the circuit. calls fail fast. they do not invent success
- half-open lets one trial through

```text
bits for sc_3: if a bit is 0, definitely absent
circuit: 5 failures -> open 30s -> one trial -> closed or open again
```

- homework
```text
// 28a  bloom says no for sc_3. do we read Postgres
// 28b  bloom says maybe. we read Postgres
// 28c  an open circuit does not mark payment sc_3 as paid
```

- example 29. bloom bit and circuit for tm_2
- if any of the hashed bits is 0, tm_2 was never added. skip the disk
- if all are 1, maybe. check Postgres
- five timeouts open the circuit. calls fail fast. they do not invent success
- half-open lets one trial through

```text
bits for tm_2: if a bit is 0, definitely absent
circuit: 5 failures -> open 30s -> one trial -> closed or open again
```

- homework
```text
// 29a  bloom says no for tm_2. do we read Postgres
// 29b  bloom says maybe. we read Postgres
// 29c  an open circuit does not mark payment tm_2 as paid
```

- example 30. bloom bit and circuit for up_6
- if any of the hashed bits is 0, up_6 was never added. skip the disk
- if all are 1, maybe. check Postgres
- five timeouts open the circuit. calls fail fast. they do not invent success
- half-open lets one trial through

```text
bits for up_6: if a bit is 0, definitely absent
circuit: 5 failures -> open 30s -> one trial -> closed or open again
```

- homework
```text
// 30a  bloom says no for up_6. do we read Postgres
// 30b  bloom says maybe. we read Postgres
// 30c  an open circuit does not mark payment up_6 as paid
```

- example 31. bloom bit and circuit for st_1
- if any of the hashed bits is 0, st_1 was never added. skip the disk
- if all are 1, maybe. check Postgres
- five timeouts open the circuit. calls fail fast. they do not invent success
- half-open lets one trial through

```text
bits for st_1: if a bit is 0, definitely absent
circuit: 5 failures -> open 30s -> one trial -> closed or open again
```

- homework
```text
// 31a  bloom says no for st_1. do we read Postgres
// 31b  bloom says maybe. we read Postgres
// 31c  an open circuit does not mark payment st_1 as paid
```

- example 32. bloom bit and circuit for n_2
- if any of the hashed bits is 0, n_2 was never added. skip the disk
- if all are 1, maybe. check Postgres
- five timeouts open the circuit. calls fail fast. they do not invent success
- half-open lets one trial through

```text
bits for n_2: if a bit is 0, definitely absent
circuit: 5 failures -> open 30s -> one trial -> closed or open again
```

- homework
```text
// 32a  bloom says no for n_2. do we read Postgres
// 32b  bloom says maybe. we read Postgres
// 32c  an open circuit does not mark payment n_2 as paid
```

- example 33. bloom bit and circuit for b_3
- if any of the hashed bits is 0, b_3 was never added. skip the disk
- if all are 1, maybe. check Postgres
- five timeouts open the circuit. calls fail fast. they do not invent success
- half-open lets one trial through

```text
bits for b_3: if a bit is 0, definitely absent
circuit: 5 failures -> open 30s -> one trial -> closed or open again
```

- homework
```text
// 33a  bloom says no for b_3. do we read Postgres
// 33b  bloom says maybe. we read Postgres
// 33c  an open circuit does not mark payment b_3 as paid
```

- example 34. bloom bit and circuit for se_1
- if any of the hashed bits is 0, se_1 was never added. skip the disk
- if all are 1, maybe. check Postgres
- five timeouts open the circuit. calls fail fast. they do not invent success
- half-open lets one trial through

```text
bits for se_1: if a bit is 0, definitely absent
circuit: 5 failures -> open 30s -> one trial -> closed or open again
```

- homework
```text
// 34a  bloom says no for se_1. do we read Postgres
// 34b  bloom says maybe. we read Postgres
// 34c  an open circuit does not mark payment se_1 as paid
```

- example 35. bloom bit and circuit for q_1
- if any of the hashed bits is 0, q_1 was never added. skip the disk
- if all are 1, maybe. check Postgres
- five timeouts open the circuit. calls fail fast. they do not invent success
- half-open lets one trial through

```text
bits for q_1: if a bit is 0, definitely absent
circuit: 5 failures -> open 30s -> one trial -> closed or open again
```

- homework
```text
// 35a  bloom says no for q_1. do we read Postgres
// 35b  bloom says maybe. we read Postgres
// 35c  an open circuit does not mark payment q_1 as paid
```

- example 36. bloom bit and circuit for r_1
- if any of the hashed bits is 0, r_1 was never added. skip the disk
- if all are 1, maybe. check Postgres
- five timeouts open the circuit. calls fail fast. they do not invent success
- half-open lets one trial through

```text
bits for r_1: if a bit is 0, definitely absent
circuit: 5 failures -> open 30s -> one trial -> closed or open again
```

- homework
```text
// 36a  bloom says no for r_1. do we read Postgres
// 36b  bloom says maybe. we read Postgres
// 36c  an open circuit does not mark payment r_1 as paid
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

- example 1. saga versus transaction for pc_12
- the row and its first child are one Postgres transaction, not a saga
- a saga is for Stripe plus our row, with a compensating step
- the object key holds bytes. Postgres holds owner and key pc_12
- compensation is idempotent. a retry does not refund twice

```text
pending row pc_12
charge
mark paid
if the mark never happens, a worker asks what the charge did
object key stores the file. the row stores the key
```

- homework
```text
// 1a  why pc_12 and its child are not a saga
// 1b  compensate a failed reserve after a charge
// 1c  who may watch is the database check, not the object store alone
```

- example 2. saga versus transaction for m_1
- the row and its first child are one Postgres transaction, not a saga
- a saga is for Stripe plus our row, with a compensating step
- the object key holds bytes. Postgres holds owner and key m_1
- compensation is idempotent. a retry does not refund twice

```text
pending row m_1
charge
mark paid
if the mark never happens, a worker asks what the charge did
object key stores the file. the row stores the key
```

- homework
```text
// 2a  why m_1 and its child are not a saga
// 2b  compensate a failed reserve after a charge
// 2c  who may watch is the database check, not the object store alone
```

- example 3. saga versus transaction for t_9
- the row and its first child are one Postgres transaction, not a saga
- a saga is for Stripe plus our row, with a compensating step
- the object key holds bytes. Postgres holds owner and key t_9
- compensation is idempotent. a retry does not refund twice

```text
pending row t_9
charge
mark paid
if the mark never happens, a worker asks what the charge did
object key stores the file. the row stores the key
```

- homework
```text
// 3a  why t_9 and its child are not a saga
// 3b  compensate a failed reserve after a charge
// 3c  who may watch is the database check, not the object store alone
```

- example 4. saga versus transaction for v_4
- the row and its first child are one Postgres transaction, not a saga
- a saga is for Stripe plus our row, with a compensating step
- the object key holds bytes. Postgres holds owner and key v_4
- compensation is idempotent. a retry does not refund twice

```text
pending row v_4
charge
mark paid
if the mark never happens, a worker asks what the charge did
object key stores the file. the row stores the key
```

- homework
```text
// 4a  why v_4 and its child are not a saga
// 4b  compensate a failed reserve after a charge
// 4c  who may watch is the database check, not the object store alone
```

- example 5. saga versus transaction for f_2
- the row and its first child are one Postgres transaction, not a saga
- a saga is for Stripe plus our row, with a compensating step
- the object key holds bytes. Postgres holds owner and key f_2
- compensation is idempotent. a retry does not refund twice

```text
pending row f_2
charge
mark paid
if the mark never happens, a worker asks what the charge did
object key stores the file. the row stores the key
```

- homework
```text
// 5a  why f_2 and its child are not a saga
// 5b  compensate a failed reserve after a charge
// 5c  who may watch is the database check, not the object store alone
```

- example 6. saga versus transaction for u_3
- the row and its first child are one Postgres transaction, not a saga
- a saga is for Stripe plus our row, with a compensating step
- the object key holds bytes. Postgres holds owner and key u_3
- compensation is idempotent. a retry does not refund twice

```text
pending row u_3
charge
mark paid
if the mark never happens, a worker asks what the charge did
object key stores the file. the row stores the key
```

- homework
```text
// 6a  why u_3 and its child are not a saga
// 6b  compensate a failed reserve after a charge
// 6c  who may watch is the database check, not the object store alone
```

- example 7. saga versus transaction for pay_8
- the row and its first child are one Postgres transaction, not a saga
- a saga is for Stripe plus our row, with a compensating step
- the object key holds bytes. Postgres holds owner and key pay_8
- compensation is idempotent. a retry does not refund twice

```text
pending row pay_8
charge
mark paid
if the mark never happens, a worker asks what the charge did
object key stores the file. the row stores the key
```

- homework
```text
// 7a  why pay_8 and its child are not a saga
// 7b  compensate a failed reserve after a charge
// 7c  who may watch is the database check, not the object store alone
```

- example 8. saga versus transaction for room_2
- the row and its first child are one Postgres transaction, not a saga
- a saga is for Stripe plus our row, with a compensating step
- the object key holds bytes. Postgres holds owner and key room_2
- compensation is idempotent. a retry does not refund twice

```text
pending row room_2
charge
mark paid
if the mark never happens, a worker asks what the charge did
object key stores the file. the row stores the key
```

- homework
```text
// 8a  why room_2 and its child are not a saga
// 8b  compensate a failed reserve after a charge
// 8c  who may watch is the database check, not the object store alone
```

- example 9. saga versus transaction for vs_7
- the row and its first child are one Postgres transaction, not a saga
- a saga is for Stripe plus our row, with a compensating step
- the object key holds bytes. Postgres holds owner and key vs_7
- compensation is idempotent. a retry does not refund twice

```text
pending row vs_7
charge
mark paid
if the mark never happens, a worker asks what the charge did
object key stores the file. the row stores the key
```

- homework
```text
// 9a  why vs_7 and its child are not a saga
// 9b  compensate a failed reserve after a charge
// 9c  who may watch is the database check, not the object store alone
```

- example 10. saga versus transaction for sc_3
- the row and its first child are one Postgres transaction, not a saga
- a saga is for Stripe plus our row, with a compensating step
- the object key holds bytes. Postgres holds owner and key sc_3
- compensation is idempotent. a retry does not refund twice

```text
pending row sc_3
charge
mark paid
if the mark never happens, a worker asks what the charge did
object key stores the file. the row stores the key
```

- homework
```text
// 10a  why sc_3 and its child are not a saga
// 10b  compensate a failed reserve after a charge
// 10c  who may watch is the database check, not the object store alone
```

- example 11. saga versus transaction for tm_2
- the row and its first child are one Postgres transaction, not a saga
- a saga is for Stripe plus our row, with a compensating step
- the object key holds bytes. Postgres holds owner and key tm_2
- compensation is idempotent. a retry does not refund twice

```text
pending row tm_2
charge
mark paid
if the mark never happens, a worker asks what the charge did
object key stores the file. the row stores the key
```

- homework
```text
// 11a  why tm_2 and its child are not a saga
// 11b  compensate a failed reserve after a charge
// 11c  who may watch is the database check, not the object store alone
```

- example 12. saga versus transaction for up_6
- the row and its first child are one Postgres transaction, not a saga
- a saga is for Stripe plus our row, with a compensating step
- the object key holds bytes. Postgres holds owner and key up_6
- compensation is idempotent. a retry does not refund twice

```text
pending row up_6
charge
mark paid
if the mark never happens, a worker asks what the charge did
object key stores the file. the row stores the key
```

- homework
```text
// 12a  why up_6 and its child are not a saga
// 12b  compensate a failed reserve after a charge
// 12c  who may watch is the database check, not the object store alone
```

- example 13. saga versus transaction for st_1
- the row and its first child are one Postgres transaction, not a saga
- a saga is for Stripe plus our row, with a compensating step
- the object key holds bytes. Postgres holds owner and key st_1
- compensation is idempotent. a retry does not refund twice

```text
pending row st_1
charge
mark paid
if the mark never happens, a worker asks what the charge did
object key stores the file. the row stores the key
```

- homework
```text
// 13a  why st_1 and its child are not a saga
// 13b  compensate a failed reserve after a charge
// 13c  who may watch is the database check, not the object store alone
```

- example 14. saga versus transaction for n_2
- the row and its first child are one Postgres transaction, not a saga
- a saga is for Stripe plus our row, with a compensating step
- the object key holds bytes. Postgres holds owner and key n_2
- compensation is idempotent. a retry does not refund twice

```text
pending row n_2
charge
mark paid
if the mark never happens, a worker asks what the charge did
object key stores the file. the row stores the key
```

- homework
```text
// 14a  why n_2 and its child are not a saga
// 14b  compensate a failed reserve after a charge
// 14c  who may watch is the database check, not the object store alone
```

- example 15. saga versus transaction for b_3
- the row and its first child are one Postgres transaction, not a saga
- a saga is for Stripe plus our row, with a compensating step
- the object key holds bytes. Postgres holds owner and key b_3
- compensation is idempotent. a retry does not refund twice

```text
pending row b_3
charge
mark paid
if the mark never happens, a worker asks what the charge did
object key stores the file. the row stores the key
```

- homework
```text
// 15a  why b_3 and its child are not a saga
// 15b  compensate a failed reserve after a charge
// 15c  who may watch is the database check, not the object store alone
```

- example 16. saga versus transaction for se_1
- the row and its first child are one Postgres transaction, not a saga
- a saga is for Stripe plus our row, with a compensating step
- the object key holds bytes. Postgres holds owner and key se_1
- compensation is idempotent. a retry does not refund twice

```text
pending row se_1
charge
mark paid
if the mark never happens, a worker asks what the charge did
object key stores the file. the row stores the key
```

- homework
```text
// 16a  why se_1 and its child are not a saga
// 16b  compensate a failed reserve after a charge
// 16c  who may watch is the database check, not the object store alone
```

- example 17. saga versus transaction for q_1
- the row and its first child are one Postgres transaction, not a saga
- a saga is for Stripe plus our row, with a compensating step
- the object key holds bytes. Postgres holds owner and key q_1
- compensation is idempotent. a retry does not refund twice

```text
pending row q_1
charge
mark paid
if the mark never happens, a worker asks what the charge did
object key stores the file. the row stores the key
```

- homework
```text
// 17a  why q_1 and its child are not a saga
// 17b  compensate a failed reserve after a charge
// 17c  who may watch is the database check, not the object store alone
```

- example 18. saga versus transaction for r_1
- the row and its first child are one Postgres transaction, not a saga
- a saga is for Stripe plus our row, with a compensating step
- the object key holds bytes. Postgres holds owner and key r_1
- compensation is idempotent. a retry does not refund twice

```text
pending row r_1
charge
mark paid
if the mark never happens, a worker asks what the charge did
object key stores the file. the row stores the key
```

- homework
```text
// 18a  why r_1 and its child are not a saga
// 18b  compensate a failed reserve after a charge
// 18c  who may watch is the database check, not the object store alone
```

- example 19. saga versus transaction for pc_12
- the row and its first child are one Postgres transaction, not a saga
- a saga is for Stripe plus our row, with a compensating step
- the object key holds bytes. Postgres holds owner and key pc_12
- compensation is idempotent. a retry does not refund twice

```text
pending row pc_12
charge
mark paid
if the mark never happens, a worker asks what the charge did
object key stores the file. the row stores the key
```

- homework
```text
// 19a  why pc_12 and its child are not a saga
// 19b  compensate a failed reserve after a charge
// 19c  who may watch is the database check, not the object store alone
```

- example 20. saga versus transaction for m_1
- the row and its first child are one Postgres transaction, not a saga
- a saga is for Stripe plus our row, with a compensating step
- the object key holds bytes. Postgres holds owner and key m_1
- compensation is idempotent. a retry does not refund twice

```text
pending row m_1
charge
mark paid
if the mark never happens, a worker asks what the charge did
object key stores the file. the row stores the key
```

- homework
```text
// 20a  why m_1 and its child are not a saga
// 20b  compensate a failed reserve after a charge
// 20c  who may watch is the database check, not the object store alone
```

- example 21. saga versus transaction for t_9
- the row and its first child are one Postgres transaction, not a saga
- a saga is for Stripe plus our row, with a compensating step
- the object key holds bytes. Postgres holds owner and key t_9
- compensation is idempotent. a retry does not refund twice

```text
pending row t_9
charge
mark paid
if the mark never happens, a worker asks what the charge did
object key stores the file. the row stores the key
```

- homework
```text
// 21a  why t_9 and its child are not a saga
// 21b  compensate a failed reserve after a charge
// 21c  who may watch is the database check, not the object store alone
```

- example 22. saga versus transaction for v_4
- the row and its first child are one Postgres transaction, not a saga
- a saga is for Stripe plus our row, with a compensating step
- the object key holds bytes. Postgres holds owner and key v_4
- compensation is idempotent. a retry does not refund twice

```text
pending row v_4
charge
mark paid
if the mark never happens, a worker asks what the charge did
object key stores the file. the row stores the key
```

- homework
```text
// 22a  why v_4 and its child are not a saga
// 22b  compensate a failed reserve after a charge
// 22c  who may watch is the database check, not the object store alone
```

- example 23. saga versus transaction for f_2
- the row and its first child are one Postgres transaction, not a saga
- a saga is for Stripe plus our row, with a compensating step
- the object key holds bytes. Postgres holds owner and key f_2
- compensation is idempotent. a retry does not refund twice

```text
pending row f_2
charge
mark paid
if the mark never happens, a worker asks what the charge did
object key stores the file. the row stores the key
```

- homework
```text
// 23a  why f_2 and its child are not a saga
// 23b  compensate a failed reserve after a charge
// 23c  who may watch is the database check, not the object store alone
```

- example 24. saga versus transaction for u_3
- the row and its first child are one Postgres transaction, not a saga
- a saga is for Stripe plus our row, with a compensating step
- the object key holds bytes. Postgres holds owner and key u_3
- compensation is idempotent. a retry does not refund twice

```text
pending row u_3
charge
mark paid
if the mark never happens, a worker asks what the charge did
object key stores the file. the row stores the key
```

- homework
```text
// 24a  why u_3 and its child are not a saga
// 24b  compensate a failed reserve after a charge
// 24c  who may watch is the database check, not the object store alone
```

- example 25. saga versus transaction for pay_8
- the row and its first child are one Postgres transaction, not a saga
- a saga is for Stripe plus our row, with a compensating step
- the object key holds bytes. Postgres holds owner and key pay_8
- compensation is idempotent. a retry does not refund twice

```text
pending row pay_8
charge
mark paid
if the mark never happens, a worker asks what the charge did
object key stores the file. the row stores the key
```

- homework
```text
// 25a  why pay_8 and its child are not a saga
// 25b  compensate a failed reserve after a charge
// 25c  who may watch is the database check, not the object store alone
```

- example 26. saga versus transaction for room_2
- the row and its first child are one Postgres transaction, not a saga
- a saga is for Stripe plus our row, with a compensating step
- the object key holds bytes. Postgres holds owner and key room_2
- compensation is idempotent. a retry does not refund twice

```text
pending row room_2
charge
mark paid
if the mark never happens, a worker asks what the charge did
object key stores the file. the row stores the key
```

- homework
```text
// 26a  why room_2 and its child are not a saga
// 26b  compensate a failed reserve after a charge
// 26c  who may watch is the database check, not the object store alone
```

- example 27. saga versus transaction for vs_7
- the row and its first child are one Postgres transaction, not a saga
- a saga is for Stripe plus our row, with a compensating step
- the object key holds bytes. Postgres holds owner and key vs_7
- compensation is idempotent. a retry does not refund twice

```text
pending row vs_7
charge
mark paid
if the mark never happens, a worker asks what the charge did
object key stores the file. the row stores the key
```

- homework
```text
// 27a  why vs_7 and its child are not a saga
// 27b  compensate a failed reserve after a charge
// 27c  who may watch is the database check, not the object store alone
```

- example 28. saga versus transaction for sc_3
- the row and its first child are one Postgres transaction, not a saga
- a saga is for Stripe plus our row, with a compensating step
- the object key holds bytes. Postgres holds owner and key sc_3
- compensation is idempotent. a retry does not refund twice

```text
pending row sc_3
charge
mark paid
if the mark never happens, a worker asks what the charge did
object key stores the file. the row stores the key
```

- homework
```text
// 28a  why sc_3 and its child are not a saga
// 28b  compensate a failed reserve after a charge
// 28c  who may watch is the database check, not the object store alone
```

- example 29. saga versus transaction for tm_2
- the row and its first child are one Postgres transaction, not a saga
- a saga is for Stripe plus our row, with a compensating step
- the object key holds bytes. Postgres holds owner and key tm_2
- compensation is idempotent. a retry does not refund twice

```text
pending row tm_2
charge
mark paid
if the mark never happens, a worker asks what the charge did
object key stores the file. the row stores the key
```

- homework
```text
// 29a  why tm_2 and its child are not a saga
// 29b  compensate a failed reserve after a charge
// 29c  who may watch is the database check, not the object store alone
```

- example 30. saga versus transaction for up_6
- the row and its first child are one Postgres transaction, not a saga
- a saga is for Stripe plus our row, with a compensating step
- the object key holds bytes. Postgres holds owner and key up_6
- compensation is idempotent. a retry does not refund twice

```text
pending row up_6
charge
mark paid
if the mark never happens, a worker asks what the charge did
object key stores the file. the row stores the key
```

- homework
```text
// 30a  why up_6 and its child are not a saga
// 30b  compensate a failed reserve after a charge
// 30c  who may watch is the database check, not the object store alone
```

- example 31. saga versus transaction for st_1
- the row and its first child are one Postgres transaction, not a saga
- a saga is for Stripe plus our row, with a compensating step
- the object key holds bytes. Postgres holds owner and key st_1
- compensation is idempotent. a retry does not refund twice

```text
pending row st_1
charge
mark paid
if the mark never happens, a worker asks what the charge did
object key stores the file. the row stores the key
```

- homework
```text
// 31a  why st_1 and its child are not a saga
// 31b  compensate a failed reserve after a charge
// 31c  who may watch is the database check, not the object store alone
```

- example 32. saga versus transaction for n_2
- the row and its first child are one Postgres transaction, not a saga
- a saga is for Stripe plus our row, with a compensating step
- the object key holds bytes. Postgres holds owner and key n_2
- compensation is idempotent. a retry does not refund twice

```text
pending row n_2
charge
mark paid
if the mark never happens, a worker asks what the charge did
object key stores the file. the row stores the key
```

- homework
```text
// 32a  why n_2 and its child are not a saga
// 32b  compensate a failed reserve after a charge
// 32c  who may watch is the database check, not the object store alone
```

- example 33. saga versus transaction for b_3
- the row and its first child are one Postgres transaction, not a saga
- a saga is for Stripe plus our row, with a compensating step
- the object key holds bytes. Postgres holds owner and key b_3
- compensation is idempotent. a retry does not refund twice

```text
pending row b_3
charge
mark paid
if the mark never happens, a worker asks what the charge did
object key stores the file. the row stores the key
```

- homework
```text
// 33a  why b_3 and its child are not a saga
// 33b  compensate a failed reserve after a charge
// 33c  who may watch is the database check, not the object store alone
```

- example 34. saga versus transaction for se_1
- the row and its first child are one Postgres transaction, not a saga
- a saga is for Stripe plus our row, with a compensating step
- the object key holds bytes. Postgres holds owner and key se_1
- compensation is idempotent. a retry does not refund twice

```text
pending row se_1
charge
mark paid
if the mark never happens, a worker asks what the charge did
object key stores the file. the row stores the key
```

- homework
```text
// 34a  why se_1 and its child are not a saga
// 34b  compensate a failed reserve after a charge
// 34c  who may watch is the database check, not the object store alone
```

- example 35. saga versus transaction for q_1
- the row and its first child are one Postgres transaction, not a saga
- a saga is for Stripe plus our row, with a compensating step
- the object key holds bytes. Postgres holds owner and key q_1
- compensation is idempotent. a retry does not refund twice

```text
pending row q_1
charge
mark paid
if the mark never happens, a worker asks what the charge did
object key stores the file. the row stores the key
```

- homework
```text
// 35a  why q_1 and its child are not a saga
// 35b  compensate a failed reserve after a charge
// 35c  who may watch is the database check, not the object store alone
```

- example 36. saga versus transaction for r_1
- the row and its first child are one Postgres transaction, not a saga
- a saga is for Stripe plus our row, with a compensating step
- the object key holds bytes. Postgres holds owner and key r_1
- compensation is idempotent. a retry does not refund twice

```text
pending row r_1
charge
mark paid
if the mark never happens, a worker asks what the charge did
object key stores the file. the row stores the key
```

- homework
```text
// 36a  why r_1 and its child are not a saga
// 36b  compensate a failed reserve after a charge
// 36c  who may watch is the database check, not the object store alone
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

- example 1. rough size around 72 requests a second
- a day is about 100,000 seconds
- peak is often a small multiple of the average
- 1 KB is 1000 bytes for this estimate
- we want the order of magnitude, then we stop

```text
average 72 rps
peak 288 rps
out ~ 288 * 8 KB
logs/day ~ 72 * 100000 * 200 bytes
id in the story: pc_12
```

- homework
```text
// 1a  peak at 4 times 72
// 1b  1000 files of 100 MB is about 100 GB
// 1c  0.2 jobs a second does not need a second worker yet
```

- example 2. rough size around 3 requests a second
- a day is about 100,000 seconds
- peak is often a small multiple of the average
- 1 KB is 1000 bytes for this estimate
- we want the order of magnitude, then we stop

```text
average 3 rps
peak 12 rps
out ~ 12 * 8 KB
logs/day ~ 3 * 100000 * 200 bytes
id in the story: m_1
```

- homework
```text
// 2a  peak at 4 times 3
// 2b  1000 files of 100 MB is about 100 GB
// 2c  0.2 jobs a second does not need a second worker yet
```

- example 3. rough size around 16 requests a second
- a day is about 100,000 seconds
- peak is often a small multiple of the average
- 1 KB is 1000 bytes for this estimate
- we want the order of magnitude, then we stop

```text
average 16 rps
peak 64 rps
out ~ 64 * 8 KB
logs/day ~ 16 * 100000 * 200 bytes
id in the story: t_9
```

- homework
```text
// 3a  peak at 4 times 16
// 3b  1000 files of 100 MB is about 100 GB
// 3c  0.2 jobs a second does not need a second worker yet
```

- example 4. rough size around 6 requests a second
- a day is about 100,000 seconds
- peak is often a small multiple of the average
- 1 KB is 1000 bytes for this estimate
- we want the order of magnitude, then we stop

```text
average 6 rps
peak 24 rps
out ~ 24 * 8 KB
logs/day ~ 6 * 100000 * 200 bytes
id in the story: v_4
```

- homework
```text
// 4a  peak at 4 times 6
// 4b  1000 files of 100 MB is about 100 GB
// 4c  0.2 jobs a second does not need a second worker yet
```

- example 5. rough size around 1 requests a second
- a day is about 100,000 seconds
- peak is often a small multiple of the average
- 1 KB is 1000 bytes for this estimate
- we want the order of magnitude, then we stop

```text
average 1 rps
peak 4 rps
out ~ 4 * 8 KB
logs/day ~ 1 * 100000 * 200 bytes
id in the story: f_2
```

- homework
```text
// 5a  peak at 4 times 1
// 5b  1000 files of 100 MB is about 100 GB
// 5c  0.2 jobs a second does not need a second worker yet
```

- example 6. rough size around 1 requests a second
- a day is about 100,000 seconds
- peak is often a small multiple of the average
- 1 KB is 1000 bytes for this estimate
- we want the order of magnitude, then we stop

```text
average 1 rps
peak 4 rps
out ~ 4 * 8 KB
logs/day ~ 1 * 100000 * 200 bytes
id in the story: u_3
```

- homework
```text
// 6a  peak at 4 times 1
// 6b  1000 files of 100 MB is about 100 GB
// 6c  0.2 jobs a second does not need a second worker yet
```

- example 7. rough size around 15000 requests a second
- a day is about 100,000 seconds
- peak is often a small multiple of the average
- 1 KB is 1000 bytes for this estimate
- we want the order of magnitude, then we stop

```text
average 15000 rps
peak 60000 rps
out ~ 60000 * 8 KB
logs/day ~ 15000 * 100000 * 200 bytes
id in the story: pay_8
```

- homework
```text
// 7a  peak at 4 times 15000
// 7b  1000 files of 100 MB is about 100 GB
// 7c  0.2 jobs a second does not need a second worker yet
```

- example 8. rough size around 12 requests a second
- a day is about 100,000 seconds
- peak is often a small multiple of the average
- 1 KB is 1000 bytes for this estimate
- we want the order of magnitude, then we stop

```text
average 12 rps
peak 48 rps
out ~ 48 * 8 KB
logs/day ~ 12 * 100000 * 200 bytes
id in the story: room_2
```

- homework
```text
// 8a  peak at 4 times 12
// 8b  1000 files of 100 MB is about 100 GB
// 8c  0.2 jobs a second does not need a second worker yet
```

- example 9. rough size around 48 requests a second
- a day is about 100,000 seconds
- peak is often a small multiple of the average
- 1 KB is 1000 bytes for this estimate
- we want the order of magnitude, then we stop

```text
average 48 rps
peak 192 rps
out ~ 192 * 8 KB
logs/day ~ 48 * 100000 * 200 bytes
id in the story: vs_7
```

- homework
```text
// 9a  peak at 4 times 48
// 9b  1000 files of 100 MB is about 100 GB
// 9c  0.2 jobs a second does not need a second worker yet
```

- example 10. rough size around 11 requests a second
- a day is about 100,000 seconds
- peak is often a small multiple of the average
- 1 KB is 1000 bytes for this estimate
- we want the order of magnitude, then we stop

```text
average 11 rps
peak 44 rps
out ~ 44 * 8 KB
logs/day ~ 11 * 100000 * 200 bytes
id in the story: sc_3
```

- homework
```text
// 10a  peak at 4 times 11
// 10b  1000 files of 100 MB is about 100 GB
// 10c  0.2 jobs a second does not need a second worker yet
```

- example 11. rough size around 2 requests a second
- a day is about 100,000 seconds
- peak is often a small multiple of the average
- 1 KB is 1000 bytes for this estimate
- we want the order of magnitude, then we stop

```text
average 2 rps
peak 8 rps
out ~ 8 * 8 KB
logs/day ~ 2 * 100000 * 200 bytes
id in the story: tm_2
```

- homework
```text
// 11a  peak at 4 times 2
// 11b  1000 files of 100 MB is about 100 GB
// 11c  0.2 jobs a second does not need a second worker yet
```

- example 12. rough size around 200 requests a second
- a day is about 100,000 seconds
- peak is often a small multiple of the average
- 1 KB is 1000 bytes for this estimate
- we want the order of magnitude, then we stop

```text
average 200 rps
peak 800 rps
out ~ 800 * 8 KB
logs/day ~ 200 * 100000 * 200 bytes
id in the story: up_6
```

- homework
```text
// 12a  peak at 4 times 200
// 12b  1000 files of 100 MB is about 100 GB
// 12c  0.2 jobs a second does not need a second worker yet
```

- example 13. rough size around 1500 requests a second
- a day is about 100,000 seconds
- peak is often a small multiple of the average
- 1 KB is 1000 bytes for this estimate
- we want the order of magnitude, then we stop

```text
average 1500 rps
peak 6000 rps
out ~ 6000 * 8 KB
logs/day ~ 1500 * 100000 * 200 bytes
id in the story: st_1
```

- homework
```text
// 13a  peak at 4 times 1500
// 13b  1000 files of 100 MB is about 100 GB
// 13c  0.2 jobs a second does not need a second worker yet
```

- example 14. rough size around 24 requests a second
- a day is about 100,000 seconds
- peak is often a small multiple of the average
- 1 KB is 1000 bytes for this estimate
- we want the order of magnitude, then we stop

```text
average 24 rps
peak 96 rps
out ~ 96 * 8 KB
logs/day ~ 24 * 100000 * 200 bytes
id in the story: n_2
```

- homework
```text
// 14a  peak at 4 times 24
// 14b  1000 files of 100 MB is about 100 GB
// 14c  0.2 jobs a second does not need a second worker yet
```

- example 15. rough size around 4 requests a second
- a day is about 100,000 seconds
- peak is often a small multiple of the average
- 1 KB is 1000 bytes for this estimate
- we want the order of magnitude, then we stop

```text
average 4 rps
peak 16 rps
out ~ 16 * 8 KB
logs/day ~ 4 * 100000 * 200 bytes
id in the story: b_3
```

- homework
```text
// 15a  peak at 4 times 4
// 15b  1000 files of 100 MB is about 100 GB
// 15c  0.2 jobs a second does not need a second worker yet
```

- example 16. rough size around 15 requests a second
- a day is about 100,000 seconds
- peak is often a small multiple of the average
- 1 KB is 1000 bytes for this estimate
- we want the order of magnitude, then we stop

```text
average 15 rps
peak 60 rps
out ~ 60 * 8 KB
logs/day ~ 15 * 100000 * 200 bytes
id in the story: se_1
```

- homework
```text
// 16a  peak at 4 times 15
// 16b  1000 files of 100 MB is about 100 GB
// 16c  0.2 jobs a second does not need a second worker yet
```

- example 17. rough size around 10 requests a second
- a day is about 100,000 seconds
- peak is often a small multiple of the average
- 1 KB is 1000 bytes for this estimate
- we want the order of magnitude, then we stop

```text
average 10 rps
peak 40 rps
out ~ 40 * 8 KB
logs/day ~ 10 * 100000 * 200 bytes
id in the story: q_1
```

- homework
```text
// 17a  peak at 4 times 10
// 17b  1000 files of 100 MB is about 100 GB
// 17c  0.2 jobs a second does not need a second worker yet
```

- example 18. rough size around 2 requests a second
- a day is about 100,000 seconds
- peak is often a small multiple of the average
- 1 KB is 1000 bytes for this estimate
- we want the order of magnitude, then we stop

```text
average 2 rps
peak 8 rps
out ~ 8 * 8 KB
logs/day ~ 2 * 100000 * 200 bytes
id in the story: r_1
```

- homework
```text
// 18a  peak at 4 times 2
// 18b  1000 files of 100 MB is about 100 GB
// 18c  0.2 jobs a second does not need a second worker yet
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

- example 1. the boxes, with pc_12 as the row
- nginx, Nest, Postgres, Redis, worker, Socket.IO
- job id laytime:pc_12
- figure 7500.0 is computed in the worker from the pure function
- Anemi crosses out Redis and the worker

```text
clients -> nginx -> Nest -> Postgres row pc_12
                 Nest -> Redis job laytime:pc_12
                 worker -> Socket.IO
```

- homework
```text
// 1a  redraw with pc_12
// 1b  cross out Redis for Anemi
// 1c  the number 7500.0 comes from the function, not from the gateway
```

- example 2. the boxes, with m_1 as the row
- nginx, Nest, Postgres, Redis, worker, Socket.IO
- job id laytime:m_1
- figure 0 is computed in the worker from the pure function
- Anemi crosses out Redis and the worker

```text
clients -> nginx -> Nest -> Postgres row m_1
                 Nest -> Redis job laytime:m_1
                 worker -> Socket.IO
```

- homework
```text
// 2a  redraw with m_1
// 2b  cross out Redis for Anemi
// 2c  the number 0 comes from the function, not from the gateway
```

- example 3. the boxes, with t_9 as the row
- nginx, Nest, Postgres, Redis, worker, Socket.IO
- job id laytime:t_9
- figure 7500.0 is computed in the worker from the pure function
- Anemi crosses out Redis and the worker

```text
clients -> nginx -> Nest -> Postgres row t_9
                 Nest -> Redis job laytime:t_9
                 worker -> Socket.IO
```

- homework
```text
// 3a  redraw with t_9
// 3b  cross out Redis for Anemi
// 3c  the number 7500.0 comes from the function, not from the gateway
```

- example 4. the boxes, with v_4 as the row
- nginx, Nest, Postgres, Redis, worker, Socket.IO
- job id laytime:v_4
- figure 0 is computed in the worker from the pure function
- Anemi crosses out Redis and the worker

```text
clients -> nginx -> Nest -> Postgres row v_4
                 Nest -> Redis job laytime:v_4
                 worker -> Socket.IO
```

- homework
```text
// 4a  redraw with v_4
// 4b  cross out Redis for Anemi
// 4c  the number 0 comes from the function, not from the gateway
```

- example 5. the boxes, with f_2 as the row
- nginx, Nest, Postgres, Redis, worker, Socket.IO
- job id laytime:f_2
- figure 0 is computed in the worker from the pure function
- Anemi crosses out Redis and the worker

```text
clients -> nginx -> Nest -> Postgres row f_2
                 Nest -> Redis job laytime:f_2
                 worker -> Socket.IO
```

- homework
```text
// 5a  redraw with f_2
// 5b  cross out Redis for Anemi
// 5c  the number 0 comes from the function, not from the gateway
```

- example 6. the boxes, with u_3 as the row
- nginx, Nest, Postgres, Redis, worker, Socket.IO
- job id laytime:u_3
- figure 0 is computed in the worker from the pure function
- Anemi crosses out Redis and the worker

```text
clients -> nginx -> Nest -> Postgres row u_3
                 Nest -> Redis job laytime:u_3
                 worker -> Socket.IO
```

- homework
```text
// 6a  redraw with u_3
// 6b  cross out Redis for Anemi
// 6c  the number 0 comes from the function, not from the gateway
```

- example 7. the boxes, with pay_8 as the row
- nginx, Nest, Postgres, Redis, worker, Socket.IO
- job id laytime:pay_8
- figure 7500.0 is computed in the worker from the pure function
- Anemi crosses out Redis and the worker

```text
clients -> nginx -> Nest -> Postgres row pay_8
                 Nest -> Redis job laytime:pay_8
                 worker -> Socket.IO
```

- homework
```text
// 7a  redraw with pay_8
// 7b  cross out Redis for Anemi
// 7c  the number 7500.0 comes from the function, not from the gateway
```

- example 8. the boxes, with room_2 as the row
- nginx, Nest, Postgres, Redis, worker, Socket.IO
- job id laytime:room_2
- figure 17500.0 is computed in the worker from the pure function
- Anemi crosses out Redis and the worker

```text
clients -> nginx -> Nest -> Postgres row room_2
                 Nest -> Redis job laytime:room_2
                 worker -> Socket.IO
```

- homework
```text
// 8a  redraw with room_2
// 8b  cross out Redis for Anemi
// 8c  the number 17500.0 comes from the function, not from the gateway
```

- example 9. the boxes, with vs_7 as the row
- nginx, Nest, Postgres, Redis, worker, Socket.IO
- job id laytime:vs_7
- figure 4000.0 is computed in the worker from the pure function
- Anemi crosses out Redis and the worker

```text
clients -> nginx -> Nest -> Postgres row vs_7
                 Nest -> Redis job laytime:vs_7
                 worker -> Socket.IO
```

- homework
```text
// 9a  redraw with vs_7
// 9b  cross out Redis for Anemi
// 9c  the number 4000.0 comes from the function, not from the gateway
```

- example 10. the boxes, with sc_3 as the row
- nginx, Nest, Postgres, Redis, worker, Socket.IO
- job id laytime:sc_3
- figure 7500.0 is computed in the worker from the pure function
- Anemi crosses out Redis and the worker

```text
clients -> nginx -> Nest -> Postgres row sc_3
                 Nest -> Redis job laytime:sc_3
                 worker -> Socket.IO
```

- homework
```text
// 10a  redraw with sc_3
// 10b  cross out Redis for Anemi
// 10c  the number 7500.0 comes from the function, not from the gateway
```

- example 11. the boxes, with tm_2 as the row
- nginx, Nest, Postgres, Redis, worker, Socket.IO
- job id laytime:tm_2
- figure 0 is computed in the worker from the pure function
- Anemi crosses out Redis and the worker

```text
clients -> nginx -> Nest -> Postgres row tm_2
                 Nest -> Redis job laytime:tm_2
                 worker -> Socket.IO
```

- homework
```text
// 11a  redraw with tm_2
// 11b  cross out Redis for Anemi
// 11c  the number 0 comes from the function, not from the gateway
```

- example 12. the boxes, with up_6 as the row
- nginx, Nest, Postgres, Redis, worker, Socket.IO
- job id laytime:up_6
- figure 7500.0 is computed in the worker from the pure function
- Anemi crosses out Redis and the worker

```text
clients -> nginx -> Nest -> Postgres row up_6
                 Nest -> Redis job laytime:up_6
                 worker -> Socket.IO
```

- homework
```text
// 12a  redraw with up_6
// 12b  cross out Redis for Anemi
// 12c  the number 7500.0 comes from the function, not from the gateway
```

- example 13. the boxes, with st_1 as the row
- nginx, Nest, Postgres, Redis, worker, Socket.IO
- job id laytime:st_1
- figure 125000.0 is computed in the worker from the pure function
- Anemi crosses out Redis and the worker

```text
clients -> nginx -> Nest -> Postgres row st_1
                 Nest -> Redis job laytime:st_1
                 worker -> Socket.IO
```

- homework
```text
// 13a  redraw with st_1
// 13b  cross out Redis for Anemi
// 13c  the number 125000.0 comes from the function, not from the gateway
```

- example 14. the boxes, with n_2 as the row
- nginx, Nest, Postgres, Redis, worker, Socket.IO
- job id laytime:n_2
- figure 3000.0 is computed in the worker from the pure function
- Anemi crosses out Redis and the worker

```text
clients -> nginx -> Nest -> Postgres row n_2
                 Nest -> Redis job laytime:n_2
                 worker -> Socket.IO
```

- homework
```text
// 14a  redraw with n_2
// 14b  cross out Redis for Anemi
// 14c  the number 3000.0 comes from the function, not from the gateway
```

- example 15. the boxes, with b_3 as the row
- nginx, Nest, Postgres, Redis, worker, Socket.IO
- job id laytime:b_3
- figure 0 is computed in the worker from the pure function
- Anemi crosses out Redis and the worker

```text
clients -> nginx -> Nest -> Postgres row b_3
                 Nest -> Redis job laytime:b_3
                 worker -> Socket.IO
```

- homework
```text
// 15a  redraw with b_3
// 15b  cross out Redis for Anemi
// 15c  the number 0 comes from the function, not from the gateway
```

- example 16. the boxes, with se_1 as the row
- nginx, Nest, Postgres, Redis, worker, Socket.IO
- job id laytime:se_1
- figure 28125.0 is computed in the worker from the pure function
- Anemi crosses out Redis and the worker

```text
clients -> nginx -> Nest -> Postgres row se_1
                 Nest -> Redis job laytime:se_1
                 worker -> Socket.IO
```

- homework
```text
// 16a  redraw with se_1
// 16b  cross out Redis for Anemi
// 16c  the number 28125.0 comes from the function, not from the gateway
```

- example 17. the boxes, with q_1 as the row
- nginx, Nest, Postgres, Redis, worker, Socket.IO
- job id laytime:q_1
- figure 0 is computed in the worker from the pure function
- Anemi crosses out Redis and the worker

```text
clients -> nginx -> Nest -> Postgres row q_1
                 Nest -> Redis job laytime:q_1
                 worker -> Socket.IO
```

- homework
```text
// 17a  redraw with q_1
// 17b  cross out Redis for Anemi
// 17c  the number 0 comes from the function, not from the gateway
```

- example 18. the boxes, with r_1 as the row
- nginx, Nest, Postgres, Redis, worker, Socket.IO
- job id laytime:r_1
- figure 0 is computed in the worker from the pure function
- Anemi crosses out Redis and the worker

```text
clients -> nginx -> Nest -> Postgres row r_1
                 Nest -> Redis job laytime:r_1
                 worker -> Socket.IO
```

- homework
```text
// 18a  redraw with r_1
// 18b  cross out Redis for Anemi
// 18c  the number 0 comes from the function, not from the gateway
```

- example 19. the boxes, with pc_12 as the row
- nginx, Nest, Postgres, Redis, worker, Socket.IO
- job id laytime:pc_12
- figure 7500.0 is computed in the worker from the pure function
- Anemi crosses out Redis and the worker

```text
clients -> nginx -> Nest -> Postgres row pc_12
                 Nest -> Redis job laytime:pc_12
                 worker -> Socket.IO
```

- homework
```text
// 19a  redraw with pc_12
// 19b  cross out Redis for Anemi
// 19c  the number 7500.0 comes from the function, not from the gateway
```

- example 20. the boxes, with m_1 as the row
- nginx, Nest, Postgres, Redis, worker, Socket.IO
- job id laytime:m_1
- figure 0 is computed in the worker from the pure function
- Anemi crosses out Redis and the worker

```text
clients -> nginx -> Nest -> Postgres row m_1
                 Nest -> Redis job laytime:m_1
                 worker -> Socket.IO
```

- homework
```text
// 20a  redraw with m_1
// 20b  cross out Redis for Anemi
// 20c  the number 0 comes from the function, not from the gateway
```

- example 21. the boxes, with t_9 as the row
- nginx, Nest, Postgres, Redis, worker, Socket.IO
- job id laytime:t_9
- figure 7500.0 is computed in the worker from the pure function
- Anemi crosses out Redis and the worker

```text
clients -> nginx -> Nest -> Postgres row t_9
                 Nest -> Redis job laytime:t_9
                 worker -> Socket.IO
```

- homework
```text
// 21a  redraw with t_9
// 21b  cross out Redis for Anemi
// 21c  the number 7500.0 comes from the function, not from the gateway
```

- example 22. the boxes, with v_4 as the row
- nginx, Nest, Postgres, Redis, worker, Socket.IO
- job id laytime:v_4
- figure 0 is computed in the worker from the pure function
- Anemi crosses out Redis and the worker

```text
clients -> nginx -> Nest -> Postgres row v_4
                 Nest -> Redis job laytime:v_4
                 worker -> Socket.IO
```

- homework
```text
// 22a  redraw with v_4
// 22b  cross out Redis for Anemi
// 22c  the number 0 comes from the function, not from the gateway
```

- example 23. the boxes, with f_2 as the row
- nginx, Nest, Postgres, Redis, worker, Socket.IO
- job id laytime:f_2
- figure 0 is computed in the worker from the pure function
- Anemi crosses out Redis and the worker

```text
clients -> nginx -> Nest -> Postgres row f_2
                 Nest -> Redis job laytime:f_2
                 worker -> Socket.IO
```

- homework
```text
// 23a  redraw with f_2
// 23b  cross out Redis for Anemi
// 23c  the number 0 comes from the function, not from the gateway
```

- example 24. the boxes, with u_3 as the row
- nginx, Nest, Postgres, Redis, worker, Socket.IO
- job id laytime:u_3
- figure 0 is computed in the worker from the pure function
- Anemi crosses out Redis and the worker

```text
clients -> nginx -> Nest -> Postgres row u_3
                 Nest -> Redis job laytime:u_3
                 worker -> Socket.IO
```

- homework
```text
// 24a  redraw with u_3
// 24b  cross out Redis for Anemi
// 24c  the number 0 comes from the function, not from the gateway
```

- example 25. the boxes, with pay_8 as the row
- nginx, Nest, Postgres, Redis, worker, Socket.IO
- job id laytime:pay_8
- figure 7500.0 is computed in the worker from the pure function
- Anemi crosses out Redis and the worker

```text
clients -> nginx -> Nest -> Postgres row pay_8
                 Nest -> Redis job laytime:pay_8
                 worker -> Socket.IO
```

- homework
```text
// 25a  redraw with pay_8
// 25b  cross out Redis for Anemi
// 25c  the number 7500.0 comes from the function, not from the gateway
```

- example 26. the boxes, with room_2 as the row
- nginx, Nest, Postgres, Redis, worker, Socket.IO
- job id laytime:room_2
- figure 17500.0 is computed in the worker from the pure function
- Anemi crosses out Redis and the worker

```text
clients -> nginx -> Nest -> Postgres row room_2
                 Nest -> Redis job laytime:room_2
                 worker -> Socket.IO
```

- homework
```text
// 26a  redraw with room_2
// 26b  cross out Redis for Anemi
// 26c  the number 17500.0 comes from the function, not from the gateway
```

- example 27. the boxes, with vs_7 as the row
- nginx, Nest, Postgres, Redis, worker, Socket.IO
- job id laytime:vs_7
- figure 4000.0 is computed in the worker from the pure function
- Anemi crosses out Redis and the worker

```text
clients -> nginx -> Nest -> Postgres row vs_7
                 Nest -> Redis job laytime:vs_7
                 worker -> Socket.IO
```

- homework
```text
// 27a  redraw with vs_7
// 27b  cross out Redis for Anemi
// 27c  the number 4000.0 comes from the function, not from the gateway
```

- example 28. the boxes, with sc_3 as the row
- nginx, Nest, Postgres, Redis, worker, Socket.IO
- job id laytime:sc_3
- figure 7500.0 is computed in the worker from the pure function
- Anemi crosses out Redis and the worker

```text
clients -> nginx -> Nest -> Postgres row sc_3
                 Nest -> Redis job laytime:sc_3
                 worker -> Socket.IO
```

- homework
```text
// 28a  redraw with sc_3
// 28b  cross out Redis for Anemi
// 28c  the number 7500.0 comes from the function, not from the gateway
```

- example 29. the boxes, with tm_2 as the row
- nginx, Nest, Postgres, Redis, worker, Socket.IO
- job id laytime:tm_2
- figure 0 is computed in the worker from the pure function
- Anemi crosses out Redis and the worker

```text
clients -> nginx -> Nest -> Postgres row tm_2
                 Nest -> Redis job laytime:tm_2
                 worker -> Socket.IO
```

- homework
```text
// 29a  redraw with tm_2
// 29b  cross out Redis for Anemi
// 29c  the number 0 comes from the function, not from the gateway
```

- example 30. the boxes, with up_6 as the row
- nginx, Nest, Postgres, Redis, worker, Socket.IO
- job id laytime:up_6
- figure 7500.0 is computed in the worker from the pure function
- Anemi crosses out Redis and the worker

```text
clients -> nginx -> Nest -> Postgres row up_6
                 Nest -> Redis job laytime:up_6
                 worker -> Socket.IO
```

- homework
```text
// 30a  redraw with up_6
// 30b  cross out Redis for Anemi
// 30c  the number 7500.0 comes from the function, not from the gateway
```

- example 31. the boxes, with st_1 as the row
- nginx, Nest, Postgres, Redis, worker, Socket.IO
- job id laytime:st_1
- figure 125000.0 is computed in the worker from the pure function
- Anemi crosses out Redis and the worker

```text
clients -> nginx -> Nest -> Postgres row st_1
                 Nest -> Redis job laytime:st_1
                 worker -> Socket.IO
```

- homework
```text
// 31a  redraw with st_1
// 31b  cross out Redis for Anemi
// 31c  the number 125000.0 comes from the function, not from the gateway
```

- example 32. the boxes, with n_2 as the row
- nginx, Nest, Postgres, Redis, worker, Socket.IO
- job id laytime:n_2
- figure 3000.0 is computed in the worker from the pure function
- Anemi crosses out Redis and the worker

```text
clients -> nginx -> Nest -> Postgres row n_2
                 Nest -> Redis job laytime:n_2
                 worker -> Socket.IO
```

- homework
```text
// 32a  redraw with n_2
// 32b  cross out Redis for Anemi
// 32c  the number 3000.0 comes from the function, not from the gateway
```

- example 33. the boxes, with b_3 as the row
- nginx, Nest, Postgres, Redis, worker, Socket.IO
- job id laytime:b_3
- figure 0 is computed in the worker from the pure function
- Anemi crosses out Redis and the worker

```text
clients -> nginx -> Nest -> Postgres row b_3
                 Nest -> Redis job laytime:b_3
                 worker -> Socket.IO
```

- homework
```text
// 33a  redraw with b_3
// 33b  cross out Redis for Anemi
// 33c  the number 0 comes from the function, not from the gateway
```

- example 34. the boxes, with se_1 as the row
- nginx, Nest, Postgres, Redis, worker, Socket.IO
- job id laytime:se_1
- figure 28125.0 is computed in the worker from the pure function
- Anemi crosses out Redis and the worker

```text
clients -> nginx -> Nest -> Postgres row se_1
                 Nest -> Redis job laytime:se_1
                 worker -> Socket.IO
```

- homework
```text
// 34a  redraw with se_1
// 34b  cross out Redis for Anemi
// 34c  the number 28125.0 comes from the function, not from the gateway
```

- example 35. the boxes, with q_1 as the row
- nginx, Nest, Postgres, Redis, worker, Socket.IO
- job id laytime:q_1
- figure 0 is computed in the worker from the pure function
- Anemi crosses out Redis and the worker

```text
clients -> nginx -> Nest -> Postgres row q_1
                 Nest -> Redis job laytime:q_1
                 worker -> Socket.IO
```

- homework
```text
// 35a  redraw with q_1
// 35b  cross out Redis for Anemi
// 35c  the number 0 comes from the function, not from the gateway
```

- example 36. the boxes, with r_1 as the row
- nginx, Nest, Postgres, Redis, worker, Socket.IO
- job id laytime:r_1
- figure 0 is computed in the worker from the pure function
- Anemi crosses out Redis and the worker

```text
clients -> nginx -> Nest -> Postgres row r_1
                 Nest -> Redis job laytime:r_1
                 worker -> Socket.IO
```

- homework
```text
// 36a  redraw with r_1
// 36b  cross out Redis for Anemi
// 36c  the number 0 comes from the function, not from the gateway
```
