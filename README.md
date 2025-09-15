# Flask REST API Demo

This project demonstrates a simple REST API built with Python's Flask framework. The API performs CRUD (Create, Read, Update, Delete) operations on user data, which can be tested locally and deployed to Azure App Service for cloud hosting.

## Features

- Retrieve all users
- Retrieve a specific user by ID
- Create a new user
- Update an existing user
- Delete a user

## Prerequisites

Before you can run or deploy this app, you need to have the following installed:

- Python 3.x
- pip (Python package manager)
- Flask (`pip install Flask`)
- gunicorn (`pip install gunicorn`)
- Azure CLI (optional, for deployment)

## Project Structure

- app.py: Main Flask application 
- requirements.txt: List of Python dependencies 
- test-api.http: Test the REST API using the REST Client extension in Visual Studio Code
- README.md: Documentation

## Running Locally

To run the Flask API on your local machine:

1. Clone this repository:

   ```bash
   git clone https://github.com/ramymohamed10/25F_CST8916_REST-API-DEMO.git
   ```
   
2. Navigate to the project directory:
   ```bash
   cd 25F_CST8916_REST-API-DEMO
3. Install the dependencies:
   ```bash
   pip install -r requirements.txt
4. Run the application:
   ```bash
   python app.py
5. The API will be running at http://127.0.0.1:8000
6. Use **test-api.http** to test the REST API using the REST Client extension in Visual Studio Code.

## Deploying to Azure

### What is Azure App Service?

[Azure App Service](https://learn.microsoft.com/en-us/azure/app-service/) is a fully managed platform for building, deploying, and scaling web apps. With Azure App Service, you can host web applications, REST APIs, and mobile backends in any programming language without managing infrastructure. It provides integrated continuous deployment and scaling features, making it a powerful and flexible solution for cloud-based web hosting.

Some key features of Azure App Service include:
- **Automatic Scaling**: Scale up or out depending on traffic.
- **Continuous Integration/Continuous Deployment (CI/CD)**: Easily integrate with GitHub, Bitbucket, or Azure DevOps for automated deployments.
- **Custom Domains and SSL**: Secure your apps with custom domains and certificates.
- **Load Balancing**: Built-in load balancing to handle high-traffic applications.
- **Monitoring and Diagnostics**: Access real-time monitoring and logs for troubleshooting.


You can learn more about Azure App Service and its features in the [official documentation](https://learn.microsoft.com/en-us/azure/app-service/).

To learn how to deploy a Python web app (Django, Flask, or FastAPI) to Azure App Service, refer to the [Quickstart Guide](https://learn.microsoft.com/en-us/azure/app-service/quickstart-python?tabs=flask%2Cwindows%2Cazure-cli%2Cazure-cli-deploy%2Cdeploy-instructions-azportal%2Cterminal-bash%2Cdeploy-instructions-zip-azcli). This guide walks you through deploying a Python web app to Azure, leveraging App Service to run your app in a Linux server environment.

## Why the URL is different (Local vs Azure)

When you run your Flask API **on your own laptop**:
- Flask is started with `python app.py`.
- You tell it to listen on **port 8000**.
- The URL is:  
  **http://localhost:8000/**  
  - **localhost** = your own computer  
  - **8000** = the port number you picked  

When you deploy the same app to **Azure App Service**:
- Azure gives your app a **public web address** like:  
  **https://cst8916-ramy-xxxx.azurewebsites.net/**
- Azure always serves your site using the **standard web ports**:  
  - **80** for HTTP  
  - **443** for HTTPS (secure)  
- That’s why the browser only shows `https://...` — because port 443 is the default.

---

### Think of it like this 🚪
- **Localhost:8000** → your house, back door, you choose the number (“8000”).  
- **Azure URL (443)** → a public library with one main entrance, number already set (443 = HTTPS).  
  You can’t change the public entrance — Azure manages it.

---

### Diagram

```text
LOCAL (your laptop)                          AZURE (cloud hosting)
───────────────────────                      ─────────────────────────────
Browser → http://localhost:8000/             Browser → https://yourapp.azurewebsites.net/
          |                                             |
          | port 8000 (you chose it)                    | port 443 (default HTTPS)
          ↓                                             ↓
      Flask app (python app.py)                   Azure front door → your Flask app
                                                  (internally still listening 8000)
```

## High-Level ASCII diagram

```
┌──────────────────────────────────────────────────────────────────────────────┐
│                         Flask REST API — Big Picture                         │
├──────────────────────────────────────────────────────────────────────────────┤
│ Clients                                                                      │
│ ┌────────────────────────┐     HTTP (JSON)     ┌───────────────────────────┐ │
│ │ VS Code REST Client    │  ─────────────────► │  Flask App (app.py)       │ │
│ │ (test-api.http)        │ ◄─────────────────  │  run: python app.py       │ │
│ └────────────────────────┘                     │  port: 8000               │ │
│ ┌────────────────────────┐                     │                           │ │
│ │ Browser / curl /Postman│                     │ Routes /  Endpoints       │ │
│ └────────────────────────┘                     │  ───────────────────────  │ │
│                                                │  GET    /users            │ │
│                                                │  GET    /users/{id}       │ │
│                                                │  POST   /users            │ │
│                                                │  PUT    /users/{id}       │ │
│                                                │  DELETE /users/{id}       │ │
│                                                └─────────────┬─────────────┘ │
│                                                              │               │
│                                                              ▼               │
│                                                ┌───────────────────────────┐ │
│                                                │ In-Memory User Store      │ │
│                                                │ (e.g., list/dict)         │ │
│                                                │                           │ │
│                                                └───────────────────────────┘ │
│                                                                              │
├──────────────────────────────────────────────────────────────────────────────┤
│ Local Dev Flow                                                               │
│    git clone → cd project → pip install -r requirements.txt → python app.py  │
│    → Test with test-api.http or browser → iterate                            │
├──────────────────────────────────────────────────────────────────────────────┤
│ Deploy to Azure App Service (Linux)                                          │
│                                                                              │
│ ┌───────────────┐     push / az deploy      ┌──────────────────────────────┐ │
│ │ GitHub Repo   │ ────────────────────────► │ Azure App Service (Gunicorn) │ │
│ └───────────────┘                           │ Public URL (HTTPS)           │ │
│                                             │ Auto scale • Logging • CI/CD │ │
│                                             └──────────────────────────────┘ │
├──────────────────────────────────────────────────────────────────────────────┤
│ Project Structure (key files)                                                │
│   app.py          → Flask application & routes                               │
│   requirements.txt → Python dependencies                                     │
│   test-api.http    → Ready-made requests for REST Client                     │
│   README.md        → Documentation                                           │
└──────────────────────────────────────────────────────────────────────────────┘
```

## Homework (Self-Study)

To better understand how a REST API is built with Flask, go through the source code in **app.py** and complete the following:

1. **Trace the Code**
   - Identify where the Flask app is created.
   - Locate the routes (`/users`, `/users/<id>`) and note which HTTP methods each supports.
   - See how data is stored (in-memory) and how JSON is returned.

2. **Test the Endpoints**
   - Run the app locally and use `test-api.http` or `curl` to try all CRUD operations.
   - Observe the status codes (200, 201, 404, etc.) and responses.

3. **Reflect**
   - Make sure you are able to describe how a request like `POST /users` flows:
     - From the request body → Flask route → data store → JSON response.

4. **Optional Enhancement**
   - Add one small improvement (e.g., input validation, error handling, or duplicate ID check).
   - Test your change with a sample request.

This is not a graded activity.

