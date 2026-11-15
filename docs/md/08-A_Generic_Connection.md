<!-- A Generic Connection -->
<section
  id="a-generic-connection"
  aria-labelledby="a-generic-connection"
  data-item="A Generic Connection"
>
  <h2><a href="#a-generic-connection">A Generic Connection</a></h2>
  

  Did you notice that you used the same code in both of the last two scripts:

::: {.os .win}
```python-w
conn = psycopg2.connect(
    dbname="events_db",
    user="postgres",
    password="<YOUR REGISTERED PASSWORD>",
    host="localhost",
    port="5432"
)
```
:::

::: {.os .mac}
```python-w
conn = psycopg2.connect(
    dbname="events_db",
    user="<YOUR_USER_NAME>",
    host="localhost",
    port="5432"
)
```
:::

This means that, as you were creating the scripts, you had to think. You had to replace <span class="os win anon">`<YOUR REGISTERED PASSWORD>`</span><span class="os mac anon">`<YOUR_USER_NAME>`</span> with the correct value for your system.

Here's a way to simplify the process, and to ensure at the same time that your private data remains secret.

### Creating a `.env` file

1. At the root of your `event_assistant` project directory, create a file named `.env`
2. Give it the following text...

::: {.os .win}
```tex
DB_NAME=events_db
DB_USER=postrgres
DB_password=&lt;YOUR REGISTERED PASSWORD&gt;
DB_HOST=localhost
DB_PORT=5432
```
... except that instead of `<YOUR REGISTERED PASSWORD>`, you write the actual password that you set when you installed PostgreSQL.
:::

::: {.os .mac}
```tex
DB_NAME=events_db
DB_USER=&lt;YOUR_USER_NAME&gt;
DB_PASSWORD=
DB_HOST=localhost
DB_PORT=5432
```
... except that instead of `<YOUR_USER_NAME>`, you write your actual Mac user name. Note that `DB_PASSWORD` is given, but it is assigned no value.
:::

### A Hidden File

Because the name of the `.env` file starts with a dot (`.`), your operating system will keep it hidden. When you copy the contents of the folder, `.env` will not be copied.

If you are using Git, you should ensure that your (hidden) `.gitignore` file contains a line for `.env`. This guarantees that Git will not push the `.env` file to a remote repository, where others might find it.

### Creating a `config.py` Script

1. At the root of your `event_assistant` project directory, create a file named `config.py`
2. Give it the following text...

```python
from pydantic_settings import BaseSettings, SettingsConfigDict

class Settings(BaseSettings):
    db_name: str
    db_user: str
    db_password: str
    db_host: str = "localhost"
    db_port: int = 5432

    model_config = SettingsConfigDict(env_file=".env")

settings = Settings()
```

When you run this script, it will read in the data from your `.env` file and create a dict called `settings` which contains the data from `.env`. Note that it will set values for `db_host` and `db_port` by default, so even if these values are missing in your `.env`, they will be present in `settings`.

### Importing `settings` from the `config.py` Script

Edit your `connect_db.py` and `add_data.py` scripts as follows:

#### `connect_db.py`
```python
<i>import psycopg2</i>
<b>from config import settings

conn = psycopg2.connect(
    dbname=settings.db_name,
    user=settings.db_user,
    password=settings.db_password,
    host=settings.db_host,
    port=settings.db_port
)</b><i>
cur = conn.cursor()
cur.execute("SELECT 1")
print(cur.fetchone())
cur.close()
conn.close()</i>
```

#### `add_data.py`
```python
<i>import psycopg2</i>
<b>from config import settings

# --- Connect to database ---
conn = psycopg2.connect(
    dbname=settings.db_name,
    user=settings.db_user,
    password=settings.db_password,
    host=settings.db_host,
    port=settings.db_port
)</b>
<i>cur = conn.cursor()

# --- Ensure that a "meetings" table exists
cur.execute("""</i>
```
```python-w
     ... and so on
```


OK. So this actually makes your scripts _longer_ than the originals, but now they are generic. If you need to change the connection details for your project, you can change them in one place, and the changes will apply everywhere, and this works for both Windows and MacOS.

<details class="pivot" open>
<summary>What You've Just Learned</summary>
Hard-coding data makes your code fragile, especially if the same data needs to be used in different places in your code. In most cases, it is good to apply the DRY principle: Don't Repeat Yourself.

Creating the `.env` and `config.py` files means that your project is more robust.

In the next section, you can use `from config import settings` and the new arguments for `psycopg2.connect()` call to avoid having to hard-code your personal data again.

</details>

</section>