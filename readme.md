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
  blockedUntil: timestamp
}
```