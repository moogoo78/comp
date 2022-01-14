*****************
PostgreSQL
*****************

<<<<<<< HEAD
How-to
==============
=======

How-to
--------------------

daterange

overlap

.. code-block:: sql

    SELECT * FROM my_table WHERE DATERANGE(my_start::date , my_end::date, '[)') && DATERANGE( date '2018-01-01', date '2018-01-31', '[)')


Install
-------------------

mac php (with-postgresql), 為了用 adminer.php:
>>>>>>> m

Data Dump/Import
-------------------------------------

.. code-block:: bash

   # dump
   $ pg_dump -U USERNAME DBNAME > dbexport.pgsql
   $ # PGPASSWORD="mypassword" pg_dump -U myusername dbname 密碼 > output.sql$ pg_dump -U postgres -f /tmp/dump.sql.gz --compress=5 --no-owner dbname

   ## import
   $ psql -f backup.sql dbname dbuser

   # specific table
   pg_dump -U xxx public.TABLE_NAME DATABASE_NAME > out.sql
   pg_dump -U xxx -d DB_NAME -t TABLE_NAME > out.sql

   ## export csv
   $ psql -U user -d db_name -c "Copy (Select * From foo_table LIMIT 10) To STDOUT With CSV HEADER DELIMITER ',';" > foo_data.csv


sequence 亂掉
-----------------------
restore data 會發生 => duplicate key error...

.. code-block::

   SELECT setval('my_sequence_name', (SELECT max(id) FROM my_table));

truncate & auto increment 從頭開始:
加 RESTART IDENTITY 才能從頭開始

.. code-block::

   TRUNCATE table_name RESTART IDENTITY;


或加 CASCADE


Slow Query
---------------

.. code-block:: bash

   # 1. find config file
   $ psql -U postgres -c 'SHOW config_file'
   # 2. edit conf
   # default
   #log_min_duration_statement = -1
   log_min_duration_statement = 1000 # (log all queries executing more than 1 second)


可能的 log paths::

  /var/lib/pgsql/PG_VERSION/data/log/
  /var/log/postgresql/
  /var/lib/postgresql/PG_VERSION/main/pg_log


restart postgres (not "service")

.. code-block:: bash

   su - postgres -c "PGDATA=$PGDATA {path/to/}pg_ctl -w restart"


建立在當前使用者
----------------------

.. code-block::

   postgres=# CREATE DATABASE mydbname ;
   postgres=# CREATE DATABASE mydbname  OWNER myusername ;
   postgres=# GRANT ALL PRIVILEGES ON DATABASE mydbname to myusername ;
   ALTER ROLE myusername WITH superuser;


commands
------------------

.. code-block:: text

   # ALTER DATABASE name RENAME TO newname

   # mysql: SHOW DATABASES
   # pgres: \l
   # mysql: SHOW TABLES
   # pgres: \d
   # mysql: USE mydbname
   # pgres: \c  mydbname # \connect

   # mysql: SHOW COLUMNS
   # pgres: \d table

   # mysql: DESCRIBE TABLE
   # pgres: \d+ table ( \dt)

   # exit: \q

   SELECT version();
   show data_directory;


Install
=======================

mac php (with-postgresql), 為了用 adminer.php
--------------------------------------------------

* `Install PostgreSQL PHP extensions on Mac OS X <https://gist.github.com/doole/8651341/59f9ccb85e3ae48861b4f892b342e08efff9236e/>`__
* `How To Install and Use PostgreSQL 9.4 on Debian 8 | DigitalOcean <https://www.digitalocean.com/community/tutorials/how-to-install-and-use-postgresql-9-4-on-debian-8>`__

.. code-block:: bash

   ## package
   # debian
   $ apt-get install postgresql postgresql-client postgresql-server-dev-9.4 # or postgresql-server-dev-all

   # OSX
   $ brew install postgresql


設定新密碼:

.. code-block:: bash

   $ sudo -u postgres psql postgres
   postgres=# \password postgres


以後要用 psql 時 (不用 sudo -u postgres), 要改:

.. code-block::

   $ sudo vim /etc/postgresql/9.x/main/pg_hba.conf
   # 找到 local all postgres peer # 把 peer 改 md5
   $ sudo service postgresql restart

.. code-block:: bash

   ## Debian Jessie
   # first time
   # createuser myuser

   ## OSX
   # first time
   $ initdb /usr/local/var/postgres -E utf8
   # service (daemon)
   $ brew services start postgresql


`PostgreSql - Debian Wiki <https://wiki.debian.org/PostgreSql#Installation>`__

Reference
----------------------

Security
^^^^^^^^^^^^

 - `“FATAL: Ident authentication failed”, or how cool ideas get bad usage schemas – select * from depesz; <https://www.depesz.com/2007/10/04/ident/>`__
