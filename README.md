# SQL Problems PostgreSQL Docker Setup

This setup provides a PostgreSQL database with all SQL problems data pre-loaded using Docker Compose.

## What's Included

- **PostgreSQL 15** database server with SQL problems data
- **Apache Spark 3.5.0** with Jupyter Notebook integration
- **All SQL problems data** automatically loaded on startup
- **Persistent storage** for database data
- **Pre-configured JDBC connectivity** between Spark and PostgreSQL

## Quick Start

1. **Start the services:**
   ```bash
   docker-compose up -d
   ```

2. **Connect to PostgreSQL:**
   - **Host:** localhost
   - **Port:** 5432
   - **Database:** postgres
   - **Username:** postgres
   - **Password:** postgres

3. **Access Jupyter Notebook:**
   - Open http://localhost:8888 in your browser
   - No authentication required (token disabled for development)
   - Sample notebook: `spark_postgres_connection.ipynb`

4. **Access Spark UI:**
   - Open http://localhost:4040 in your browser (when Spark is running)

## Database Structure

The database contains **50 SQL problems** loaded as individual tables in the `public` schema. Each problem's data is stored in separate tables with original names from the problem scripts.

## Example Usage

```sql
-- List all schemas
SELECT schema_name FROM information_schema.schemata WHERE schema_name LIKE '%problem%';

-- Switch to a specific problem schema
SET search_path TO medium_problem_1, public;

-- List tables in current schema
\dt

-- Example query for medium problem 1 (friend requests)
SELECT * FROM fb_friend_requests LIMIT 5;
```

## Management Commands

```bash
# Start services
docker-compose up -d

# Stop services
docker-compose down

# View logs
docker-compose logs postgres

# Restart database (reload data)
docker-compose restart postgres

# Connect to database via psql
docker-compose exec postgres psql -U postgres -d sql_problems

# Remove everything (including data)
docker-compose down -v
```

## Troubleshooting

- **Port already in use:** Change the port mapping in `docker-compose.yml`
- **Database won't start:** Check logs with `docker-compose logs postgres`
- **Data not loading:** Verify `docker-init/init.sql` exists and is readable

## File Structure

```
sql_sandbox/
├── docker-compose.yml          # Docker Compose configuration
├── docker-init/
│   └── init.sql               # Combined SQL initialization script
├── sql_problems/              # Original problem files
│   ├── medium/
│   └── hard/
└── collect_sql_scripts.py     # Script to regenerate init.sql
```