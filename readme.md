## 03-site-banner

- Redis stores in key value pairs
- Naming convention for key:

  ```js
  key = 'feature-name:key-name'
  ```

- Usage:

```js
redis.set(key, message) // creates a new pair

redis.get(key) // view the value saved using the key

redis.del(key) // deletes the pair

redis.exists(key) // returns if the key exists
```

---

## 04-otp-login-with-ttl

1. TTL → time to live
2. In this section we learn about ttl by using it in implementing auto expire otps

3. New Redis Method:

```js
  redis.set(otpkey, otp, 'EX', duration-in-sec) // EX → expire, set ttl

  redis.ttl(otpkey) // returns: remaining time if not expired, -2 if expired
```

Extra: we can store otps like this for more robust auth system:

```js
value = {
	otp: otp,
	attempts: 0,
	maxAttempts: 3,
	createdAt: timestamp,
	lastAttemptedAt: timestamp,
	blockedUntil: timestamp,
}
```

---

## 05-user-profile-cache-json-v-hash

1. Redis can also store objects as values
2. In this section, user profile is stored using `set` and `hset`
3. `set → strings` no updating, replace whole value
4. `hset → objects` can update singular fields

5. You can still store objects in set if you stringify it before storing and can parse before sending response

6. Hash commands:

```js
const key = 'user:123:hash'

// 1. HSET - Storing/Updating object fields natively
await redis.hset(key, {
	name: 'Aniket',
	email: 'aniket@aniket.com',
	phone: '123456',
})

// 2. HEXISTS - Checking if a specific field is present
const hasEmail = await redis.hexists(key, 'email')
console.log(hasEmail) // Returns 1 (true) or 0 (false)

// 3. HGETALL - Retrieving the entire object
const userProfile = await redis.hgetall(key)
console.log(userProfile)
// Output: { name: 'Aniket', email: 'aniket@aniket.com', phone: '123456' }

// 4. HDEL - Deleting a specific field from the object
await redis.hdel(key, 'phone')

// If you run HGETALL again now, 'phone' will be completely gone,
// but 'name' and 'email' remain perfectly intact.
```

---

## 06-email-queue-w-redis-lists

1. Using redis as queue in this section
2. Queue/List commands:

```js
const queueKey = 'my_queue'

// Adding to the left
await redis.lpush(queueKey, 'Job C')
await redis.lpush(queueKey, 'Job B', 'Job A') // You can push multiple items at once

// Adding to the right
await redis.rpush(queueKey, 'Job D')
await redis.rpush(queueKey, 'Job E')

// Grab the item from the front of the list
const firstJob = await redis.lpop(queueKey)
console.log(firstJob)
// Output: "Job A"

// Grab the item from the back of the list
const lastJob = await redis.rpop(queueKey)
console.log(lastJob)
// Output: "Job E"
```

3. lpush & rpush → adds one or multiple elements
4. lpop & rpop → removes and return an element

5. DRAWBACKS of using redis as a job queue:
   1. Job loss → once you pop a job it is lost
   2. Retry → since job is lost you cannot retry if process failed
   3. Parallel workers → no parallel workers

---

## 07-order-confirmation-jobs-w-bullmq

1. BullMQ → Open source Message Queue for Redis. A library which helps to use redis as job queue.
2. queue.js → defines connection and queue
3. api.js →
   1. exposes an endpoint to receive job requests
   2. it uses the queue defined in queue.js
   3. call queue.add method to add new job
   4. it takes three props: job-name, job-data, job-config-options
4. worker.js → picks job from specified queue and executes it

5. To test the project
   1. run `npm run api` and `npm run worker` in two terminals
   2. send a POST request to `http://localhost:3000/welcome-email` and request.body=`{"to": "recipient@gmail.com", "name":"recipient name"}`

---

## 08

1. pub-sub (publisher-subscriber) is a notification broadcasting architecture.
2. You publish notifications using publisher.
3. And subscribers who are active can receive those notifications.
4. There can be inactive subscribers, who do not receive notifications unless they become active.

5. To test the project:
   1. run `npm run api` and `npm run subscriber` in two terminals
   2. send a POST request to `http://localhost:3000/notifications` and request.body=`{"title": "notification you want to send"}`
