# Email Configuration

MWECAU DigiVote supports multiple email providers through Django's email backend system and Celery.

## Supported Backends

### 1. Brevo (Recommended for most deployments)

Uses `django-anymail`.

```env
EMAIL_BACKEND=anymail.backends.brevo.EmailBackend
BREVO_API_KEY=your_brevo_api_key
DEFAULT_FROM_EMAIL="MWECAU DigiVote <noreply@yourdomain.com>"
```

### 2. Standard SMTP

Any SMTP-compatible service (institutional mail server, Gmail, Mailgun, etc.).

```env
EMAIL_BACKEND=django.core.mail.backends.smtp.EmailBackend
EMAIL_HOST=smtp.yourdomain.com
EMAIL_PORT=587
EMAIL_USE_TLS=True
EMAIL_HOST_USER=...
EMAIL_HOST_PASSWORD=...
```

### 3. AWS SES

```env
EMAIL_BACKEND=django_ses.SESBackend
AWS_ACCESS_KEY_ID=...
AWS_SECRET_ACCESS_KEY=...
AWS_SES_REGION_NAME=eu-west-1   # or your region
```

All email sending is queued through Celery (`email_queue`) to prevent blocking the request cycle.

## Important Implementation Details

- Password reset tokens, verification emails, election notifications, and vote confirmations are all sent asynchronously.
- The system falls back to the console backend in development when no credentials are configured.
- Rate limiting is applied at the task level for notification-heavy operations.

## Queue Separation

- `email_queue`: Regular transactional emails (password resets, verifications)
- `notification_queue`: Bulk or time-sensitive election notifications

This separation allows independent scaling of workers.

See the main Celery architecture diagram for visual representation.
