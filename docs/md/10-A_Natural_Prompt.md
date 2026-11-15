<!-- A Natural Prompt -->
<section
  id="a-natural-prompt"
  aria-labelledby="a-natural-prompt"
  data-item="A Natural Prompt"
>
  <h2><a href="#a-natural-prompt">A Natural Prompt</a></h2>
    
In this section, you are going to cheat. You are going to pretend to call an LLM to convert a natural language question into SQL, but in fact, you will use a pre-defined look up table to return pre-defined SQL queries for pre-defined questions.

Why?

You will be building a working pipeline with all the necessary pieces. Later, you can modify the faked pieces to communicate with a real LLM, without altering the pipeline.

### Sending Natural Language Questions from the Client

First, you can modify the `public/index.html`, `public/assets/script.js` and  `public/assets/styles.css` so that your browser client can send one of three pre-defined questions:

* find all meetings
* find ai meetings
* find meetings on friday
  
#### index.html
```html
<i><!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <link rel="stylesheet" href="assets/styles.css">
  <script defer src="assets/script.js"></script></i><b>
  <title>Natural Language</title></b><i>
</head>
<body></i><b>
  <h3>Natural Language Database Requests</h3>
  <div id="buttons">
    <button
      type="button"
      id="find-all"
      data-question="find all meetings"
    >
      Find All Meetings
    </button>
    <button
      type="button"
      id="find-ai"
      data-question="find ai meetings"
    >
      Find AI Meetings
    </button>
    <button
      type="button"
      id="find-friday"
      data-question="find meetings on friday"
    >
      Find Friday Meetings
    </button>
  </div>
  <p id="question"></p>
  <pre id="json"></pre>
  <pre id="result"></pre></b><i>
</body>
</html></i>
```

#### script.js (all new)
```javascript
// Read data from the server
const buttons    = document.getElementById("buttons")
const questionP  = document.getElementById("question")
const jsonP      = document.getElementById("json")
const resultP    = document.getElementById("result")
const endPoint = "/ask"

buttons.addEventListener("click", treatQuery)

function treatQuery({target}) {
  const { question } = target.dataset
  questionP.textContent = `Natural language question = "${question}"`
  
  const options = {
    method: "POST",
    headers: {
      "Content-Type": "application/json"
    },
    body: JSON.stringify({ question })
  }
  fetch(endPoint, options)
  .then(response => response.json())
  .then(displayResults)
}

function displayResults(results) {
  jsonP.textContent =  JSON.stringify(results, null, '  ')
  resultP.textContent = results.answer
}
```

#### styles.css

```css
<i>body {
  height: 100vh;
  margin: 0 auto;
  display: flex;
  flex-direction: column;
  align-items: center;
  color: #ddd;
  background-color: #222;</i><b>
  max-width: 800px;
}

h3 {
  text-decoration: underline;
}

pre {
  white-space: pre-wrap;
}

#question {
  margin: 0.25em;
}

#result {
  font-family:'Segoe UI', Tahoma, Geneva, Verdana, sans-serif
}</b>
```

<details class="trouble">
<summary>A Test Doomed To fail</summary>
You can restart your `main.py` server and then visit [http://127.0.0.1:8000/](http://127.0.0.1:8000/) after making these changes. However, if you click on any of the new buttons, the server will not know how to respond:

![The server responds with `Method Not Allowed`](images/notAllowed.webp)

This is because the new client expects to find an endpoint like `post("/ask")`, and the server currently only provides `get("/treat-query")`. This mismatch is what generates the `{ detail: "Method Not Allowed" }` response.

</details>

### Adding a `fake_llm_in.py` to Simulate a Call to an LLM

1. At the root of your `event_assistant` project directory, create a new file called `fake_llm.py`
2. Give it the following script.

#### fake_llm_in.py
```python
"""
A placeholder for the LLM. It takes a natural-language
question and returns an SQL query. For now, the mapping is
hard-coded; later, this module will call a real LLM and the
rest of the system will not need to change.
"""

import asyncio


# Hard-coded mapping from question to SQL.
# Add entries as the tutorial introduces new questions.
RESPONSES = {
    "find all meetings": "SELECT id, title, location, start_time FROM meetings",
    "find ai meetings": (
        "SELECT id, title, location, start_time FROM meetings "
        "WHERE title ILIKE '%AI%' OR title ILIKE '%LLM%'"
    ),
    "find meetings on friday": (
        "SELECT id, title, location, start_time FROM meetings "
        "WHERE EXTRACT(ISODOW FROM start_time) = 5"
    ),
}


async def question_to_sql(question: str) -> str:
    """
    Convert a natural-language question into a SQL query.

    This is async so that the rest of the system is already
    shaped for the real LLM call, which will be async. The
    current implementation is synchronous (a dictionary
    lookup) wrapped in asyncio.sleep(0) to make the async
    behaviour explicit.
    """
    await asyncio.sleep(0) # simulate the await of a real network call

    key = question.strip().lower()
    if key in RESPONSES:
        return RESPONSES[key]

    # A predictable fallback, so the endpoint always returns something.
    return "SELECT id, title, location, start_time FROM meetings"
```

This script is fully commented, so that you can understand how it cheats. It does not call an LLM; it simply choose an SQL query from a pre-determined list, or returns a generic SQL query if the incoming question does not match anything in its look-up list.

### Adapting `main.py` to Use `fake_llm_in.py`

You now need to update `main.py` so that it:
* Imports the `question_to_sql()` function from `fake_llm_in.py`
* Responds to POST requests for `"/ask"`
* Reads in the value of `question` from the parsed data it receives from the client
* Makes an interim request to `fake_llm_in.py` to retrieve an SQL query that matches the question
* Run this fake AI SQL instead of the `{ query }` it used to receive

Here are the changes you can make:

#### main.py
```python
<i>from fastapi import FastAPI
from fastapi.staticfiles import StaticFiles
from pathlib import Path

import psycopg2
from pydantic import BaseModel
from config import settings</i><b>

from fake_llm_in import question_to_sql</b><i>

app = FastAPI()</i><b>

class AskRequest(BaseModel):
    question: str


@app.post("/ask")
async def ask(request: AskRequest):
    # 1. Translate the question into SQL (placeholder for now).
    sql = await question_to_sql(request.question)
    print(f"[ask] sql: {sql}")

    # 2. Execute the SQL. NOTE: SQL IS UNTRUSTWORTHY.</b><i>
    conn = psycopg2.connect(
        dbname=settings.db_name,
        user=settings.db_user,
        password=settings.db_password,
        host=settings.db_host,
        port=settings.db_port
    )
    try:
        cur = conn.cursor()</i><b>
        cur.execute(sql)</b><i>
        rows = cur.fetchall()
        
        return [
           {"id": r[0], "title": r[1], "location": r[2], "start_time": r[3].isoformat()}
           for r in rows
        ]
    finally:
        cur.close()
        conn.close()

app.mount("/", StaticFiles(directory=Path("public"), html=True))</i>
```

### Displaying the Raw Output from the Database

Now, when you click on a button in the browser client, you see the same kind of response as you saw in <button data-name="anchor-sql">the previous section</button>.

![A natural language question results in raw data in response](images/naturalRaw.webp)

<details class="pivot" open>
<summary>What You've Just Learned</summary>
In this section, you've seen how to:

1. Send a natural language prompt like "find all meetings", instead of crafted SQL query. This means that people with no experience of SQL will be able to use the system.
2. Use a placeholder script to pretend to call an LLM to convert the natural prompt to SQL
3. Return the raw result of the fake AI SQL

An everyday user may have difficulty in interpreting the meanin of the raw data that is returned. In the next section, you'll add a second faked LLM request, to convert the raw data into a meaning natural language response.

</details>
</section>