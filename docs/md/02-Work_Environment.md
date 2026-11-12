<!-- Work Environment -->
<section
  id="work-environment"
  aria-labelledby="work-environment"
  data-item="Work Environment"
>
  <h2><a href="#work-environment">Setting up your Work Environment</a></h2>
  

You will need the following apps installed on your development computer in order to complete this tutorial. How you do this depends on your Operating System. Ensure that your Operating System is selected in the sidebar on the left.

![Show instructions for your preferred operating system](images/osFilter.webp)

### Code Editor
You will need a code editor to create python, html and other scripts. A popular choice is Virtual Studio Code, which is free. You can [download Virtual Studio Code here](https://code.visualstudio.com/download).

::: {.os .win}
<details class="note" open>
<summary>PowerShell</summary>
This tutorial assumes that you will be using PowerShell in your Windows Terminal in VS Code.

Here are the steps to switch to PowerShell, if you have been using a different shell:

1. Open the Command Palette with `Ctrl + Shift + P`.
2. Start typing `Terminal: Select Default Profile` and select it.
3. Choose `PowerShell` (or `"Windows PowerShell"`) from the list that appears.

Important: Close the current terminal (click the trash can icon) and open a new one (Ctrl + `) for the change to take effect.
</details>

### Python
You can [download Python](https://www.python.org/downloads/) from the official website.

![Choose the standalone installer](images/windowsStandalone.webp)

Choose the standalone installer, because this gives you the option of checking the `Add python.exe to PATH` button before you click Install Now.

![Check the `Add python.exe to PATH` button](images/AddToPATH.webp)

This is important, because it allows you to run Python from the command line.

![Checking that Python is available on the command line](images/pythonVersion.webp)

### PostgreSQL

You can [download PostgreSQL for Windows](https://www.postgresql.org/download/windows/) from the official website.
:::

::: {.os .mac}
### Python
You can [download Python](https://www.python.org/downloads/) from the official website, or you can run:

```zsh-w
brew install python
```

After installation, you need to run a script to ensure Python 3 is recognized in your terminal:

* Navigate to the Applications folder.
* Find the Python 3.x folder.
* Right-click on Update Shell Profile.command and select "Open." This will configure your terminal to recognize the python3 command.

To check that `python3` is available on the command line, run:

```zsh-w
python3 --version
```
If Python 3 is installed correctly, you should see the version number displayed.

### PostgreSQL

To install PostgreSQL, and ensure that it is running, use the following commands:

```zsh-w
brew install postgresql@16
brew services start postgresql@16
```
:::

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

### Optional Tools

You might also like to install:

* [Postman](https://www.postman.com/downloads/), so that you can run automatic tests on your server
* [Git](https://git-scm.com/install/mac), so that you can save your work to a developer platform like [GitHub](https://github.com/), [GitLab](https://gitlab.com) or [Bitbucket](https://bitbucket.org/). Even if you only save your project locally, Git helps you make experiments and choose those that give the best results.

A Git repository containing the completed project will be available [here (eventually)]().
</section>