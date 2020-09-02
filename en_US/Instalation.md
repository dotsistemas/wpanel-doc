# Installation

Here you will learn how to install Wpanel in different ways, the main ones being installation in a development environment and a production environment.

# Summary

1. [Quick start (PHP + SQLite)](#quick-start)
2. [Creating a development environment (Linux only)](#development-environment)
    1. [Clone the repository](#clone-the-repository)
    2. [Install the dependencies](#install-the-dependencies)
    3. [Development server](#development-server)
    4. [Database](#database)
    5. [Run Wpanel the Setup](#run-the-wpanel-setup)
3. [Publishing in production](#publishing-in-production)
    1. [Sending files to the server](#sending-files-to-the-server)
    2. [Production database](#production-database)
    3. [Switch Wpanel to production mode](#switch-wpanel-to-production-mode)
    4. [Run Wpanel Setup in production](#run-wpanel-setup-in-production)

# Quick Start

The easiest way to have Wpanel CMS ready to start developing your project is through [Composer](https://getcomposer.org), with the SQLite database, using the built-in PHP server. To do this, follow the steps below.

- Use composer to create your project:
```sh
composer create-project "wpanel/wpanel4-cms" Blog
```

- Access the Blog folder created in the previous step and run the start script: 
```sh
cd Blog
composer run dev
```
- Run the installation of Wpanel with accessing http://localhost:8080/index.php/setup
- Fill in the form with the data for the administrator (ROOT)
- Access the admin using the data provided in the previous step
- Okay, Wpanel is already running at http://localhost:8080

# Development environment

Follow the steps below to set up your development environment. You can use the built-in PHP server or use a native server. You can also use [Docker](https://www.docker.com/).

By default, Wpanel comes with the SQLite database configured, this can be changed without any difficulty. The specific steps for configuring the MySQL database will be described below.

## Clone the repository

Run the command below to clone the Wpanel repository:

```sh
git clone https://github.com/wpanel/wpanel4-cms.git yourproject
```

Alternatively you can download the project [in this link](https://github.com/wpanel/wpanel4-cms/archive/master.zip).


## Install the dependencies

Wpanel uses the Composer dependency manager, if you have not installed it [check here](https://getcomposer.org) how to install it in your development environment.

Then, execute the command below so that everything is in place.

```sh
composer install
```

## Development Server

You will need to have a local server with PHP 7 configured to run Wpanel. The simplest way to do this is using the server built into PHP 7, to do this just run the following command:

```sh
composer run dev
```
So the composer will run the embedded server pointing to the public folder.

Alternatively, you can run an Apache or NGINX server with PHP locally or another tool like XAMMP/LAMP and use several advanced features, such as Virtualhosts that will not be covered at this time.

**Note:** If you choose to use a native server or another solution, make sure the server is pointing to **public/index.php**

## Database
_*These steps are exclusive to MySQL, if you want to use SQLIte, proceed to the next step._

- Create a database on your local MySQL using PHPMyAdmin for example or another tool of your choice.
- Configure the database connection credentials in the file ```/public/config/config.php``` in the array **$db['development']** as shown below:

```php
$db['development'] = array(
    'dsn'	=> '',
    'hostname' => 'localhost',    // Hostname
    'username' => 'root',         // Username
    'password' => 'root',         // Password
    'database' => 'wpanel4-cms',  // DB Name
    'dbdriver' => 'mysqli',       // DB Driver
    'dbprefix' => '',
    'pconnect' => TRUE,
    'db_debug' => (ENVIRONMENT !== 'production'),
    'cache_on' => FALSE,
    'cachedir' => '',
    'char_set' => 'utf8',
    'dbcollat' => 'utf8_general_ci',
    'swap_pre' => '',
    'encrypt' => FALSE,
    'compress' => FALSE,
    'stricton' => FALSE,
    'failover' => array(),
    'save_queries' => TRUE
);
```
For more information on database connection settings [read the CodeIgniter documentation here](https://codeigniter.com/userguide3/database/configuration.html).

## Run the Wpanel Setup

The setup process consists of creating the table structure in the database using [migrations](https://codeigniter.com/userguide3/libraries/migration.html), and creating a ROOT user in the control panel. This user will have Developer privileges, which will be covered later in advanced topics.

Access the link http://localhost:8080/index.php/setup or the address you put on your local server, eg http://yourvirtualhost/index.php/setup

![Setup](https://dotsistemas.com.br/wpanel/setup.png?raw=true)

Fill out the form with the user's access credentials and click next.

After this process, you will be directed to the Site Administration login.

![Login](https://dotsistemas.com.br/wpanel/login.png?raw=true)

Log in and welcome!

![Admin](https://dotsistemas.com.br/wpanel/admin.png?raw=true)

Click on the "View website" option on the top right bar to access the website.

![Home](https://dotsistemas.com.br/wpanel/home.png?raw=true)

# Publishing in production

Publishing your project in production is not much different from the previous steps, except that you need to pay attention to some details.

## Sending files to the server

You can upload the project files to your production server via FTP or using GIT, if you have access to the server terminal. Check with your provider for availability.

We know that some shared hosting providers use a specific public folder in their configuration which can be **public_html** or **www**, in which case you will need to rename the public Wpanel folder for your situation.

In the end, the file structure should look like this:

```sh
/path/userfolder
  |- /app
  |- /sys
  |- /public_html   <--- Public folder
        |- index.php
        |- .htaccess
```

_**Note**: Make sure that when accessing **yourdomain.com** the server is arriving at index.php_

## Production database
_* These steps are exclusive to MySQL, if you want to use SQLIte, proceed to the next step._

- Create a MySQL database on your server using PHPMyAdmin or another tool provided by your hosting.

- Configure the database connection credentials in the ```/public/config/config.php``` file in the **$db['production']** array as shown below:

```php
$db['production'] = array(
    'dsn'	=> '',
    'hostname' => 'localhost',    // Hostname
    'username' => 'root',         // Username
    'password' => 'root',         // Password
    'database' => 'wpanel4-cms',  // DB Name
    'dbdriver' => 'mysqli',       // DB Driver
    'dbprefix' => '',
    'pconnect' => TRUE,
    'db_debug' => (ENVIRONMENT !== 'production'),
    'cache_on' => FALSE,
    'cachedir' => '',
    'char_set' => 'utf8',
    'dbcollat' => 'utf8_general_ci',
    'swap_pre' => '',
    'encrypt' => FALSE,
    'compress' => FALSE,
    'stricton' => FALSE,
    'failover' => array(),
    'save_queries' => TRUE
);
```
For more information on database connection settings [read the CodeIgniter documentation here](https://codeigniter.com/userguide3/database/configuration.html).

## Switch Wpanel to production mode

By default, Wpanel is distributed in development mode, this activates some specific behaviors in the project, such as Profiler, error messages for debugging and other things. Activating Wpanel in production mode is important so that your project does not display certain information to the final user.

To enter production mode, change the ENVIRONMENT constant to 'production' in the ```/public/index.php``` file around line 76 as shown below.

From:
```php
define('ENVIRONMENT', isset($_SERVER['CI_ENV']) ? $_SERVER['CI_ENV'] : 'development');
```
To:
```php
define('ENVIRONMENT', isset($_SERVER['CI_ENV']) ? $_SERVER['CI_ENV'] : 'production');
```

## Run Wpanel Setup in production

The setup process follows the same principle as the process performed in the development environment.

Visit the link http://yoursite.com/index.php/setup

![Setup](https://dotsistemas.com.br/wpanel/setup.png?raw=true)

Fill out the form with the user's access credentials and click next.

After this process, you will be directed to the Site Administration login.

![Login](https://dotsistemas.com.br/wpanel/login.png?raw=true)

Log in and welcome!

![Admin](https://dotsistemas.com.br/wpanel/admin.png?raw=true)

Click on the "View website" option on the top right bar to access the website.

![Home](https://dotsistemas.com.br/wpanel/home.png?raw=true)

_**Note**: Alternatively it is possible for you to restore a backup of your local database on the production server, dispensing with this step, however it is highly recommended that you maintain your database using Migrations for several factors, such as versioning and rollback possibility in case of errors._
