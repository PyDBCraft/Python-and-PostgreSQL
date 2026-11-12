<!-- Making an API Request -->
<section
  id="making-an-api-request"
  aria-labelledby="making-an-api-request"
  data-item="Making an API Request"
>
  <h2><a href="#making-an-api-request">Making an API Request</a></h2>
  
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

4. Refresh your browser page.
5. Click on the button.
6. Open the Developer Tools of your browser at the Console tab.

![The API request sent by the Get Server Time button receives no response](images/untimelyButton.webp)

### What's (Not) Happening



### Getting the Server to Respond to the Request

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

8. Click on the Get Server Time button again, and observe how the response shown in the browser Console has changed.

![The server responds, but the client doesn't treat the response](images/untreatedTime.webp)


### Treating the Response in the client

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

</section>