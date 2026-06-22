# Scalability Considerations

Current production deployment runs on a single VPS.

## Current Architecture Limits

- Single application server (Gunicorn workers)
- Single Celery worker process
- Shared Redis on the same machine
- Single database instance

This setup is sufficient for university-wide student elections (typically a few thousand voters).

## Planned Horizontal Scaling Path

When load increases or high availability is required:

1. **Multiple web servers**
   - Behind a load balancer (nginx or AWS ALB)
   - All point to the same MySQL and Redis

2. **Dedicated Celery workers**
   - Separate machine(s) for background tasks
   - Use Redis as broker (already done)

3. **Database**
   - Read replicas for reporting / observer dashboards
   - Primary for writes (voting, admin actions)

4. **Redis**
   - Can stay as single instance for cache + broker in most university scenarios
   - For very large scale, consider Redis Cluster or separate instances for cache vs broker

## Stateless Design

The application is designed to be stateless:
- Sessions stored in Redis
- Media on Cloudinary
- No local filesystem dependencies for user data

This makes adding more web servers straightforward.

## Caching Strategy Benefits

Aggressive use of Redis caching (election results, turnout, rate limiting) significantly reduces database load during peak voting periods.
