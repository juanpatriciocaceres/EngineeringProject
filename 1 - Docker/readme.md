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
```

## Setting Up the Project Environment on Google Cloud Platform (GCP)

This guide outlines the process of setting up the project environment on Google Cloud Platform (GCP) to facilitate development and deployment tasks. Below are the steps involved:

1. **Generating SSH Keys:**
   SSH keys are generated to securely authenticate and establish connections between the local machine and the virtual machine (VM) on GCP.

2. **Creating a Virtual Machine on GCP:**
   A VM instance is created on GCP to host the project resources and services. This VM serves as the primary environment for development and deployment tasks.

3. **Connecting to the VM with SSH:**
   Using SSH, establish a secure connection to the VM instance on GCP from the local machine's terminal.

4. **Installing Anaconda:**
   Anaconda, a comprehensive Python distribution, is installed on the VM to manage Python packages and environments efficiently.

5. **Installing Docker:**
   Docker is installed on the VM to facilitate containerization of project dependencies and services, ensuring consistency and portability across environments.

6. **Creating SSH Config File:**
   A SSH configuration file is created on the local machine to simplify SSH connections to the remote VM instance.

7. **Accessing the Remote Machine with VS Code and SSH Remote:**
   Visual Studio Code (VS Code) is configured to connect to the remote VM instance via SSH, allowing seamless remote development and file management.

8. **Installing Docker-Compose:**
   Docker-Compose is installed on the VM to manage multi-container Docker applications, simplifying the deployment and orchestration of complex services.

9. **Installing pgcli:**
   pgcli, a PostgreSQL client with auto-completion and syntax highlighting, is installed on the VM to facilitate interactions with PostgreSQL databases.

10. **Port-forwarding with VS Code: Connecting to pgAdmin and Jupyter from the Local Computer:**
    Port-forwarding is configured in VS Code to connect to pgAdmin (for PostgreSQL management) and Jupyter (for interactive data analysis) running on the remote VM from the local computer's browser.

11. **Installing Terraform:**
    Terraform is installed on the VM to provision and manage GCP resources programmatically, enabling infrastructure as code practices.

12. **Using SFTP for Putting the Credentials to the Remote Machine:**
    Secure File Transfer Protocol (SFTP) is used to transfer credentials and sensitive data securely from the local machine to the remote VM instance.

13. **Shutting Down and Removing the Instance:**
    Once the project tasks are completed, the VM instance is shut down and removed from GCP to avoid unnecessary costs and resource usage.

By following these steps, the project environment is set up efficiently on Google Cloud Platform, providing a robust foundation for development, deployment, and collaboration.
