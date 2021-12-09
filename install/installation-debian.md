# Install self-hosted TimescaleDB on Debian-based systems
You can host TimescaleDB yourself, on your Debian or Ubuntu system. These
instructions use the `apt` package manager on these distributions:

*   Debian 9 Stretch
*   Debian 10 Buster
*   Debian 11 Bullseye
*   Ubuntu 18.04 LTS Bionic Beaver
*   Ubuntu 20.04 LTS Focal Fossa
*   Ubuntu 21.04 Hirsute Hippo

<highlight type="warning">
If you have already installed PostgreSQL using a method other than the `apt`
package manager, you could encounter errors following these instructions. It is
safest to remove any existing PostgreSQL installations before you begin. If you
want to keep your current PostgreSQL installation, do not install TimescaleDB
TimescaleDB using this method.
[Install from source](/how-to-guides/install-timescaledb/installation-source/)
instead.
</highlight>

<procedure>

### Installing self-hosted TimescaleDB on Debian-based systems
1.  At the command prompt, as root, add the PostgreSQL third party repository
    to get the latest PostgreSQL packages:
    ```bash
    apt install gnupg postgresql-common
    ```
1.  Run the PostgreSQL repository setup script:
    ```bash
    /usr/share/postgresql-common/pgdg/apt.postgresql.org.sh
    ```
1.  Add the TimescaleDB third party repository:
    <terminal>

    <tab label='Debian'>

    ```bash
    sh -c "echo 'deb [signed-by=/usr/share/keyrings/timescale.keyring] https://packagecloud.io/timescale/timescaledb/debian/ $(lsb_release -c -s) main' > /etc/apt/sources.list.d/timescaledb.list"
    ```

    </tab>

    <tab label="Ubuntu">

    ```bash
    sh -c "echo 'deb [signed-by=/usr/share/keyrings/timescale.keyring] https://packagecloud.io/timescale/timescaledb/ubuntu/ $(lsb_release -c -s) main' > /etc/apt/sources.list.d/timescaledb.list"
    ```

    </tab>

    </terminal>
1.  Get the TimescaleDB GPG signing packages, and unpack them:
    ```bash
    wget --quiet -O - https://packagecloud.io/timescale/timescaledb/gpgkey | gpg --dearmor -o /usr/share/keyrings/timescale.keyring
    ```
1.  Update your local repository list:
    ```bash
    apt update
    ```
1.  Install TimescaleDB:
    ```bash
    apt install timescaledb-2-postgresql-14
    ```

</procedure>

When you have completed the installation, you need to configure your database so
that you can use it. The easiest way to do this is to run the `timescaledb-tune`
script, which is included with the `timescaledb-tools` package. For more
information, see the [configuration][config] section.

## Set up the TimeascaleDB extension
When you have PostgreSQL and TimescaleDB installed, you can connect to it from
your local system using the `psql` command-line utility. This is the same tool
you might have used to connect to PostgreSQL before, but if you haven't
installed it yet, check out our [installing psql][install-psql] section.

<procedure>

### Setting up the TimescaleDB extension
1.  On your local system, at the command prompt, connect to the PostgreSQL
    instance as the `postgres` superuser:
    ```bash
    psql -U postgres -h localhost
    ```
    If your connection is successful, you'll see a message like this, followed
    by the `psql` prompt:
    ```
    psql (13.3, server 12.8 (Ubuntu 12.8-1.pgdg21.04+1))
    SSL connection (protocol: TLSv1.3, cipher: TLS_AES_256_GCM_SHA384, bits: 256, compression: off)
    Type "help" for help.
    tsdb=>
    ```
1.  At the `psql` prompt, create an empty database. Our database is
    called `example`:
    ```sql
    CREATE database example;
    ```
1.  Connect to the database you created:
    ```sql
    \c example
    ```
1.  Add the TimescaleDB extension:
    ```sql
    CREATE EXTENSION IF NOT EXISTS timescaledb;
    ```
1.  You can now connect to your database using this command:
    ```bash
    psql -U postgres -h localhost -d example
    ```

</procedure>

You can check that the TimescaleDB extension is installed by using the `\dx`
command at the `psql` prompt. It looks like this:
```sql
tsdb=> \dx
List of installed extensions
-[ RECORD 1 ]------------------------------------------------------------------
Name        | pg_stat_statements
Version     | 1.7
Schema      | public
Description | track execution statistics of all SQL statements executed
-[ RECORD 2 ]------------------------------------------------------------------
Name        | plpgsql
Version     | 1.0
Schema      | pg_catalog
Description | PL/pgSQL procedural language
-[ RECORD 3 ]------------------------------------------------------------------
Name        | timescaledb
Version     | 2.5.1
Schema      | public
Description | Enables scalable inserts and complex queries for time-series data
-[ RECORD 4 ]------------------------------------------------------------------
Name        | timescaledb_toolkit
Version     | 1.3.1
Schema      | public
Description | timescaledb_toolkit

tsdb=>
```

## Where to next
Now that you have your first TimescaleDB database up and running, you can check
out the [TimescaleDB][tsdb-docs] section in our documentation, and find out what
you can do with it.

If you want to work through some tutorials to help you get up and running with
TimescaleDB and time-series data, check out our [tutorials][tutorials] section.

You can always [contact us][contact] if you need help working something out, or
if you want to have a chat.


[contact]: https://www.timescale.com/contact
[install-psql]: /how-to-guides/connecting/psql/
[tsdb-docs]: timeascaledb/:currentVersion:/index/
[tutorials]: /timescaledb/:currentVersion:/tutorials/
[config]: /how-to-guides/configuration/