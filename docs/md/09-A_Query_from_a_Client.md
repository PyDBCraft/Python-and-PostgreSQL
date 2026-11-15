<!-- A Query from a Client -->
<section
  id="a-query-from-a-client"
  aria-labelledby="a-query-from-a-client"
  data-item="A Query from a Client"
>
  <h2><a href="#a-query-from-a-client">A Query from a Client</a></h2>

Now that the `events_db` database in PostgreSQL contains a `meetings` table with some dummy data, you can update your project so that you can retrieve this data from your browser, display it.

### Updating the Client

First, you need to edit `index.html` and `assets/script.js` so that your web page makes a `POST` request to your server.

You can remove the previous button that requested the server time, as this has now served its purpose.

#### `public/index.html`
```html
<i><!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta
    name="viewport"
    content="width=device-width, initial-scale=1.0"
  >
  <link rel="stylesheet" href="assets/styles.css">
  <script defer src="assets/script.js"></script>
  <title>API Request</title>
</head>
<body>
  <h3>API Request</h3></i><b>
  <p id="query">SELECT * FROM meetings</p>
  <button
    type="button"
    id="treat_query"
  >
    Treat Query
  </button>
  <pre id="result">
    Click the button to get data from the database
  </pre></b><i>
</body>
</html></i>
```

#### `public/assets/script.js`

```javascript
// Read data from the server
const queryP = document.getElementById("query")
const button = document.getElementById("treat_query")
const result = document.getElementById("result")
const endPoint = "/treat_query"

button.addEventListener("click", treatQuery)

function treatQuery() {
  const query = queryP.textContent
  const options = {
    method: "POST",
    headers: {
      "Content-Type": "application/json"
    },
    body: JSON.stringify({ query })
  }
  fetch(endPoint, options)
  .then(response => response.json())
  .then(displayResults)
}

function displayResults(results) {
  result.textContent = JSON.stringify(results, null, '  ')
}
```

### Updating the server

Here are the changes you should make to `main.py`. Note that you are now importing `settings` from `config.py` and using this to create the connection with the database.

```python
<i>from fastapi import FastAPI
# Prepare to serve static files from a specific folder
from fastapi.staticfiles import StaticFiles
from pathlib import Path</i>
<b># Prepare to read an incoming post message
from pydantic import BaseModel
# Prepare to connect to PostgreSQL
import psycopg2
from config import settings</b><i>

# Create an instance of the FastAPI app
app = FastAPI()</i>

<b># Respond to a request for "/treat_query" with data from the database
class QueryRequest(BaseModel):
    query: str

@app.post("/treat_query")
async def treat_query(request: QueryRequest):
    conn = psycopg2.connect(<u>
        dbname=settings.db_name,
        user=settings.db_user,
        password=settings.db_password,
        host=settings.db_host,
        port=settings.db_port</u>
    )
    try:
        cur = conn.cursor()
        cur.execute(request.query) # !!! UNTRUSTED QUERY !!!
        rows = cur.fetchall()
        return [
           {"id": r[0], "title": r[1], "location": r[2], "start_time": r[3].isoformat()}
           for r in rows
        ]
    finally:
        cur.close()
        conn.close()</b><i>

# Ensure that a request for "/" serves up "public/index.html"
app.mount("/", StaticFiles(directory=Path("public"), html=True))</i>
```

<details class="note" open>
<summary>`BaseModel` from `pydantic`</summary>
`pydantic` is a validation library. Its BaseModel class ensures that a given datum has the expected format.

In this case...

```python-#15
class QueryRequest(BaseModel):
    query: str
```

... ensures that the data parsed from the incoming `request` ensures that the parsed data has a key named `query` whose value is a string. This means that the line...

```python-#29
execute(request.query) # !!! UNTRUSTED QUERY !!!
```

... will not fail due to the format of `request.query`. However, it **does not** guarantee that this line will fail because `request.query` is ill-formed or malicious SQL. Because you explicitly designed the `request.query` to be "SELECT * FROM meetings", these last two issues are not a problem here. For a production-ready server, you will need to deal with them.

</details>

### Testing Your Code

1. In your Terminal window, run

```bash-w
fastapi dev main.py
```
2. Ctrl-click or Cmd-click on the link to [http://127.0.0.1:8000](http://127.0.0.1:8000) which is logged to the console
3. Click on the Treat Query button.

![Retrieving data from the server](images/treatQuery.webp)

<details class="pivot" open>
<summary>What You've Just Learned</summary>
In this section, you have seen how to send a pre-defined SQL query to the server, and have the server respond with raw data from the rows it requested from the database.

This is fine in a proof-of-concept, but it is not secure. In particular, the server should never trust data sent from a client.

In the next section, you will see how to send a natural language question instead of an SQL query. As you will see, this will simply displace the security problem; it will not solve it.

</details>
</section>