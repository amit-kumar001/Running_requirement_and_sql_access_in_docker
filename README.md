# Running_requirement_and_sql_access_in_docker
### Running the Containers and Modifying Environment Settings.
<ol>
<li>Now make a copy of the .env.example file that Laravel includes by default and name the copy .env, which is the file Laravel expects to define its environment:</li>

<strong>cp .env.example .env</strong></br>

<li>Run docker-compose up for the first time, it will download all of the necessary Docker images. Once the images are downloaded and stored in your local machine, Compose will create your containers.</li> 

<strong>docker-compose up</strong></br>

open another terminal and check running containers.</br>

<strong>docker ps </strong></br>

CONTAINER ID in this output is a unique identifier for each container, while NAMES lists the service name associated with each. You can use both of these identifiers to access the containers.</br>

root@a7627c4d11b2:/#
<li>now modify the <strong>.env file</strong></li>

```

DB_CONNECTION=mysql
DB_HOST=db
DB_PORT=3306
DB_DATABASE=laravel
DB_USERNAME=laraveluser
DB_PASSWORD=passcode

```

<li>NOW set the application key for the Laravel application with the php artisan key:generate command. This command will generate a key and copy it to your .env file, ensuring that your user sessions and encrypted data remain secure:</li>

<strong>docker-compose exec app php artisan key:generate</strong></br>

<li>Now, you have the environment settings required to run your application. To cache these settings into a file, which will boost your application's load speed, run:</li>

<strong>docker-compose exec app php artisan config:cache</strong></br>

configuration settings will be loaded into /var/www/bootstrap/cache/config.php on the container.</br>

<li>Now copy the webserver port and run it.</li>
</ol>


### Creating a User for MySQL
<ol>
The default MySQL installation only creates the root administrative account, which has unlimited privileges on the database server. In general, it's better to avoid using the root administrative account when interacting with the database. Instead, let's create a dedicated database user for our application's Laravel database.<br>

<strong>docker-compose exec db bash</strong></br>

Inside the container, log into the MySQL root administrative account:</br>

<strong>root@a7627c4d11b2:/# mysql -u root -p</strong></br>

Run the show databases command to check for existing databases:</br>

<strong>show databases;</strong></br>
```

+--------------------+
| Database           |
+--------------------+
| information_schema |
| laravel            |
| mysql              |
| performance_schema |
| sys                |
+--------------------+

```
</br>

create the user account that will be allowed to access this database. Our username will be laraveluser, though you can replace this with another name if you'd prefer. Just be sure that your username and password here match the details you set in your .env file </br>

<strong>GRANT ALL ON laravel.* TO 'laraveluser'@'%' IDENTIFIED BY '123';</strong></br>

Flush the privileges to notify the MySQL server of the changes:</br>

<strong>FLUSH PRIVILEGES;</strong></br>

now exit</br>

<strong>EXIT;</strong></br>
<strong>root@a7627c4d11b2:/# exit</strong></br>

</ol>



