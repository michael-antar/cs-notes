#Databases #Backend #Architecture
(**Remote Dictionary Server**) An **in-memory Key-Value store** used as a _database, cache, and message broker_.

It falls under the _Key-Value store_ category of [[NoSQL]]. It is a giant, global [[Hash Map]]. Every piece of data has a unique ID (Key), and you use that ID to find it. You cannot "search" by value (e.g., you can't easily say `SELECT * WHERE name=John`). _You must know the key_.

It is unique in the way that it _manipulates data inside the memory_.
- It doesn't just store text, it natively understands and manipulates complex data structures (Linked Lists, Hash Maps, Sets) inside the memory.
- You can ask Redis to "pop the last item off the list" or "increment this specific counter" and it does it _atomically_ in memory without moving the whole object back and forth to your app.

## Usage
It is _rarely used as a main database_. While it supports persistence (saving to disk), it is almost exclusively used as a _secondary "Speed Layer" (Cache)_. 
- **Cost**: RAM is expensive, Storing 1 TB of data in Redis costs 100x more than storing it in [[MySQL]] on a hard drive.
- **Data Safety**: Because it lives in RAM, if the server loses power and the backup snapshot hasn't run recently, you lose the data in between.
- **Capacity**: You are strictly limited to the amount of RAM on the machine. You can't just "add more disk space".

## Speed
Redis can handle millions of requests per second with sub-millisecond accuracy.
==TODO==

## Caching Strategies
Redis has 2 _internal backup mechanisms_:
- **Redis Database Snapshot (RDB)**: Every X minutes (e.g, every 15 minutes), Redis takes a "photo" of the entire RAM and saves it as a `.rdb` file on the hard drive.
	- _Pros_: Very fast restart. Compact file size
	- _Cons_: If the server crashes at 2:14 PM, and the last snapshot was 2:00 PM, you lose 14 minutes of data.
	- _Use Case_: IoT Telemetry (Losing 10 minutes of temperature data is annoying but not fatal)
- **Append Only File (AOF)**: Redis logs _every single write command_ to a file (like a transaction log)
	- _Pros_: Zero data loss (or maximum 1 second loss)
	- _Cons_: The file gets huge. Restarting is slow because Redis has to "replay" every command to rebuild the memory
	- _Use Case_: Financial transactions or Payment processing.

### Caching Pattern
Since Redis is _volatile_ (data can be evicted or expired), your code must handle the case where the data is missing. This is called the **Cache-Aside Pattern (Lazy Loading)**:
1. **Request**: User asks "Give me Device 101"
2. **Check Redis**: App asks Redis `GET device:101`
3. **Cache Miss**: Redis says `null` (Data was evicted or never loaded)
4. **Fetch DB**: App queries MySQL `SELECT * FROM devices WHERE id=101`
5. **Populate Cache**: App saves the result to Redis `SET device:101 ...`
6. **Return**: App sends data to the user

**The "Cache Hit" ratio:** The next time someone asks for Device 101, Step 3 returns the data instantly. You skip steps 4, 5, and 6.

### Cache Warming
Sometimes you don't want to wait for the user to ask for data. You want the data to be _ready before they ask_. This is called **Cache Warming**.

**Scenario**: You are deploying a new version of the Redis server. It _starts empty (0% Hit Rate)_. The first 10,000 requests will all miss Redis and hit MySQL. This might crash MySQL.

**The Solution**: You write a script that runs immediately after Redis starts:
1. Query MySQL for the "Top 1,000 Most Active Users"
2. Push them into Redis immediately
3. _Then_ open the gates to traffic.

## Data Retention
_It is expected for Redis data to be cleared_.

There are two ways data leaves Redis:
- **Time-To-Live (TTL)**: "_Intentional Expiration_". You set a timer on specific keys
	- _Command_: `SET session:user:123 "token" EX3600` (Expire in 1 hour)
	- _Why_: You want the user to be forced to re-login eventually. Or you only care about the "Device Status" for 5 minutes. If the device hasn't reported in 5 minutes, you want the key to vanish so the system assumes it is OFFLINE.
- **Eviction (LRU)**: "_Forced Removal_". When Redis memory is full (e.g., you hit the 16GB limit), it must delete something to accept new writes.

## Commands 
**Set Data**: `SET key value`
**Get Data**: `GET key`