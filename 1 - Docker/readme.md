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
```
## Ingesting CSV Data into PostgreSQL Database

This Python script is an essential component of our data pipeline. It's designed to efficiently ingest CSV data into a PostgreSQL database, enabling subsequent analysis and transformation tasks. Below is an overview of its purpose and functionality:

### General Purpose:
- The script automates the process of downloading CSV data from a specified URL and loading it into a PostgreSQL database.
- It handles large datasets efficiently by reading data in chunks and utilizing parallel processing capabilities.

### Key Features:
- **Dynamic Configuration:** The script accepts configuration parameters (user, password, host, port, database name, table name, and CSV file URL) via command-line arguments.
- **Data Download:** It downloads the CSV file from the provided URL, handling both regular CSV and gzip-compressed CSV files.
- **Database Interaction:** It establishes a connection to the PostgreSQL database using SQLAlchemy, creates the necessary table, and inserts data in chunks.
- **Chunk Processing:** Data is read from the CSV file in chunks to optimize memory usage and processing speed. Each chunk is converted to datetime objects before insertion into the database.
- **Progress Monitoring:** The script provides feedback on the progress of data ingestion, including the time taken for each chunk insertion.

### Usage:
1. **Command-line Arguments:** Provide the required PostgreSQL credentials, database details, table name, and CSV file URL as command-line arguments.
2. **Execution:** Execute the script to initiate the data ingestion process. Progress updates and completion messages will be displayed in the terminal.

### Example Command:
```bash
python ingest_data.py --user myuser --password mypassword --host localhost --port 5432 --db mydatabase --table_name mytable --url https://example.com/data.csv
