---
title: "Scavenger"
date: 2020-11-11T13:45:41+08:00
draft: false
categories: ["projects"]
tags: ["project", "python", "web-app", "web-scraping", "flask", "vue.js"]
---

{{< image src="https://i.ibb.co/DD7V5qh/scavenger-1.jpg" alt="Scanvenger landing page" position="center">}}

## Introduction

[Scavenger](http://scavenger.nuttaphat.com/) is an experimental web scraping project as a service that simply offers an easy-to-use functionality to scrap a website and gather a specific piece of information as needed.

## Features

- **Web Scraping**: It gets its hands dirty going out to endless space of World Wide Web to make sure that you get what you need
- **Processing**: Gliding through a scraped website to seek a piece of information as required
- **Presenting**: It manages and presents the result beautifully to make it easier to comprehend

## Inspiration

One of the core ideas on developing this project was that I wanted to create a web app application and deliver it within tight schedules. I decided to quickly grasp **Flask** since it offers quick and easy development. Moreover, with **Jinja**, Flask is able to create templates at ease.

```python
from flask import Flask
app = Flask(__name__)

@app.route('/')
def index():
   return render_template('index.html')

if __name__ == '__main__':
   app.run(debug=True)
```

As for the application section, I decided to implement **Vue.js** from CDN in order to minimise server workloads. Combining **Flask templates** and **Vue.js** allowed me to be able to create a simple UI easily.

{{< image src="https://i.ibb.co/02mNTkh/scavenger-2.jpg" alt="Scanvenger app page" position="center">}}

With the power of **beautifulsoup4**, web scraping functionalities rely heavily on it. Even though there is a great number of methods that are ready to be used, I still needed to write business logic, prepare data schema, and create API endpoints. Indeed, this part will always need to be worked on as time goes by.

Furthermore, since it is a containerized web application, everything can be up and running in a matter of seconds by hooking **Dockerhub**, where the container lies, with **Azure** server.

> **Q:** So, how long did it take?
>
> **A:** Three weeks of my free time after work from scratch
>
> **Q:** Isn't that too slow?
>
> **A:** I have no idea. At least I did it.

I hope you enjoy using it as much as I do. Cheers.

## Tech Stack

- **Back-end**: [Flask](https://flask.palletsprojects.com/en/1.1.x/), [beautifulsoup4](https://pypi.org/project/beautifulsoup4/)
- **Front-end**: [Flask templates](https://flask.palletsprojects.com/en/1.1.x/tutorial/templates/), [Vue.js](https://vuejs.org/), [axios](https://github.com/axios/axios), [Foundation](https://get.foundation/)
- **Servers**: [uWSGI](https://uwsgi-docs.readthedocs.io/en/latest/), [nginx](https://www.nginx.com/), [Azure](https://azure.microsoft.com/)

- **CI/CD**: [Travis CI](https://travis-ci.org/), [Sentry](https://sentry.io/)

- **Version Control**: [GitHub](https://github.com/), [Dockerhub](https://hub.docker.com/)

- **Container**: [uwsgi-nginx-flask-docker](https://github.com/tiangolo/uwsgi-nginx-flask-docker)

## Project links

- [http://scavenger.nuttaphat.com/](http://scavenger.nuttaphat.com/)
- [https://scavenger-project.azurewebsites.net/](https://scavenger-project.azurewebsites.net/)
