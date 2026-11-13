<!-- Adding data -->
<section
  id="adding-data"
  aria-labelledby="adding-data"
  data-item="Adding data to events_db"
>
  <h2><a href="#adding-data">Adding data to `events_db`</a></h2>

Now that you have checked that your Python scripts can connect to your local PostgreSQL database, you are ready to add some data. To start with, you can add one simple table with a single row, and check that Python can read the data that it has added.

1. At the root of your `events_assistant` project folder, create a new file called `add_data.py`
2. Give it the following script:

::: {.os .win}
```python
import psycopg2

# --- Connection settings ---
conn = psycopg2.connect(
    dbname="events_db",
    user="postgres",
    password="<YOUR REGISTERED PASSWORD>",
    host="localhost",
    port="5432"
)
cur = conn.cursor()

# --- Create the table ---
cur.execute("""
    CREATE TABLE IF NOT EXISTS meeting (
        id SERIAL PRIMARY KEY,
        title TEXT NOT NULL,
        location TEXT NOT NULL,
        start_time TIMESTAMP NOT NULL
    )
""")
conn.commit()
print("Table 'meeting' is ready.")

# --- Ensure the table is empty before adding a row ---
cur.execute("TRUNCATE meeting RESTART IDENTITY;")
conn.commit()

# --- Insert one row ---
cur.execute(
    """
    INSERT INTO meeting (title, location, start_time)
    VALUES (%s, %s, %s)
    RETURNING id
    """,
    ('Python for AI', 'Maker Lab', '2026-11-20 19:00:00')
)
new_id = cur.fetchone()[0]
conn.commit()
print(f"Inserted meeting with id={new_id}.")

# --- Query all rows ---
cur.execute("SELECT id, title, location, start_time FROM meeting")
rows = cur.fetchall()
print("\nAll meetings:")
for row in rows:
    print(row)

# --- Clean up ---
cur.close()
conn.close()
```

:::


::: {.os .mac}
```python
import psycopg2

# --- Connection settings ---
conn = psycopg2.connect(
    dbname="events_db",
    user="<YOUR_USER_NAME>",
    host="localhost",
    port="5432"
)
cur = conn.cursor()

# --- Create the table ---
cur.execute("""
    CREATE TABLE IF NOT EXISTS meeting (
        id SERIAL PRIMARY KEY,
        title TEXT NOT NULL,
        location TEXT NOT NULL,
        start_time TIMESTAMP NOT NULL
    )
""")
conn.commit()
print("Table 'meeting' is ready.")

# --- Ensure the table is empty before adding a row ---
cur.execute("TRUNCATE meeting RESTART IDENTITY;")
conn.commit()

# --- Insert one row ---
cur.execute(
    """
    INSERT INTO meeting (title, location, start_time)
    VALUES (%s, %s, %s)
    RETURNING id
    """,
    ('Python for AI', 'Maker Lab', '2026-11-20 19:00:00')
)
new_id = cur.fetchone()[0]
conn.commit()
print(f"Inserted meeting with id={new_id}.")

# --- Query all rows ---
cur.execute("SELECT id, title, location, start_time FROM meeting")
rows = cur.fetchall()
print("\nAll meetings:")
for row in rows:
    print(row)

# --- Clean up ---
cur.close()
conn.close()
```
:::

3. In a Terminal window open on your `event_assistant` project folder, run the command:
```bash-w
python add_data.py
```
If all goes well, each time you run the script, you should see the following output in your Terminal:
```bash-w
Table 'meeting' is ready.
Inserted meeting with id=1.

All meetings:
(1, 'Python for AI', 'Maker Lab', datetime.datetime(2026, 11, 20, 19, 0))
```

<details class="challenge" open>
<summary>Add multiple rows</summary>
Here's a challenge: Can you edit the `add_data.py` script so that you add multiple rows at once?

**NOTE: The next section assumes that you have indeed inserted 3 or more rows into the `events_db` database, using one of the solutions below, or your own solution.**

<details class="solution">
<summary>Hints</summary>

Here are three possible solutions. Note that the code following the command...

```python-w
cur.execute...
```

... depend on what exactly was executed.

#### Solution 1
```python-#28
# --- Insert multiple rows ---
rows = [
    ('Python for AI', 'Maker Lab', '2026-11-20 19:00:00'),
    ('LLMs &amp; SQL', 'City Library', '2026-11-21 18:30:00'),
    ('Database design', 'Café Olé', '2026-11-22 17:00:00')
]

cur.executemany("""
    INSERT INTO meeting (title, location, start_time)
    VALUES (%s, %s, %s)
    """,
    rows
)
conn.commit()
print(f"Inserted {cur.rowcount} meetings.")
```

#### Solution 2
```python-#28
# --- Insert multiple rows ---
cur.execute("""
    INSERT INTO meeting (title, location, start_time)
    VALUES
        ('Python for AI', 'Maker Lab', '2026-11-20 19:00:00'),
        ('LLMs &amp; SQL', 'City Library', '2026-11-21 18:30:00'),
        ('Database design', 'Café Olé', '2026-11-22 17:00:00')
""")
conn.commit()
print(f"Inserted {cur.rowcount} meetings.")
```

#### Solution 3
```python-#28
# --- Insert multiple rows ---
cur.execute("""
    INSERT INTO meeting (title, location, start_time)
    VALUES
        (%s, %s, %s),
        (%s, %s, %s),
        (%s, %s, %s)
    RETURNING id
    """,
    (
        "Python for AI", "Maker Lab", "2026-11-20 19:00:00",
        "LLMs &amp; SQL", "City Library", "2026-11-21 18:30:00",
        "Database design", "Café Olé", "2026-11-22 17:00:00",
    )
)
new_ids = [row[0] for row in cur.fetchall()]
conn.commit()
print(f"Inserted meetings with ids={new_ids}.")
```
</details>
</details>


<details class="pivot" open>
<summary>What You've Just Learned</summary>
In this section, you saw how to:

1. Add a new table to you database, if it does not exist yet
2. Empty the table, so that it contains no rows, and so that new `id`s start at 1
3. Insert a single row into the table
4. Insert multiple rows into the table
5. Create a query to read all rows in the table

Now that your `events_db` database contains some data, you can modify the `main.py` server and the `index.html` file that it serves up, so that you can display data from the database in a browser client.

</details>

</section>