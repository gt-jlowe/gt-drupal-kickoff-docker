# Drupal Kickoff

![Visibility: Intentionally Public](https://flat.badgen.net/badge/Visibility/Intentionally%20Public/f2a)
![Protected Data: None](https://flat.badgen.net/badge/Protected%20Data/None/f96854)

Composer and initial configuration starting point for GT College of Sciences Drupal projects, based on minimal or no installation profile and then mixing in recipes.

## Drupal Recipes

[Drupal Recipes](https://www.drupal.org/docs/extending-drupal/drupal-recipes) are a way to define a set of modules and configuration that can be installed with a single command. This project calls recipes for installing common configuration and modules to allow lifecycle adaptation without being pinned to a specific base profile.

[Applying Recipes](https://www.drupal.org/project/distributions_recipes)

## Will call recipes and modules

- gtsciences/gt-standard -- Recipe for common core modules and configuration
- gtsciences/gt-sso -- Recipe for GT SSO logins
- gtsciences/gt-seo-social -- Recipe for SEO and social linking utilities
- gtsciences/gt_theme -- modified from IC's GT theme
- gtsciences/gt_tools -- to help with compatibility with campus builds
- gtsciences/hg-reader -- Mercury Syndication module using core media

## Useful References

[Composer Patches](https://github.com/cweagans/composer-patches)
[Composer Recipe Unpack](https://gitlab.ewdev.ca/yonas.legesse/drupal-recipe-unpack)
[Recipes take inpute in 11.1](https://www.drupal.org/node/3470507)

[![Coding Style: Drupal](https://flat.badgen.net/badge/code%20style/Drupal/009cde?icon=php)](https://www.drupal.org/docs/develop/standards/php/php-coding-standards)
[![GitHub Super-Linter](https://github.com/gatech-arcs/drupal-kickoff/workflows/Lint%20Code%20Base/badge.svg)](https://github.com/marketplace/actions/super-linter)
![Dependabot Status](https://flat.badgen.net/github/dependabot/ubuntu/yaru)
-------------------------------
## Local Development with Docker

This project includes a Docker setup for a consistent local development environment. It uses `docker-compose` to run three services:

* **`app`**: The PHP-FPM container running Drupal 11.
* **`nginx`**: The web server that receives requests and passes them to the `app` service.
* **`db`**: A MariaDB database service to store all site data.

### 1. Prerequisites

* [Docker Desktop](https://www.docker.com/products/docker-desktop/) (or Docker Engine + Docker Compose) must be installed and running.

### 2. First-Time Setup

Before you can build the environment, you must create a local `.env` file for your environment variables.

1.  **Create the `.env` file:**
    ```sh
    cp .env.example .env
    ```
    *(Note: If you don't have a `.env.example`, just create a new file named `.env` and paste the content below into it.)*

2.  **Edit your `.env` file:**
    The file should contain the following. You can change these values, but the defaults are recommended.

    ```bash
    # Database Settings
    DB_NAME=drupal
    DB_USER=drupal
    DB_PASSWORD=drupal
    DB_ROOT_PASSWORD=supersecret
    DB_HOST=db
    DB_PORT=3306
    ```

### 3. Running the Site

1.  **Build and Start the Containers:**
    This will build the `app` image from the `Dockerfile` and start all services in the background (`-d`).

    ```sh
    docker-compose up -d --build
    ```

2.  **Install Composer Dependencies:**
    This command runs `composer install` *inside* the `app` container.

    ```sh
    docker-compose exec app composer install
    ```

3.  **Install Drupal:**
    If this is a new site, you can run the standard installer using Drush.

    ```sh
    docker-compose exec app drush si --db-url=mysql://drupal:drupal@db/drupal --account-pass=admin
    ```

    *If you have an existing database*, import your database SQL file using your preferred SQL tool. Connect to the host `127.0.0.1` and port `3306`.

Your site is now running at: **[http://localhost:8080](http://localhost:8080)**

---

### Common Docker Commands

* **Start containers** (in background):
    ```sh
    docker-compose up -d
    ```

* **Stop containers:**
    ```sh
    docker-compose down
    ```

* **Run Drush commands:**
    Prefix any drush command with `docker-compose exec app`.

    ```sh
    docker-compose exec app drush cr  # Clear cache
    docker-compose exec app drush cim # Import config
    ```

* **View logs:**
    ```sh
    docker-compose logs -f app # View app/Drupal logs
    docker-compose logs -f    # View all logs
    ```
