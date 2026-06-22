# Monitoring & Logging

## Logging

- Django logging configured for different levels (debug, info, warning, error).
- Access logs via custom middleware.
- Celery task logs are captured.
- Log files are rotated daily in production.

Recommended log locations on VPS:
- `/var/www/mwecau_digivote/src/logs/`
- Systemd journal: `journalctl -u gunicorn-digivote`

## Key Metrics to Monitor

- Voter turnout during active elections (already exposed via internal APIs for observers)
- Login failure rates (brute force signals)
- Celery queue length (`email_queue` and `notification_queue`)
- Database connection pool usage
- Redis memory usage
- Email delivery success rate (via Brevo / AWS dashboards)

## Health Checks

Basic health can be verified by:
- Loading the landing page
- Checking `/admin/` (for staff)
- Verifying an active election appears correctly

More advanced monitoring (Prometheus + Grafana, etc.) can be added later without changing application code significantly.
