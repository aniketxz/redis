## 03-site-banner

- Redis stores in key value pairs
- Naming convention for key:
  ```js
    key = 'feature-name:key-name'
  ```

- Usage:
  1. redis.set(key, message) → creates a new pair
  2. redis.get(key) → view the value saved using the key
  3. redis.del(key) → deletes the pair
  4. redis.exists(key) → returns if the key exists