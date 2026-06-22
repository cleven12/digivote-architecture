# Database: MySQL

MWECAU DigiVote uses **MySQL** (or MariaDB) as the primary relational database.

## Why MySQL

- Excellent support in Django via `django.db.backends.mysql` (pymysql or mysqlclient).
- Widely used and well-supported in Tanzanian university and government environments.
- Good balance of performance, features, and operational familiarity.
- Easy replication and backup tooling available.

## Configuration

In `mw_es/settings.py`:

```python
DATABASES = {
    'default': {
        'ENGINE': 'django.db.backends.mysql',
        'NAME': config('DB_NAME'),
        'USER': config('DB_USER'),
        'PASSWORD': config('DB_PASSWORD'),
        'HOST': config('DB_HOST', default='localhost'),
        'PORT': config('DB_PORT', default='3306'),
        'CONN_MAX_AGE': 60,
        'OPTIONS': {
            'connect_timeout': 10,
            'charset': 'utf8mb4',
        },
    }
}
```

Recommended `.env` example:

```env
DB_ENGINE=django.db.backends.mysql
DB_NAME=digivote_db
DB_USER=digivote_user
DB_PASSWORD=...
DB_HOST=localhost
DB_PORT=3306
```

## Recommended MySQL Settings

- Use `utf8mb4` character set and `utf8mb4_unicode_ci` collation.
- Enable `innodb_file_per_table`.
- Set appropriate `innodb_buffer_pool_size` (50-70% of available RAM on the DB server).
- Use `CONN_MAX_AGE` in Django to reuse connections.

## Migrations

All schema changes are managed through Django migrations.

After any model change:

```bash
python manage.py makemigrations
python manage.py migrate
```

## Backup & Recovery

Recommended approach:
- Daily logical dumps using `mysqldump --single-transaction`
- Point-in-time recovery with binary logs when possible
- Regular test restores

Example backup script:

```bash
mysqldump -u$DB_USER -p$DB_PASSWORD --single-transaction --routines --triggers digivote_db > backup-$(date +%F).sql
```

## Notes for Production

- The application was previously documented with PostgreSQL in early versions.
- Migration to MySQL is complete and is now the supported production database.
- Always run migrations in a maintenance window or during low-traffic periods.
