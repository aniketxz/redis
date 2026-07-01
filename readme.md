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
