#rails app with Postgres db

#Install PostgreSQL with this yum command as a root user:
```
$ sudo su -
$ yum -y install postgresql-server postgresql-devel postgresql-contrib
```

Initialize the database server with the initdb command:
```
$ postgresql-setup initdb
```
By default, PostgreSQL on CentOS server does not allow password based authentication. We need to edit the PostgreSQL configuration for the RoR installation to allow logins with a password.

Edit pg_hba.conf with vim:
```
$ vim /var/lib/pgsql/data/pg_hba.conf
```
Then replace "ident" with "md5" under IPv4 local connections and IPv6 local connections, so they look like this :
```
# IPv4 local connections:
host    all             all             127.0.0.1/32            md5
# IPv6 local connections:
host    all             all             ::1/128                 md5
```

Save the file, then start the "postgresql" service:
```
$ systemctl start postgresql
```

Postgresql is running on localhost ip address with port 5432, you can check that with the command below:
```
$ netstat -plntu
$ systemctl status postgresql
```

When the installation is done, become the postgres user on the shell with su and then access the postgresql shell (psql):
```
$ su - postgres
```

Enter the PostgreSQL console:
```
$ psql
```
```
postgres=#
```

Set a new password for the postgres user with the command below:
```
postgres-#  \password postgres
postgres-# Enter new password: test123
```

Next, create a new role named 'rails-dev' (for example) for the rails development with this command:
```
postgres-# create role rails_dev with createdb login password 'test';
```

Check the new role, you will see new role is created:
```
postgres-# \du
```

Exit the PostgreSQL console:
```
postgres-# \q
```

Create a new application with PostgreSQL as the default database:
```
$ rails new myapp -d postgresql
```
Edit the database.yml file in the config directory:
```
$ cd myapp/
$ vim config/database.yml
```

set the role we've created, passsword, host and port:
```
  username: rails_dev
  password: test
  host: localhost
  port: 5432
  ```

For the test section and add the new configuration below:
```
  database: myapp_test
  host: localhost
  port: 5432
  username: rails_dev
  password: test
  ```

Generate the database with the rails command:
```
$ rails db:setup
$ rails db:migrate
```

Then start the rails server:
```
rails s
```

To connect to database again:
```
psql DBNAME USERNAME
e.g. psql myrailsapp_development postgres
```

Now in Psql you could run commands such as:
```
\? #list all the commands
\l #list databases
\conninfo #display information about current connection
```
