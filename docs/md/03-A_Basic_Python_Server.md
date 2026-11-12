<!-- A Basic Python Server -->
<section
  id="#a-basic-python-server"
  aria-labelledby="a-basic-python-server"
  data-item="A Basic Python Server"
>
  <h2><a href="#a-basic-python-server">A Basic Python Server</a></h2>

A key feature of this project is that it provides an API (Application Programming Interface) that allows a browser to communicate with a server, and through it, with a database.

You can think of an API like the waiters in a restaurant. When you enter the restaurant, all the tables are arranged in public in a standard way. The kitchen is often invisible. A customer will talk to a waiter who will take the message into the kitchen, and, a short while later, the customer will receive the specific food that they ordered.

In a news page in a browser, for example, articles and images will be arranged in a standard way, but depending on what links the visitor chooses, the specific data in the page will change.

### FastAPI

FastAPI is like a restaurant infrastructure. It is a framework that provides all the features that you need create a robust API, along with possibility to customize these features to your specific needs.

Before you can open a new restaurant, you must have a building, with an electricity supply, a gas supply, plumbing, and all the equipment like stoves and fridges that the staff will need. 

Before you can use FastAPI, you must have something similar. Fortunately, installing the structure for FastAPI is much faster than building a restaurant, and it is cost-free.

### A Virtual Environment for FastAPI

Here's how to get started, with an ultra-simple Python server. In a Terminal window, in the directory where you want to create your project, run the following commands:

```bash-w
mkdir event_assistant
cd event_assistant
python3 -m venv .venv
```

The line `python3 -m venv .venv` will create a directory called `.venv` (for Virtual ENVironment) in your `event_assistant` project directory.

The dot (.) as the first character of the name will make the `.venv` folder invisible. This is the convention, but you can ignore it. Even the name `.venv` is a convention: you could call it "george", and use "george" instead of `.venv` everywhere, but your colleagues will not appreciate this.

::: {.os .mac}
<details class="tldr">
<summary>Contents of `.venv`</summary>
```zsh-w
./
└── .venv/
    ├── .gitignore
    ├── bin/
    │   ├── activate
    │   ├── activate.csh
    │   ├── activate.fish
    │   ├── Activate.ps1
    │   ├── pip*
    │   ├── pip3*
    │   ├── pip3.15*
    │   ├── python -> python3.15*
    │   ├── python3 -> python3.15*
    │   └── python3.15 -> /Library/Frameworks/Python.framework/Versions/3.15/bin/python3.15*
    ├── include/
    ├── lib/
    │   └── python3.15/
    └── pyvenv.cfg
```

</details>
:::

### Discovering the Global Environment

Currently, if you run the command...

::: {.os .mac}
```zsh-w
which python3
```
... the output will be something like:

```zsh-w
/Library/Frameworks/Python.framework/Versions/3.15/bin/python3
```
And if you run the command...

```zsh-w
which python
```
... the output is likely to be:

```zsh-w
python not found
```
:::

::: {.os .win}
```powershell-w
which python
```
... the output will be something like:
```powershell-w
/c/Users/james/AppData/Local/Microsoft/WindowsApps/python
```
:::

### Activitating a Virtual Environment

The following command activates a virtual environment:

::: {.os .mac}
```zsh-w
source .venv/bin/activate
```
Now, if you run the command...

```zsh-w
which python3
```

... the output will be something like:
```zsh-w       
/Users/full/path/to/event_assistant/.venv/bin/python3
```

And:
```zsh-w
which python
```
... will show something like:
```zsh-w       
<i>/Users/</i><b>full/path/to/event_assistant/.venv/bin/</b>python
```
:::

::: {.os .win}
```powershell-w
.venv/Scripts/Activate.psl
```

<details class="trouble">
<summary>Troubleshooting</summary>

If you see an error message like...
```powershell-w
File C:\Users\james\Documents\event_assistant\.venv\Scripts\Activate.ps1 cannot be loaded because running scripts is disabled on this system.
```
... don't worry. PowerShell's default execution policy blocks scripts (including the `Activate.ps1` file) from running to protect you from malicious code.

To tell PowerShell that you know what you are doing, run this command once:

```powershell-w
Set-ExecutionPolicy -Scope CurrentUser -ExecutionPolicy RemoteSigned
```
This allows locally created scripts (like the venv activation script) to run. Simply run the command...

```powershell-w
.venv/Scripts/Activate.psl
```

... again after you run the `Set-ExecutionPolicy...` command, and all should be good.

</details>

Now if you run the command ...
```powershell-w
which python
```
... you should see something like:

```powershell-w       
<i>/c/Users/</i><b>full/path/to/event_assistant/.venv/Scripts/</b>python
```
:::

In other words, now that you have activated the virtual environment, python and pip commands point to the copies inside .venv, instead of the system-wide ones. This is important, because different projects may require different versions of the same custom package.

Your next step is to use `pip`, the Python package manager, to install FastAPI locally. If you did not use a virtual environment for all your projects, you would be forced to use the same globally installed version in all your other projects, too.

### Installing FastAPI in the Virtual Environment

Run the following command to install FastAPI with the standard extras that it needs to operate:

::: {.os .mac}
```zsh-w
pip install "fastapi[standard]"
```
:::

::: {.os .win}
```powershell-w
$env:PYYAML_FORCE_LIBYAML="0"
pip install "fastapi[standard]"
```
:::
You should see a rush of text in the Terminal window as `pip` downloads the files needed by FastAPI. These will appear in your `.venv` directory.

::: {.os .mac}
<details class="tldr" >
<summary>Updated contents of `.venv`</summary>
```zsh-w
<i>./
└── .venv/
    ├── .gitignore
    ├── bin/
    │   ├── activate
    │   ├── activate.csh
    │   ├── activate.fish
    │   ├── Activate.ps1</i><b>
    │   ├── agent-detector*
    │   ├── detect-installer-test*
    │   ├── dotenv*
    │   ├── email_validator*
    │   ├── fastapi*
    │   ├── httpx*
    │   ├── httpx2*
    │   ├── idna*
    │   ├── markdown-it*
    │   ├── normalizer*</b><i>
    │   ├── pip*
    │   ├── pip3*
    │   ├── pip3.15*</i><b>
    │   ├── pygmentize*</b><i>
    │   ├── python -> python3.15*
    │   ├── python3 -> python3.15*
    │   ├── python3.15 -> /Library/Frameworks/Python.framework/Versions/3.15/bin/python3.15*</i><b>
    │   ├── typer*
    │   ├── uvicorn*
    │   ├── watchfiles*
    │   └── websockets*</b><i>
    ├── include/
    ├── lib/
    │   └── python3.15/
    └── pyvenv.cfg</i>
```
</details>
:::

With all this infrastructure in place, it's time to cook a first simple meal.

### A Simple Hello World Server

Below is the Python code for a FastAPI server that responds to requests for the root path with "Hello World!" (in Russian, to check how browsers will handle Unicode).

Create a file called `main.py` inside your `event_assistant` project directory, and give it the following script:

```python
# Import FastAPI class from the fastapi package
from fastapi import FastAPI

# Create an instance of the FastAPI app
app = FastAPI()

# Define a GET route for the root URL ("/")
@app.get("/")
async def read_root() -> str:
    # Return a plain text response
    return "Здравствуй, мир!"
```

To launch this simple server, run the command:

```bash-w
fastapi dev main.py
```

You should see the following output (and more) in the Terminal:

```bash-w
 ⚡️ Starting FastAPI in development mode

 🐍 Using import string: main:app

 🌐 Server started at http://127.0.0.1:8000
    Documentation at http://127.0.0.1:8000/docs
```

### Viewing the Response in your browser

Command-click on the link to [http://127.0.0.1:8000](http://127.0.0.1:8000). Your favourite browser should open to show the server's response to the request.

![The response in Chrome](images/hwChrome.webp)

![The response in Firefox](images/hwFirefox.webp)

![The response in Safari](images/hwSafari.webp)

![The response in Edge](images/hwEdge.webp)

### Why the Browsers Disagree

The response from the server is a short string of plain text. It is not in HTML format. Chrome, Firefox and Edge guess (correctly) that the string is in `UTF-8` format. Safari makes a different guess: that it is in Windows-1252.

### Fixing the Issue Live

You could hide the problem, à la [Potemkin village](https://en.wikipedia.org/wiki/Potemkin_village), by replacing the Cyrillic text with Latin characters. That would not be a solution, but it will show the power of FastAPI. Edit your `main.py` file as follows:

```python
<i># Import FastAPI class from the fastapi package
from fastapi import FastAPI

# Create an instance of the FastAPI app
app = FastAPI()

# Define a GET route for the root URL ("/")
@app.get("/")
async def read_root() -> str:
    # Return a plain text response</i><b>
    return "Hello world!"</b>
```

Now, refresh your browser page. You should immediately see the text "Hello world!" appear in your browser. 

Your change doesn't fix the encoding problem. It sidesteps it, because 'Hello world!' is pure ASCII, and every browser agrees on ASCII. The Unicode problem is still there, waiting for the next non-Latin string. 

What this does demonstrate is something useful. Remark that you did not stop and restart the server. FastAPI did this for you as soon automatically as it noticed a change to `main.py`. That saves you time.

<details class="question" open>
<summary>What happens if you include a pathname in the URL?</summary>
Try visiting [http://127.0.0.1:8000/foo]. What happens(http://127.0.0.1:8000/foo). What happens?

![Any path other than the root `/` responds with "Not Found"](images/fooNotFound.webp)

The line...

```python-#8
@app.get("/")
```

... ensures that the following command...

```python-#9
async def read_root() -> str:
    # Return a plain text response</i><b>
    return "Hello world!"</b>
```

... applies specifically to a request for the root URL (`/`), and no other. For now, your server has no instructions about what to do for any other request, so it responds with the JSON string: `'{ "detail": "Not Found" }'`.

A major theme in this tutorial is how to handle many different requests, including unexpected ones. So a full solution for this issue is coming.

</details>

<details class="note" open>
<summary>A note about `__pycache__`</summary>
You may notice that a directory named `__pycache__/`, containing a file named `main.cpython-315.pyc`, appeared at the root of your `event_assistant` project folder, after you ran...

```bash-w
fastapi dev main.py
```

The `__pycache__` directory is where Python caches compiled versions of your code to start faster next time. **You can safely ignore this folder.** If you delete it, it will be recreated automatically next time the server starts.

If you are using Git, you should make sure Git ignores it too, by adding `__pycache__` to your `.gitignore` file.

</details>

<details class="pivot" open>
<summary>What You've Just Learned</summary>
In this section, you have:

1. Created a virtual environment for running your Python project
2. Installed a number of dependencies, including `FastAPI`, so that they are available for Python to use locally
3. Created a barebones server that proves that it is working by responding with a string when the root path (`/`) is requested
4. Noted that different browsers may treat the response differently
5. Noted that requests for paths other than `/` are treated as "Not Found"
6. Discovered that FastAPI will restart your server automatically after you make a change
7. Discovered that it is important to try to break your project, so that you fix it before others start to use it.

In the next section, you'll see how to make the server respond to a request with an HTML page, complete with an entry for `<meta charset="UTF-8">`, so that all browsers will know how to treat the Unicode text.

</details>
</section>