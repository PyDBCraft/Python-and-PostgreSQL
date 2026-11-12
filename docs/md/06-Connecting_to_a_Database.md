<!-- Connecting to a Database -->
<section
  id="connecting-to-a-database"
  aria-labelledby="connecting-to-a-database"
  data-item="Connecting a Database"
>
  <h2><a href="#connecting-to-a-database">Connecting to a Database</a></h2>

Before connecting your `main.py` server to a database, you can create a separate Python script to see how Python can connect to a PostgreSQL database. Isolating a new issue clears the way to understanding it better.

### Ensure PostgreSQL is installed
In <button class="os win anon" data-name="anchor-postgresql">section 2: Work Environment</button><button class="os mac anon" data-name="anchor-postgremac">section 2: Work Environment</button>  you should have installed PostgreSQL locally on your development computer. If you have not done that, please do it now, then return here.

### Connecting to PostgreSQL
Python needs a driver to interact with PostgreSQL. The package you need is `psycopg2-binary`. It will be installed as `psycopg`.

<details class="trouble" open>
<summary>No precompiled binary?</summary>
`psycopg2-binary` is a pre-compiled version of the driver that does not require a C compiler or PostgreSQL development tools on most common systems (standard Windows, macOS, and Linux).

However, binary packages are produced on a best-effort basis, and some platforms—such as Windows ARM64 (used by Parallels on Apple Silicon) or very new Python versions—may not have a wheel available yet.

If `pip install psycopg2-binary` fails with an error mentioning `pg_config` or a missing compiler, this means that your platform does not yet have a pre-built binary, and you will need either to install the PostgreSQL development tools or to use the newer psycopg driver instead.

</details>

1. In a Terminal window open on your `event_assistant` project folder, run the command:
```bash-w
pip install psycopg2-binary
```

This gets the Python package manager, `pip`, to download the files needed for the `psycopg2` driver for Python, and store them at <span class="os win anon">`.venv\Lib\site-packages\`</span><span class="os mac anon">`.venv/lib/python3.15/site-packages/`</span>.

2. Use the command line to tell PostgreSQL to create a new database for you:
 
```bash-w
createdb events_db
```

3. At the root of your `event_assistant` project folder, create a new script called `connect_db.py`
4. Give it the following script:

::: {.os .win}
```python
import psycopg2

conn = psycopg2.connect(
    dbname="events_db",
    user="postgres",
    password="<registered password>",
    host="localhost",
    port="5432"
)
cur = conn.cursor()
cur.execute("SELECT 1")
print(cur.fetchone())
cur.close()
conn.close()
```

Notice that `postgres` is the standard value for `user` when testing on Windows. The password is the one that you set when you installed PostgreSQL.
:::

::: {.os .mac}
```python
import psycopg2

conn = psycopg2.connect(
    dbname="events_db",
    user="<YOUR_USER_NAME>",
    host="localhost",
    port="5432"
)
cur = conn.cursor()
cur.execute("SELECT 1")
print(cur.fetchone())
cur.close()
conn.close()
```

Replace `"<YOUR_USER_NAME>"` with your Mac user name (in my case: "james"). Homebrew's PostgreSQL uses your MacOS username and no password for local connections. However, if you installed PostgreSQL another way, you may need to provide a password.
:::

5. In a Terminal window open on your `event_assistant` project folder, run the command:
```bash-w
python connect_db.py
```
If all goes well, you should see the following output in your Terminal:
```bash-w
(1,)
```

### What Just Happened?

This script simply proves that Python can connect to a PostgreSQL database, by performing the simplest possible sequence of actions.

Here's a line-by-line description of what the code in `connect_db.py` does.

```python-#1
import psycopg2
```

Reads in the `psycopg2` that you installed with `pip` just now, and makes it available to the script.

```python-#3
conn = psycopg2.connect(
    dbname="events_db",
    user="<registered user name>",
    host="localhost",
    port="5432"
)
```

Tells the `psycopg2` to connect to the `events_db` database that you have just created on the PostgreSQL app running locally on your computer. 

```python-#9
cur = conn.cursor()
cur.execute("SELECT 1")
```
Tells this `events_db` database to perform a `SELECT` operation with the number `1` that the script provides. This is an operation that will run even if the database contains no tables yet. If it succeeds, it proves that the connection to `events_db` was successful.

```python-#11
print(cur.fetchone())
```
This prints the result of the `"SELECT 1"` operation. The result is a _tuple_ with only one element: the number `1` that was to be `SELECT`ed.

```python-#13
cur.close()
conn.close()
```

This cleans up and closes down the connection, so the execution of the script is completed.

<details class="pivot" open>
<summary>What You've Just Learned</summary>
In this section, you have verified that Python can connect to PostgreSQL, run a query, and read the result.

In the next section, you will create a table in the database and insert some data that you can later retrieve with a query.

</details>

</section>