---
title: "COVID19 API"
date: 2020-06-20T14:39:53+08:00
draft: false
categories: ["projects"]
tags: ["project", "python", "covid19", "api"]
---

{{< image src="https://i.ibb.co/Wg2yPBq/covid19-api-logo.png" alt="COVID-19 LOGO" position="center">}}

_API for exploring covid-19 cases around the globe powered by FastAPI framework_

## Introduction

As going through 2020, it is clear that the year of the Rat has NOT been friendly at all. We have experienced a series of fierce events.
For instance, the tension among global giants, protesting, Australian bushfire season, extreme racial discrimination, and noticeably the COVID-19 pandemic.

COVID-19 has been spreading rapidly without showing any signs of stopping or even slowing down. Even though this pandemic has made a lot of lives miserable, we work together closely as a community to tackle this problem and prevent further damage. Hence, all the data related MUST be collected and disseminated. More importantly, the data MUST also be accurate and accessible.

Therefore, as a software engineer, I have created my version of API allowing those who are in need of the data to be able to access to the public data collected by Johns Hopkins University. I do hope that this project will be useful as a contribution.

## How it works behind the scene

Data collection is always a demanding process which needs to be done. Luckily, this process was already done by Johns Hopkins University and can be accessed at [COVID-19 Data Repository by the Center for Systems Science and Engineering (CSSE) at Johns Hopkins University](https://github.com/CSSEGISandData/COVID-19) However, the data is raw which can be quite difficult to read without knowing what exactly we are looking for.

By using [FastAPI framework](https://fastapi.tiangolo.com/), I created an API server collecting data from the data repository and pre-processing them using a Python's data analysis and manipulation library [Pandas](https://pandas.pydata.org/). With those powerful tools mentioned earlier, the data is ready to be queried quickly and effortlessly. Not only it provides a faster way to create an API server, but also an automatically-generated API documentation.

### Workflow

{{< image src="/img/projects/covid19_api/workflow.png" alt="WORKFLOW" position="center">}}

As the workflow depicted above suggests, the process is fairly simple and straight forward.

1. Get a request from a user
2. Get the raw data from the repository
3. Pre-process the data using Pandas
4. Send a response with the result data as JSON

```json
{
  "data": {
    "confirmed": 16682030,
    "deaths": 659374,
    "recovered": 9711187,
    "active": 5538584
  },
  "dt": "07-29-2020",
  "ts": 1595980800
}
```

### Deployment

Moreover, a Dockerfile is also included. This will allow the API server to be deployed with ease by simply using

```console
docker-compose up -d
```

This will deploy and run a server on the background with pre-defined configurations which can be altered as see fit. Alternatively, we can also run a server with uvicorn as shown below for debugging purposes:

```console
$ uvicorn main:app --reload
INFO:     Uvicorn running on http://127.0.0.1:8000 (Press CTRL+C to quit)
INFO:     Started reloader process [16788]
INFO:     Started server process [9784]
INFO:     Waiting for application startup.
INFO:     Application startup complete.
INFO:     127.0.0.1:51196 - "GET / HTTP/1.1" 307
INFO:     127.0.0.1:51196 - "GET /docs HTTP/1.1" 200
INFO:     127.0.0.1:51196 - "GET /openapi.json HTTP/1.1" 200
```

## Summary

This project is an open-source API server powered by FastAPI allowing everyone to access the COVID-19 data effortlessly. With a powerful library, as such Pandas, it allows the data manipulation to be performed quickly and clearly; furthermore, the data analysis can be done more efficiently. Without a doubt, the combination of FastAPI and Pandas demonstrates how impactful open data can be.

## To Learn More

Please visit the following links

- [Repository](https://github.com/nat236919/covid19-api)
- [API documentation](https://nat236919.github.io/covid19-api)
