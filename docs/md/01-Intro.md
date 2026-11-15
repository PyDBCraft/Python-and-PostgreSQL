---
title: Python and PostgreSQL
subtitle: A backend for a MeetUp-like web app
month: November 2026
organization: PyDBCraft
repo: Python-and-PostgreSQL
---
<section
  id="intro"
  aria-labelledby="intro"
  data-item="Introduction"
>
  <h2><a href="#intro">Introduction</a></h2>

Demand is growing for developers who can use AI to convert a natural language request into valid SQL to query a database.

This tutorial is designed to help you build an "intelligent event assistant" for a web site similar to [MeetUp.com](https://meetup.com/). The ultimate goal is to allow end users to type a phrase like...

* "Find me a tech meetup next week that's free."
* "What events are happening near Jubilee metro station on Friday evening?"
* "Show me upcoming events about AI or databases."

... and have the backend convert this into a query for a database, and respond with a useful answer.

It is written for someone who is comfortable with basic programming concepts and has used a database before, but has not yet built a web server.

### A Three-Part Tutorial

This tutorial is in four parts:

1. Create a basic proof-of-concept pipeline that works for limited input
2. Add an AI layer that gets a Large Language Model to create custom queries for the database
3. Build a robust framework around the AI pipeline, improve accuracy and security
4. Add all the features that end-users will expect from such an app.

### Toolchain

In building this first part of the project, you will be using the following tools:

#### The Stack
* Python 3: a scripting language to develop a server
* FastAPI: a framework for communicating between a browser and the server
* Uvicorn: a server to run the Python script and reload it after any changes
* PostgreSQL: a database to hold data about users, groups and meetings
* A database driver (psycopg2-binary) that lets Python talk to PostgreSQL

#### The Tooling
These are the optional tools that I use during development:

* Postman (for testing)
* Git (for collaboration and version control)
* Virtual Studio Code (for editing code)

### Teaching Approach

I do not assume that you have used any of these technologies before. I include detailed explanations of each concept in collapsible sections. If you are a beginner, you can open these and learn precisely what is happening at each step. If you are more advanced, you can skip over these collapsed sections.

Some collapsible sections contain deliberately broken versions of the code, with a challenge to diagnose the problem before reading the explanation. These are optional; the mainline code always works.

I am working on MacOS, and this will be visible from the screenshots. If you are working on a different operating system (such as Windows) some of the Terminal commands will be different from those shown in the screenshots. Choose your operating system from the pop-up menu in the sidebar, and always check that the code snippets match this.

![Choose your operating system](images/osFilter.webp)

![Check that code snippets match your operating system](images/os-specific.webp)

### Steps

This is what you will do as you work through this first part of the tutorial:

1. Create a barebones Python server responds with "Hello World!"
2. Adapt the server to return an HTML file that says "Hello World!"
3. Make the button in the HTML file that call the server; server responds with its local time
4. Connect a Python script to PostgreSQL
5. Add a table with dummy data to a Postgres database
6. Insert dummy data (users, groups, meetings)
7. Query the database from the HTML page to retrieve all meetings
8. Create a script that simulates the response of an LLM and returns an SQL query
9. Send a natural language question from the HTML page and retrieve the raw result
10. Create a second script that simulates how an LLM could convert the raw result into a natural language answer

</section>
