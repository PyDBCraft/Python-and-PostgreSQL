<!-- Work Environment -->
<section
  id="work-environment"
  aria-labelledby="work-environment"
  data-item="Work Environment"
>
  <h2><a href="#work-environment">Setting up your Work Environment</a></h2>
  

You will need the following apps installed on your development computer in order to complete this tutorial. How you do this depends on your Operating System. Ensure that your Operating System is selected in the sidebar on the left:


::: {.os .win}
### Python 3
You can [download Python](https://www.python.org/downloads/) from the official website.

![Choose the standalone installer](images/windowsStandalone.webp)

Choose the standalone installer, because this gives you the option of checking the `Add python.exe to PATH` button before you click Install Now.

![Check the `Add python.exe to PATH` button](images/AddToPATH.webp)

This is important, because it allows you to run Python from the command line.

![Checking that Python is available on the command line](images/pythonVersion.webp)

### PostgreSQL
:::

::: {.os .mac}
### Python
You can [download Python](https://www.python.org/downloads/) from the official website, or you can run:

```bash-w
brew install python
```

After installation, you need to run a script to ensure Python 3 is recognized in your terminal:

* Navigate to the Applications folder.
* Find the Python 3.x folder.
* Right-click on Update Shell Profile.command and select "Open." This will configure your terminal to recognize the python3 command.

To check that `python3` is available on the command line, run:

```bash-w
python3 --version
```
If Python 3 is installed correctly, you should see the version number displayed.

### PostgreSQL

To install PostgreSQL, and ensure that it is running, use the following commands:

```bash-w
brew install postgresql@16
brew services start postgresql@16
```
:::

<!-- ```zsh-w
python3 -m ensurepip --upgrade
pip3 install psycopg2-binary
python3 -c "import psycopg2; print(psycopg2.__version__)"
which psql

brew install postgresql@16
brew services start postgresql@16

createdb postgres_db
``` -->

::: {.os .nux}
### Python
Python 3 is usually pre-installed. To make sure that you have the latest full version, run:

```language-command-line-w
sudo apt update
sudo apt install -y python3-full

python3 --version
```

If Python 3 is installed correctly, you should see the version number displayed.

You will also need `pip` for installing packages:
```language-command-line-w
sudo apt install -y python3-pip
```
:::

### VS Code
[Download Virtual Studio Code](https://code.visualstudio.com/download)
</section>