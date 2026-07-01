## Redis Foundations

- **What is Redis?**
	Redis is an open-source, in-memory data store that is commonly used as a database, cache, and message broker. Because it stores data in memory (RAM) instead of on disk, it can read and write data extremely quickly—often in less than a millisecond.
	
- It supports multiple data types:
	Strings, Lists, Sets, Hashes, Sorted Sets, Streams

- Common use cases:
	1. **Caching**: Store frequently accessed data to reduce database load.
	2. **Session storage**: Keep user login sessions for web applications.
	3. **OTP store**: Keeps otps for fast retrieval.
	4. **Rate limiting**: Rate limit suspicious or spam requests.
	5. **Real-time analytics**: Count page views, likes, or active users.
	6. **Message queues**: Pass messages between different services.
	7. **Leaderboards**: Rank players in games using sorted sets.

- Example:
	- Suppose your application needs to display a user's profile repeatedly.

	- Without Redis:
	App → Database → User Profile

	- With Redis:
			App → Redis Cache → User Profile
									↓ (if not found)
							Database
							
	- The first request loads the profile from the database and stores it in Redis. Future requests are served directly from Redis, making the application much faster.

- When to use?
	- To lower read pressure
	- To store short lived temp data
	- To keep shared counters
	- To manage background jobs