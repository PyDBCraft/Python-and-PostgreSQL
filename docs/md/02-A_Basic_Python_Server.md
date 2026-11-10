<!-- A Basic Python Server -->
<section
  id="A Basic Python Server"
  aria-labelledby="A Basic Python Server"
  data-item="A Basic Python Server"
>
  <h2><a href="#A Basic Python Server">A Basic Python Server</a></h2>

A key feature of this project is that it provides an API (Application Programming Interface) that allows a browser to communicate with a server, and through it, with a database.

You can think of an API like the waiters in a restaurant. When you enter the restaurant, all the tables are arranged in public in a standard way. The kitchen is often invisible. A customer will talk to a waiter who will take the message into the kitchen, and, a short while later, the customer will receive the specific food that they ordered.

In a news page in a browser, for example, articles and images will be arranged in a standard way, but depending on what links the visitor chooses, the specific data in the page will change.

### FastAPI

FastAPI is like a restaurant infrastructure. It is a framework that provides all the features that you need create a robust API, along with possibility to customize these features to your specific needs.

Before you can open a new restaurant, you must have a building, with an electricity supply, a gas supply, plumbing, and all the equipment like stoves and fridges that the staff will need. 

Before you can use FastAPI, you must do something similar. Fortunately, installing the structure for FastAPI is much faster than building a restaurant, and it is cost-free.

### A Virtual Environment for FastAPI

Here's how to get started, with an ultra-simple Python server. In a Terminal window, in the directory where you want to create your project, run the following commands:

```bash-w
mkdir event_assistant           # create the project directory
cd event_assistant              # enter the project directory
python3 -m venv .venv           # create a Virtual Environment
```

The line `python3 -m venv .venv` will create a directory called `.venv` (for Virtual ENVironment) in your `event_assistant` project directory.

The dot (.) as the first character of the name will make the `.venv` folder invisible. This is the convention, but you can ignore it. Even the name `.venv` is a convention: you could call it "george", and use "george" instead of `.venv` everywhere, but your colleagues will not appreciate this.

<details class="tldr">
<summary>Contents of `.venv`</summary>
```zsh
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

### Activating the Virtual Environment

Currently, if you run the command...

```zsh
which python3
```
... the output will be something like:
```zsh
/Library/Frameworks/Python.framework/Versions/3.15/bin/python3
```
And if you run the command...

```zsh
which python
```
... the output is likely to be:
```zsh
python not found
```

The following command activates the virtual environment:

```zsh
source .venv/bin/activate       # activate the Virtual Environment
```

Now, if you run the command...

```zsh
which python3
```
... the output will be something like:
```zsh       
/Users/full/path/to/event_assistant/.venv/bin/python3
```

And:
```zsh
which python
```
... will show something like:
```zsh       
/Users/full/path/to/event_assistant/.venv/bin/python
```

In other words, now that you have activated the virtual environment, python3 and pip commands now point to the copies inside .venv, instead of the system-wide ones. This is important, because different projects may require different versions of the same package.

Your next step is to use `pip`, the Python package manager, to install FastAPI locally. If you did not use a virtual environment for all your projects, you would be forced to use the same globally installed version in all your other projects, too.

### Installing FastAPI in the Virtual Environment

Run the following command to install FastAPI

```zsh
pip install "fastapi[standard]" # install with the standard extras
```
You should see a rush of text in the Terminal window as `pip` downloads the files needed by FastAPI. These will appear in your `.venv` directory:

<details class="tldr" >
<summary>Updated contents of `.venv`</summary>
```zsh
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

With all this infrastructure in place, it's time to cook a first simple meal.

### A Simple Hello World Server

Below is the Python code for a FastAPI server that responds to requests for the root path with "Hello World!" (in Russian, to check how browsers will handle Unicode) [(source)](https://en.wikipedia.org/wiki/FastAPI#Example).

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
```zsh
fastapi dev main.py
```
You should see the following output (and more) in the Terminal:

```zsh
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

### Why the Browsers Disagree

The response from the server is a short string of plain text. It is not in HTML format. Chrome and Firefox guess (correctly) that the string is in `UTF-8` format. Safari makes a different guess: that it is in Windows-1252.

### Fixing the Issue Live

You could hide the problem by replacing the Cyrillic text with Latin characters. That would not be a solution, but it will show the power of FastAPI. Edit your `main.py` file as follows:

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

<details class="pivot" open>
<summary>Responding with HTML</summary>
In the next section, you'll see how to make the server respond to a request with an HTML page, complete with an entry for `<meta charset="UTF-8">`, so that all browsers will know how to treat the Unicode text.

</details>
</section>