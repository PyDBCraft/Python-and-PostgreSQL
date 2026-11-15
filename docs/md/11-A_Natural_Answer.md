<!-- A Natural Answer -->
<section
  id="a-natural-answer"
  aria-labelledby="a-natural-answer"
  data-item="A Natural Answer"
>
  <h2><a href="#a-natural-answer">A Natural Answer</a></h2>

  To convert the raw output of the SQL query into a natural answer, you can use a template. The process is very similar to what you did in the last section, but here it acts in reverse:
  
  Here's a second script that fakes what an LLM's response might be.

1. At the root of your `event_assistant` project directory, create a file named `fake_llm_out.py`
2. Give it the following script:

```python
"""
fake_llm_out.py

A placeholder for the output side of the LLM. It takes the
original question and the rows returned by the database, and
produces a natural-language answer. For now, the sentences are
built from a template; later, this module will call a real LLM
and the rest of the system will not need to change.
"""

import asyncio
from datetime import datetime


def _relative_date(start: datetime, now: datetime) -> str:
    """Return a human phrase for the date, computed deterministically."""
    days = (start.date() - now.date()).days
    if days == 0:
        return "today"
    if days == 1:
        return "tomorrow"
    if days == 2:
        return "the day after tomorrow"
    if 0 < days < 7:
        return f"this coming {start.strftime('%A')}"
    return start.strftime("%A, %B %d")


def _format_time(start: datetime) -> str:
    """Return a 12-hour time like '7 p.m.' from a 24-hour timestamp."""
    hour = start.hour % 12 or 12
    suffix = "a.m." if start.hour < 12 else "p.m."
    if start.minute:
        return f"{hour}:{start.minute:02d} {suffix}"
    return f"{hour} {suffix}"


async def rows_to_answer(question: str, rows: list[dict]) -> str:
    """
    Convert a question and its result rows into a natural-language
    answer.

    This is async so that the real LLM call can replace the body
    without changing the caller. The current implementation is a
    deterministic template.

    NOTE that `question` is not used here, but it will be sent as
    part of the prompt to a real LLM, to help it formulate an
    answer that best matches the original question.
    """
    await asyncio.sleep(0) # simulate the await of a real network call

    if not rows:
        return "I could not find any meetings matching that request."

    now = datetime.now()
    sentences = []
    for row in rows:
        start = datetime.fromisoformat(row["start_time"])
        sentences.append(
            f'The meeting "{row["title"]}" will take place at '
            f'{row["location"]} {_relative_date(start, now)} '
            f'at {_format_time(start)}'
        )

    if len(sentences) == 1:
        return sentences[0]
    return "Here are the meetings I found:\n\n" + "\n".join(
        f"- {s}" for s in sentences
    )
```

### Updating `main.py` to Use `fake_llm_out.py`

1. Make the following changes to `main.py`, so that it inserts `fake_llm_out.py` into the pipeline:

```python
<i>from fastapi import FastAPI
from fastapi.staticfiles import StaticFiles
from pathlib import Path

import psycopg2
from pydantic import BaseModel
from config import settings

from fake_llm_in import question_to_sql</i><b>
from fake_llm_out import rows_to_answer</b><i>

app = FastAPI()

class AskRequest(BaseModel):
    question: str


@app.post("/ask")
async def ask(request: AskRequest):
    # 1. Translate the question into SQL (placeholder for now).
    sql = await question_to_sql(request.question)
    print(f"[ask] sql: {sql}")

    # 2. Execute the SQL. NOTE: SQL IS UNTRUSTWORTHY.
    conn = psycopg2.connect(
        dbname=settings.db_name,
        user=settings.db_user,
        password=settings.db_password,
        host=settings.db_host,
        port=settings.db_port
    )

    try:
        cur = conn.cursor()
        cur.execute(sql)</i><b>
        raw_rows = cur.fetchall()
        rows = [
            {"id": r[0], "title": r[1], "location": r[2],
             "start_time": r[3].isoformat()}
            for r in raw_rows
        ]</b><i>
    finally:
        cur.close()
        conn.close()</i><b>

    # 3. Convert the rows into a natural-language answer
    #    (placeholder for now).
    answer = await rows_to_answer(request.question, rows)
    print(f"[ask] answer: {answer}")

    return {
        "question": request.question,
        "sql": sql,
        "rows": rows,
        "answer": answer,
    }</b><i>


app.mount("/", StaticFiles(directory=Path("public"), html=True))</i>
```

### Testing Your Fake LLMs

Now, when you click on one of the buttons in your browser client, the server generates a comprehensive response which includes a natural language answer. The screenshot below shows natural answer as well as the entire response sent by the server. In a production app, there should be no need to show the entire response: just the natural answer should do.

![A natural answer to a natural question](images/allNatural.webp)

<details class="pivot" open>
<summary>What You've Just Learned</summary>
In this section, you added a second LLM simulator into the pipeline, using principles that you have already seen. A question in natural language now receives a reply in natural language.

This proof-of-concept demonstrates the main steps needed to allow everyday users to get meaningful answers through an AI-assisted database.

Your next steps will be:

* To integrate a real LLM into the pipeline
* Handle the issues that arise when the LLM hallucinates or provides unsafe answers
* Expand the service provided to include all the features that end-user and meeting organizers will require.

**Congratulations on finishing the first part of this four-part tutorial!**

</details>

</section>