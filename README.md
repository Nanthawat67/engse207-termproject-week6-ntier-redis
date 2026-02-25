🚀 Week 6 – N-Tier Architecture with Redis Caching & Load Balancing
📌 Overview

This project implements a scalable N-Tier Architecture using:

Node.js (Backend API)

PostgreSQL (Database)

Redis (Cache – Cache Aside Pattern)

Nginx (Load Balancer – Round Robin)

Docker Compose (Container Orchestration)

The system supports horizontal scaling and improves performance using distributed caching.

🏗 Architecture
Client → Nginx → 3x App Instances → PostgreSQL
                         ↓
                       Redis

Nginx distributes requests using Round Robin.

Redis caches frequently accessed data.

Each backend instance exposes a unique instanceId.

🚀 Run the Project

Start with 3 backend instances:

docker compose up --scale app=3

Verify running containers:

docker ps
⚖️ Load Balancing Test
for i in $(seq 1 6); do
  curl -s http://localhost/api/health | grep instanceId
done

✔ Instance IDs should rotate between app-1, app-2, and app-3.

🧠 Redis Caching (Cache-Aside Pattern)
First Request

CACHE MISS

Query PostgreSQL

Store result in Redis

Subsequent Requests

CACHE HIT

Return data from Redis

Faster response time

Check Redis stats:

docker exec taskboard-redis redis-cli INFO stats

Important metrics:

keyspace_hits

keyspace_misses

🔄 Cache Invalidation

Creating a new task invalidates the cache:

curl -X POST http://localhost/api/tasks \
  -H "Content-Type: application/json" \
  -d '{"title":"Test","priority":"HIGH"}'

Next request → MISS
Following requests → HIT

Ensures data consistency.

🏥 Health Check Endpoint
curl http://localhost/api/health

Returns:

instanceId

database status

redis status

cache statistics

📊 Performance Observation

Initial request slower (database access)

Cached requests significantly faster

Reduced database load

Improved scalability

✅ Deliverables Completed

 docker-compose supports --scale app=3

 Redis caching working (HIT/MISS visible)

 Load balancing verified

 Frontend displays instance information

 Health endpoint includes cache stats

 Proper Git commit with detailed message

🎯 Conclusion

This project demonstrates:

Horizontal scaling with Docker Compose

Load balancing using Nginx

Performance optimization with Redis

Cache-Aside pattern with invalidation

Production-style backend architecture

The system is scalable, efficient, and ready for real-world deployment scenarios.
