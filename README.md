# Nomupay

Nomupay is a web-based e-wallet application built using the CodeIgniter 4 framework. This application is specifically designed for businesses that want to provide e-wallet services to their customers.

---

## Getting Started

1. **Clone the Repository:**

   ```bash
   git clone [https://github.com/ridwanpr/nomupay](https://github.com/ridwanpr/nomupay)
   cd nomupay
   ```

2. **Configure Environment:**
   Copy the environment template:
   ```bash
   cp env .env
   ```
   Configure your database credentials according to your chosen setup method below.

---

## How to Run the Application

Choose **Option A** to run via Docker (recommended if your host machine runs PHP 8.4+), or **Option B** to run directly on your host machine.

---

### Option A: Using Docker Compose

#### Prerequisites

- Docker & Docker Compose installed.
- Ensure your `docker-compose.yml` includes the shared external network:

```yaml
services:
  app:
    build:
      context: .
      dockerfile: Dockerfile
    container_name: nomupay-app
    restart: unless-stopped
    ports:
      - "8080:80"
    volumes:
      - .:/var/www/html
    networks:
      - nomupay-net
    environment:
      CI_ENVIRONMENT: development

networks:
  nomupay-net:
    external: true
```

#### Steps

1. **Create the shared network (run once):**

   ```bash
   docker network create nomupay-net
   ```

2. **Configure Database Connection:**
   - **If your MySQL database runs in a separate Docker container (e.g., named `mysql`):**
     Connect your database container to the shared network (only needed once):

     ```bash
     docker network connect nomupay-net mysql
     ```

     In your `.env` file, point the hostname to your container name:

     ```ini
     database.default.hostname = mysql
     database.default.database = nomupay
     database.default.username = your_mysql_user
     database.default.password = your_mysql_password
     ```

   - **If your MySQL database runs directly on your host machine:**
     You do not need to run `docker network connect`. In `.env`, set the host to reach your host machine:
     ```ini
     database.default.hostname = host.docker.internal
     database.default.database = nomupay
     database.default.username = your_mysql_user
     database.default.password = your_mysql_password
     ```

3. **Start the application container:**

   ```bash
   docker compose up -d --build
   ```

4. **Install Composer dependencies:**

   ```bash
   docker compose exec app composer install
   ```

5. **Run database migrations:**

   ```bash
   docker compose exec app php spark migrate
   ```

6. **Access the application:**
   Open your browser and navigate to `http://localhost:8080`.

7. **Stop the application:**
   ```bash
   docker compose down
   ```

---

### Option B: Local Setup (Without Docker)

#### Prerequisites

- PHP version 8.0 up to 8.3 installed with extensions: `intl`, `mbstring`, `mysqli`.
- [Composer](https://getcomposer.org/download/) installed.
- Local MySQL/MariaDB server running on your machine.

#### Steps

1. **Configure Database:**
   In `.env`, set the hostname to your local machine:

   ```ini
   database.default.hostname = localhost
   database.default.database = nomupay
   database.default.username = root
   database.default.password = your_local_password
   ```

2. **Install dependencies:**

   ```bash
   composer install
   ```

3. **Run database schema migrations:**

   ```bash
   php spark migrate
   ```

4. **Start the development server:**

   ```bash
   php spark serve
   ```

   The application will run at `http://localhost:8080`.

5. **Access the application:**
   Open your browser and visit `http://localhost:8080`.

---

## Contribution

We welcome contributions from various parties to further develop this application. Please note that this project is also the final assignment for the Web Programming II course at Bina Sarana Informatika University (Universitas BSI). If you would like to contribute, please create a pull request, and we will review it.

## License

This project is licensed under the MIT License. Please see the [LICENSE](LICENSE) file for more information.
