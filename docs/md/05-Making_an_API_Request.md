<!-- Making an API Request -->
<section
  id="making-an-api-request"
  aria-labelledby="making-an-api-request"
  data-item="Making an API Request"
>
  <h2><a href="#making-an-api-request">Making an API Request from a Client Page</a></h2>

In this section, you'll make changes to both your new `index.html` page and your `main.py` server script, so that you can ask the server to tell you the time. You'll start by adding a button to `index.html`.
  
1. Make the following changes to the `index.html` file, to add a button and way to show the response to a click on the button:

```html
<i><!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta
    name="viewport"
    content="width=device-width, initial-scale=1.0"
  >
  <link rel="stylesheet" href="assets/styles.css"></i><b>
  <script defer src="assets/script.js"></script>
  <title>API Request</title></b><i>
</head>
<body></i><b>
  <h3>An API Request</h3>
  <button
    type="button"
    id="get-time"
  >
    Get Server Time
  </button>
  <p id="time">
    Click the button
  </p></b><i>
</body>
</html></i>
```

2. Inside the `public/assets/` folder, create a file called `scripts.js`
3. Give it the following script:

```javascript
const button = document.getElementById("get-time")
const time = document.getElementById("time")
const endpoint = "/get_time"

button.addEventListener("click", getTime)

function getTime(params) {
  fetch(endpoint)
  .then(response => console.log("response:", response))
}
```

Notice that this script is read in by the `index.html` page, in line 6:

```html-#6
  <script defer src="assets/script.js"></script>
```

The `defer` attribute means: wait until the browser has created all the HTML elements before running the script. This ensures that the Get Server Time button exists before the script tries to manipulate it.

A click on the Get Server Time button will send a request for `/get-time` in the server script... but the server script is not yet ready to respond to this.

4. Refresh your browser page.
5. Click on the button.
6. Open the Developer Tools of your browser at the Console tab.

![The API request sent by the Get Server Time button receives no response](images/untimelyButton.webp)

### What's (Not) Happening

In `public/assets/script.js` , the `fetch` command...

```javascript-#8
  fetch(endpoint)
```

... tells the browser to make a request to the server for the endpoint `/get_time`. This is an asynchronous process. It may take the server some time to respond, so the line...

```javascript-#9
  .then(response => console.log("response:", response))
```

... waits for the response before it executes.

However, the server does not know how to respond, because you have not yet told it what to do when it receives a request for anything other than the root (`/`) path. As a result, the server responds with a JSON string, including `status: 404`, `ok: false` and `statusText: "Not Found"`.

This is not what you want, but it is what your code asked for, and what is logged to your browser console.

### Getting the Server to Respond to the Request

To get `main.py` to respond correctly to this new request, you need to make some changes. In particular, you are going to need to use a new tool — the `datatime` class — which is part of the core of Python.

7. Make the following changes to `main.py`:

```python
<i>from fastapi import FastAPI
# Prepare to serve static files from a specific folder
from fastapi.staticfiles import StaticFiles
from pathlib import Path</i><b>
# Import a function to handle time
from datetime import datetime</b><i>

# Create an instance of the FastAPI app
app = FastAPI()</i><b>

# Respond to a request for "/get_time" with the time on the server
@app.get("/get_time")
async def get_time():
    return {"time": datetime.now().isoformat()}</b><i>

# Ensure that a request for "/" serves up "public/index.html"
app.mount("/", StaticFiles(directory=Path("public"), html=True))</i>
```

FastAPI should restart the server for you automatically.

8. Click on the Get Server Time button again, and observe how the response shown in the browser Console has changed.

![The server responds, but the client doesn't treat the response](images/untreatedTime.webp)

### Understanding the Server's Response

After the changes you made to `main.py`, the server _does_ know how to handle a request for `/get_time`. As you can see `status` is now [`200`](https://developer.mozilla.org/en-US/docs/Web/HTTP/Reference/Status/200), `ok` is `true` and `statusText` is `"OK"`. But where is the time?

You'll notice that the response includes `body: ReadableStream`.

When you make a request from a server, the response can be as short as "ok", or it may be live-streamed video, whose length cannot yet be known. In both cases, the server will wrap its response in a `ReadableStream` which the client can unpack, bit by bit, as it becomes available.

In this case, the `body` contains the JSON string `{"time": <time string in isoformat>}`, but this has to be unwrapped before it can be used.

### Treating the Response in the Client

Here's how to extract the JSON object from the body of the response message.

9. Make the following changes to the file at `public/assets/script.js`:

```javascript
<i>const button = document.getElementById("get-time")
const watch = document.getElementById("watch")
const endpoint = "/get_time"

button.addEventListener("click", getTime)

function getTime(params) {
  fetch(endpoint)</i><b>
  .then(response => response.json())
  .then(displayServerTime)
}

function displayServerTime({ time }) {
  watch.textContent = time
}</b>
```

10. Click the Get Server Time button again and see the value that is displayed.

![Each time you click on the button, the displayed time changes](images/serverTime.webp)

<details class="pivot" open>
<summary>What You've Just Learned</summary>
In this section, you have:

1. Edited your `index.html` file so that it displays a button and a field to show text.
2. Attached a script to the `index.html` page, to make the button active, and to display the result of its action.
3. Update `main.py` so that it responds to a request for a specific pathname.
4. Seen the importance of progressing step by step, checking at each stage the results of your changes.

**You now have a server that can respond dynamically to a specific request.**

In the next two section, you will take a step to the side, to see how Python can connect to a PostgreSQL database, and how to add data to the database. After that, you will use your new knowledge to get your server to respond to a request for data from the database.

</details>

</section>