To containerize a MySQL database and ensure it contains a specific database with schemas, tables, and data, you can follow these steps:

### 1. Dockerize MySQL Database

First, create a Dockerfile to build your custom MySQL image. Here’s a basic example:

#### Dockerfile

```dockerfile
# Use an official MySQL image as a base
FROM mysql:5.7

# Environment variables
ENV MYSQL_ROOT_PASSWORD=root_password
ENV MYSQL_DATABASE=my_database
ENV MYSQL_USER=db_user
ENV MYSQL_PASSWORD=db_password

# Copy SQL scripts to initialize the database
COPY ./schema.sql /docker-entrypoint-initdb.d/schema.sql

# Expose MySQL port
EXPOSE 3306

# Start MySQL service
CMD ["mysqld"]
```

In this Dockerfile:

- **FROM mysql:5.7**: This line specifies the base image as MySQL 5.7. You can adjust the version according to your requirements.
  
- **ENV**: Sets environment variables for MySQL root password (`MYSQL_ROOT_PASSWORD`), database name (`MYSQL_DATABASE`), username (`MYSQL_USER`), and password (`MYSQL_PASSWORD`).

- **COPY ./schema.sql /docker-entrypoint-initdb.d/schema.sql**: Copies a SQL script (`schema.sql`) into the Docker image’s initialization directory (`/docker-entrypoint-initdb.d/`). MySQL will execute scripts in this directory when the container starts, initializing the database with schemas and data.

- **EXPOSE 3306**: Exposes MySQL port 3306 to allow connections from outside the container.

- **CMD ["mysqld"]**: Specifies the default command to start MySQL server when the container starts.

### 2. Prepare Initialization SQL Script

Create a `schema.sql` file that contains SQL commands to create your database schemas, tables, and insert initial data. For example:

#### schema.sql

```sql
-- Create database
CREATE DATABASE IF NOT EXISTS my_database;

-- Use the database
USE my_database;

-- Create tables
CREATE TABLE IF NOT EXISTS users (
    id INT AUTO_INCREMENT PRIMARY KEY,
    username VARCHAR(50) NOT NULL,
    email VARCHAR(100) NOT NULL
);

-- Insert initial data
INSERT INTO users (username, email) VALUES
    ('user1', 'user1@example.com'),
    ('user2', 'user2@example.com');
```

Adjust the SQL commands in `schema.sql` according to your database schema requirements.

### 3. Build and Run the Docker Image

Build your Docker image using the Dockerfile and `schema.sql` file prepared:

```bash
docker build -t mysql-custom-image .
```

Run your newly created Docker image to start the MySQL container:

```bash
docker run -d -p 3306:3306 --name mysql-container mysql-custom-image
```

- `-d`: Runs the container in detached mode.
- `-p 3306:3306`: Maps port 3306 on your host to port 3306 on the container (MySQL default port).
- `--name mysql-container`: Specifies a name (`mysql-container`) for your container.

### 4. Verify and Access the Database

Verify that your MySQL container is running:

```bash
docker ps
```

Access the MySQL database from your host or another container:

```bash
# Connect using MySQL client
mysql -h 127.0.0.1 -P 3306 -u db_user -p

# When prompted, enter the password (db_password)
```

### Summary

By following these steps, you have containerized a MySQL database with a specific database schema and initial data. This approach allows you to deploy and manage your MySQL instance as a Docker container, ensuring consistency and portability across different environments. Adjust the Dockerfile, initialization SQL script, and Docker run command as necessary based on your specific database requirements and deployment environment.