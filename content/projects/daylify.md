---
title: "Daylify"
date: 2021-11-23T09:28:06+08:00
draft: false
categories: ["projects"]
tags: ["project", "python", "web-app", "vue.js", "fastapi"]
---

{{< image src="https://i.ibb.co/G7J6KVW/daylify-logo-transparent.png" alt="daylify-logo" position="center">}}

## Introduction

[Daylify](https://daylify.com) is an experimental web application that allows users to record their moods on a daily basis.

## Inspiration

I wanted to create an easy-to-use web application to practice my skills. Furthermore, I would like to evaluate myself to see how I grow as a person, and to examine the highlights of my journey.

This project uses **Vue.js** for the UI, and **FastAPI** for the API which are hosted separately. As for the database, I decided to adopt a **NoSQL** database using **Azure Cosmos DB**. This combination of technologies allows me to develop and deploy a workable web application quickly.

**FastAPI** with [**Pydantic**](https://pydantic-docs.helpmanual.io/) makes it easier to define models and schemas for the APi wit NoSQL databases. I could not recommend enough.

By using **Vue.js** framework, I was able to create a simple and intuitive UI with ease. Furthermore, it can easily be set up on **GitHub** with CI/CD pipline for **Netlify**.

## Features

- **Authentication**: Users can sign up and sign in using their username and password.

{{< image src="https://i.ibb.co/XSydZpk/regist.jpg" alt="regist" position="center">}}

- **Note**: Users can record their moods on a daily basis.

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
- **Container**: [uvicorn-gunicorn-docker](https://github.com/tiangolo/uvicorn-gunicorn-docker)

## Project links

- [https://daylify.nuttaphat.com/](https://daylify.nuttaphat.com/)
