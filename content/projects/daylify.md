---
title: "Daylify"
date: 2021-11-23T09:28:06+08:00
draft: false
categories: ["projects"]
tags: ["project", "python", "web-app", "vue.js", "fastapi"]
---

{{< image src="https://i.ibb.co/G7J6KVW/daylify-logo-transparent.png" alt="daylify-logo" position="center">}}

## Introduction

[Daylify](https://daylify.nuttaphat.com/) is a user-friendly web application designed to help you track and record your daily moods.

## Inspiration

The primary motivation behind creating Daylify was to develop a practical web application that would allow users to easily track their moods. Additionally, I wanted to evaluate my personal growth and highlight the milestones of my journey.

To achieve these goals, I leveraged the power of Vue.js for the user interface and FastAPI for the API, both hosted separately. For the database, I opted for a NoSQL solution using Azure Cosmos DB. This technology stack enabled me to rapidly develop and deploy a fully functional web application.

I highly recommend using FastAPI along with Pydantic for defining models and schemas when working with NoSQL databases.

By employing the Vue.js framework, I was able to create a simple and intuitive user interface effortlessly. Furthermore, the application can be easily set up on GitHub with a CI/CD pipeline for Netlify.

## Key Features

- **Authentication**: Users can sign up and sign in using their username and password.

{{< image src="https://i.ibb.co/XSydZpk/regist.jpg" alt="regist" position="center">}}

- **Mood Tracking**: Users can record their moods on a daily basis.

{{< image src="https://i.ibb.co/t47SKPT/note.jpg" alt="note" position="center">}}

- **Heatmap**: Users can view their moods in a heatmap.

{{< image src="https://i.ibb.co/D4YSZyX/heatmap.jpg" alt="heatmap" position="center">}}

- **i18n**: Internationalization support

{{< image src="https://i.ibb.co/xqFNnfF/i18n.jpg" alt="i18n" position="center">}}

## Tech Stack

- **Backend**: [FastAPI](https://fastapi.tiangolo.com/) hosted on [Azure](https://azure.microsoft.com/)
- **Frontend**: [Vue.js](https://vuejs.org/) hosted on [Netlify](https://www.netlify.com/)
- **Database**: [Azure Cosmos DB](https://docs.microsoft.com/en-us/azure/cosmos-db/)
- **Version Control**: [GitHub](https://github.com/)
- **Containerization**: [uvicorn-gunicorn-docker](https://github.com/tiangolo/uvicorn-gunicorn-docker)

## Project links

- [https://daylify.nuttaphat.com/](https://daylify.nuttaphat.com/)
