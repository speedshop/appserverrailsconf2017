
![](soldpupper.mp4)

---

![fit](terrence1.jpg)

---

![fit](terrence2.jpg)

---

![fit](terrence3.jpg)

---

![fit](terrence4.jpg)

---

# [fit] Your App Server
# [fit] Config
# [fit] is Wrong

---

# [fit] Who the heck am I?

---

![](skiing2.mov)

---

![fit](nap.jpg)

---

![fit](nate_perf_explanation.gif)

---

# [fit] memelord

---

![fit](spicy.jpg)

---

![fit](spicyjava.jpg)

---

![fit](spicywinner.jpg)

---

![fit](blog.png)

---

![fit](speedshop_s_big.png)

---

# [fit] railsspeed.com

---

# Incorrect app server configuration is the most common issue I see on client applications.

---

# [fit] Overprovisioning

---

# [fit] General Rule:
## If you spend more on Heroku per month than you have RPM, you're overprovisioned

---

# [fit] Undersizing

---

# [fit] Definitions

---

# [fit] Container (DYNO)

---

# [fit] Worker (Process)

---

# [fit] Thread

---

# Process:

1. Determine theoretical capacity
2. Determine memory usage per worker
3. Choose container size + worker counts
4. Check connection limits
5. Deploy and monitor queue depths, CPU, memory, restarts, timeouts.

---

# [fit] Little's Law

---

$$
l =  \lambda * W
$$

---

# Things in system = Arrival Rate x Time Spent in System

---

# Requests in System = Requests/Second x Average Response Time

---

# Dividing average requests in system by how many workers we have gives us a utilization percentage

---

# [fit] Averages

---

# [fit] Envato, 2013

* Envato receives 115 requests per second
* They run an average of 147ms response time
* They run 45 workers

---

# [fit] 16.9 = 115 * 0.147

---

# [fit] 16.9 / 45 = 37% Utilization

---

# Take Little's Law, multiply by a factor of 5 to get your initial estimate

---

![fit](dynoload.png)

---

# [fit] Step 1: Estimate Worker Count

---

# [fit] Container size?

---

![fit](flat.jpeg)

---

![](duped-cut.mp4)

---

![fit](log.jpeg)

---

![fit](workerkiller.jpeg)

---

# Deploy with 1x/2x dynos, 1 worker per dyno, 5 threads per worker, and measure average RSS after 24 hours.

---

# Average app is 256-512MB per worker.

---

![fit](david_meme.jpg)

---

# [fit] Step 2: Determine memory per worker

---

# Container choice on Heroku

---

![](cozy.mp4)

---

Name|RAM|vCPU count|"Compute"|$/month
---|---|---|---|---
1x | 512MB | 8 | 1-4x | 25
2x | 1GB | 8 (2x share) | 4-8x | 50
Perf-M | 2.5GB | 2 | 12x | 250
Perf-L | 14GB | 8 | 50x | 500

---

Name|RAM/$|$/Compute
---|---|---
1x | 21 | 10
2x | 21 | 8.3
Perf-M | 10 | 20
Perf-L | 28 | 10

---

# Note that 2x dynos access 8 CPUs
## Higher thread counts required?

---

# If > ~25 app instances, use Perf-L. Otherwise, use 2x.

---

# Aim for *at least* 3 workers per dyno (may have to Perf-M otherwise)

---

# Higher workers per dyno equals better routing

---

# You can reduce thread count to 3 or use jemalloc to try to squeeze another worker into a dyno

---

# Workers can be 3-4x core counts

---

# Keep thread counts to 3-5.

---

# How do I know if my app is threadsafe?

* Start slow, with 2 threads.
* Try `minitest/hell`.
* It's *probably fine* (on MRI), watch for globals (Redis) or class-level state (User.current)

---

# Step 3: Choose container size and worker count

---

# Things that use connections: ActiveRecord, Redis, Memcached, etc

---

# Connection pools and limits to watch

* ActiveRecord (`database.yml`). 1 connection per thread.
* Postgres/database.
* Redis (Sidekiq, etc).
* Memcache (usually unlimited).

---

# You may need more than 1 DB connection per thread

---

Name | Connections
---|---
Hobby | 20
Standard-0 | 120
Standard-2 | 400
Standard-4 | 500

---

# Need more than 500 connections? Use pgbouncer.

---

# Do the math to figure out how many dynos outscale your limits.

---

# Example

* I have a Perf-L dyno with 20 app workers, each with thread count of 5.
* That's 100 threads (and 100 DB connections) per dyno.
* I can scale to 5 of these dynos before outscaling my connection limits.

---

# Step 4: Check connection limits

---

# Things to Watch after Deployment

---

# [fit] Memory

---

![fit](memory.png)

---

![fit](swap.png)

---

![fit](fatcontrolle.jpg)

---

# Skylight
# Scout
# `oink`

---

## If too much, scale down `WEB_CONCURRENCY`
## If too little, scale up `WEB_CONCURRENCY`

---

# Can also tweak thread counts
## `jemalloc` or `MALLOC_ARENA_MAX`

---

![fit](malloc-arena-max.png)

---

# [fit] Queue Times

---

# [fit] High queue times? More dynos.

---

# [fit] CPU Usage

---

# If low, may benefit from more threads.

---

# [fit] Restarts

---

![fit](dyno.jpg)

---

# Apps which have high 95th percentile times may benefit from high workers-per-dyno

---

`passenger_max_request_time` and `worker_timeout`

---

# Process:

1. Determine theoretical capacity
2. Determine memory usage per worker
3. Choose container size + worker counts
4. Check connection limits
5. Deploy and monitor queue depths, CPU, memory, restarts, timeouts.


## @nateberkopec speedshop.co

---

# [fit] `preload_app!`

---

# [fit] `lowlevel_error_handler`

---

# [fit] `queue_requests`
