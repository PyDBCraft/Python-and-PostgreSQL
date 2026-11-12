<!-- Responding with HTML -->
<section
  id="responding-with-html"
  aria-labelledby="responding-with-html"
  data-item="Responding with HTML"
>
  <h2><a href="#responding-with-html">Responding with HTML</a></h2>
    
FastAPI is designed first and foremost to handle API requests, where a client asks the server to respond with dynamic data, in JSON format, based on a specific query. It can also serve static files, like an `index.html` page. And an `index.html` can make the ideal client for testing the API.

**In this section, you will be creating a simple `index.html` page and a style sheet for it, and using your existing server to deliver this.
**

The focus of this tutorial is on the API, so I will keep the frontend `index.html` as simple as possible.

### A Simple `index.html` files

1. Create a folder called `public/` at the root of your `event_assistant` project folder.
2. Inside the `public/` folder, create a file called `index.html`
3. Give it the following script:

```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta
    name="viewport"
    content="width=device-width, initial-scale=1.0"
  >
  <link rel="stylesheet" href="assets/styles.css">
  <title>Hello world!</title>
</head>
<body>
  Здравствуй, мир!
</body>
</html>
```

4. Create a subfolder called `assets/` inside the `public` folder.
5. Inside the `assets/` folder, create a file named `styles.css`.
6. Give it the following script:

```css
body {
  height: 100vh;
  margin: 0;
  display: flex;
  justify-content: center;
  align-items: center;
  color: #ddd;
  background-color: #222;
}
```

### File Hierarchy

This is what your `exent_assistant` project folder should look like now:
```bash-w
.
├── __pycache__
│   └── main.cpython-315.pyc
<i>├── .venv/</i>
├── main.py
<b>└── public
    ├── assets
    │   └── styles.css
    └── index.html</b>
```

### Updating `main.py`

7. Make the following changes to the `main.py` script:

```python
<i>from fastapi import FastAPI</i>
<b># Prepare to serve static files from a specific folder
from fastapi.staticfiles import StaticFiles
from pathlib import Path></b><i>

# Create an instance of the FastAPI app
app = FastAPI()</i><b>

# Ensure that a request for "/" serves up "public/index.html"
app.mount("/", StaticFiles(directory=Path("public"), html=True))</b>
```

8. If your server is not running, restart it with:

```bash-w
fastapi dev main.py
```

(Normally, `fastapi` should have restarted it for you.)

9. In your browser, visit [http://127.0.0.1:8000](http://127.0.0.1:8000).

![How `index.html` appears in a browser](images/HelloWorld.webp)

<details class="note" open>
<summary>Setting the `charset`</summary>
Even in Safari, the Unicode text will be displayed correctly. This is because the `index.html` file contains this tag:

```html
<meta charset="UTF-8">
```
This tells any browser that the text in the `index.html` file is to be treated an `UTF-8`

</details>

<details class="pivot" open>
<summary>The `index.html` file as a client for the backend</summary>
Now that you have seen how to get your server to deliver an HTML page, you are ready to modify `index.html` so that it sends an API request to your server. This is what you will do in the next section.

</details>
</section>