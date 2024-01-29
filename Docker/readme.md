## Containerization Step Using Docker Compose on Google Cloud Platform (GCP)

To facilitate data storage and management, we utilize Docker containers deployed within a virtual machine instance on Google Cloud Platform (GCP). The following Docker Compose configuration provisions PostgreSQL and pgAdmin containers, enabling the creation of a SQL database and providing a user-friendly interface for database management:

```yaml
services:
  pgdatabase:
    image: postgres:13  # Docker image for PostgreSQL version 13.
    environment:
      - POSTGRES_USER=root  # PostgreSQL username (root).
      - POSTGRES_PASSWORD=root  # PostgreSQL password (root).
      - POSTGRES_DB=ny_taxi  # Name of the PostgreSQL database (ny_taxi).
    volumes:
      - "./ny_taxi_postgres_data:/var/lib/postgresql/data:rw"
      # Mounts a local directory (ny_taxi_postgres_data) to store PostgreSQL data.
    ports:
      - "5432:5432"  # Exposes port 5432 on the host for PostgreSQL connections.

  pgadmin:
    image: dpage/pgadmin4  # Docker image for pgAdmin.
    environment:
      - PGADMIN_DEFAULT_EMAIL=admin@admin.com  # Default email for pgAdmin.
      - PGADMIN_DEFAULT_PASSWORD=root  # Default password for pgAdmin.
    ports:
      - "8080:80"  # Exposes port 8080 on the host for pgAdmin web interface.
