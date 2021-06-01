---
title: "Introducing FastAPI"
date: 2021-06-01T14:36:15+08:00
draft: false
categories: ["blog"]
tags: ["blog", "python", "fastapi"]
---

{{< image src="https://fastapi.tiangolo.com/img/logo-margin/logo-teal.png" alt="FastAPI Logo" position="center">}}

## Introduction

Developing APIs can be painful if we don't know where to start. As I've always been a fan of Python, it is my first language of choice for development. It has tons of frameworks supporting web development (e.g., [Django](https://www.djangoproject.com/), [Flask](https://flask.palletsprojects.com/en/2.0.x/), etc.). However, this time I would like to introduce a super-fast and easy-to-use framework which I've been using for a couple of years now: [FastAPI](https://fastapi.tiangolo.com/).

## FastAPI

FastAPI is a high-performace web framework for creating APIs with Python. It is light and easy to start, we create a simple API with just a few lines of code

```python
from typing import Optional

from fastapi import FastAPI

app = FastAPI()


@app.get("/")
def read_root():
    return {"Hello": "World"}
```

Start a sever by using **uvicorn main:app --reload**

```console
uvicorn main:app --reload

INFO: Uvicorn running on http://127.0.0.1:8000 (Press CTRL+C to quit)
INFO: Started reloader process [28720]
INFO: Started server process [28722]
INFO: Waiting for application startup.
INFO: Application startup complete.

```

It is so simple and fast to develop which is suitable for nowadays' rapid demands for development. Additionally, regardings its performance it is on par with **NodeJS** and **Go**, [Benchmarks](https://fastapi.tiangolo.com/benchmarks/).

Not only it has incredible speed, but also comes with an auto-generated interactive API document (powered by [swagger-ui](https://github.com/swagger-api/swagger-ui)) which users can input parameters and validate outputs with ease.

{{< image src="https://camo.githubusercontent.com/0a4b5cd30b256fcc8e27189516d1b7efcd119dbf23fb38e4b3b0466a2d8bfc06/68747470733a2f2f666173746170692e7469616e676f6c6f2e636f6d2f696d672f696e6465782f696e6465782d30312d737761676765722d75692d73696d706c652e706e67" alt="FastAPI docs" position="center">}}

And my favourite part is that it works perfectly with [Pydantic](https://github.com/samuelcolvin/pydantic). It just makes data validation and setting management so much fun to handle with proper structures.

```python
from typing import Any, Dict, Optional

from fastapi import FastAPI
from pydantic import BaseModel

app = FastAPI()


class ReponseModel(BaseModel):
    status: int
    data: Optional[Dict[str, Any]] = {}


@app.get("/")
def read_root() -> ReponseModel:
    res = {
        status: 1,
        data: {"Hello": "World"}
    }
    return ReponseModel(**res)
```

## Summary

When it comes to meeting deadline and avoid working overtime, [**FastAPI**](https://fastapi.tiangolo.com/) never disappoints me and my teammates. It is great for developing APIs as proof of concept or even for production. I personally recommend this framework for everyone to give it a try. It is awesome.
