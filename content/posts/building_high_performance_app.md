---
title: "Building High-Performance Application with FastAPI Redis Celery Flower Docker"
date: 2024-10-14T11:33:48+08:00
draft: false
categories: ["blog"]
tags: ["blog", "web-app", "fastapi", "redis", "celery", "flower", "docker"]
---

## Introduction

In the world of modern web applications, speed and scalability are critical. Sometimes, tasks like sending emails, processing reports, or running machine learning models need to be offloaded from the main API to avoid slowing down user interactions. This is where the FastAPI + Redis + Celery + Flower stack comes in—a powerful setup that enables asynchronous processing and real-time monitoring of background tasks.

This article will walk you through how these technologies work together and provide a guide for building your own high-performance backend using this stack.

## What is the FastAPI + Redis + Celery + Flower Stack?

Let’s break down the components of this stack:

- FastAPI: A modern Python web framework known for its speed, asynchronous capabilities, and automatic data validation.

- Redis: An in-memory data store used as a message broker and result backend. Redis facilitates task communication between FastAPI and Celery.

- Celery: A distributed task queue for running background jobs asynchronously.

- Flower: A web-based monitoring tool that provides real-time insights into Celery task execution, worker health, and error tracking.

Together, these components enable you to build an API that responds quickly while delegating long-running or resource-intensive tasks to Celery workers running in the background.

## How Does the Stack Work?

{{< image src="<https://i.ibb.co/gWgjhWq/high-app-diagram.jpg>" alt="stack-diagram" position="center">}}

1. FastAPI receives an API request that triggers a task (e.g., sending an email).

2. The task is handed over to Celery via Redis (acting as a message broker).

3. Celery workers execute the task asynchronously to prevent the API from blocking.

4. Redis stores the task’s state and results, allowing for easy tracking.

5. Flower monitors task execution and provides a web-based dashboard for insights.

## How to Build a FastAPI + Redis + Celery + Flower Application

### Step 1: Install Dependencies

install all required dependencies:

```bash
pip install fastapi uvicorn celery redis flower
```

### Step 2: Set Up the Project Structure

```bash
mkdir fastapi_celery_app
cd fastapi_celery_app
touch main.py celery_worker.py tasks.py
```

The structure will look like this:

```bash
/fastapi_celery_app
├── docker-compose.yml  # Docker Compose configuration
├── Dockerfile          # FastAPI app Docker image configuration
├── main.py             # FastAPI routes
├── celery_worker.py    # Celery configuration
├── tasks.py            # Background tasks
└── requirements.txt    # Python dependencies
```

### Step 3: Configure Celery (celery_worker.py)

```python
from celery import Celery

# Initialize Celery with Redis as broker and backend
celery_app = Celery(
    'worker',
    broker='redis://localhost:6379/0',
    backend='redis://localhost:6379/0'
)
```

This configuration ensures that Celery uses Redis for task queuing and stores results in Redis for later retrieval.

### Step 4: Define Background Tasks (tasks.py)

```python
from .celery_worker import celery_app
import time

@celery_app.task
def sample_task(duration: int):
    """Simulate a time-consuming task."""
    time.sleep(duration)
    return f'Task completed in {duration} seconds!'
```

This task simulates a long-running process by sleeping for a given number of seconds.

### Step 5: Create FastAPI Routes (main.py)

```python
from fastapi import FastAPI, BackgroundTasks
from .tasks import sample_task

app = FastAPI()

@app.get('/run-task/')
def run_task(duration: int):
    """Submit a background task via Celery."""
    task = sample_task.apply_async(args=[duration])
    return {'task_id': task.id, 'status': 'Task submitted'}


@app.get('/task-status/{task_id}'')
def get_task_status(task_id: str):
    """Check the status of a specific task."""
    task = sample_task.AsyncResult(task_id)
    if task.state == 'SUCCESS':
        return {'status': task.state, 'result': task.result}
    return {'status': task.state}
```

These routes allow users to trigger a background task and check its status by task ID.

### Step 6: Create Docker Configuration

#### Dockerfile

```dockerfile
# Use official Python image
FROM python:3.9-slim

# Set the working directory
WORKDIR /app

# Copy requirements and install dependencies
COPY requirements.txt .
RUN pip install -r requirements.txt

# Copy the app code
COPY . .

# Run the FastAPI application
CMD ["uvicorn", "main:app", "--host", "0.0.0.0", "--port", "8000", "--reload"]
```

#### Docker-compose (docker-compose.yml)

```yaml
version: '3.8'

services:
  redis:
    image: redis:alpine
    ports:
      - "6379:6379"

  backend:
    build: .
    command: uvicorn main:app --host 0.0.0.0 --port 8000 --reload
    ports:
      - "8000:8000"
    depends_on:
      - redis
      - celery_worker

  celery_worker:
    build: .
    command: celery -A celery_worker.celery_app worker --loglevel=info
    depends_on:
      - redis

  flower:
    image: mher/flower
    command: flower --broker=redis://redis:6379/0
    ports:
      - "5555:5555"
    depends_on:
      - redis
```

### Step 7: Build and Run the Application with Docker-Compose

1. Build and start the containers

```bash
docker-compose up --build
```

2. Check if all services are running

- FastAPI: <http://localhost:8000>

- Flower: <http://localhost:5555>

### Step 8: Testing the Application

1. Submit a Task:

```bash
GET http://localhost:8000/run-task/?duration=5
```

This request sends a task to Celery, and the response will include a task ID.

2. Check Task Status:

Use the task ID from the previous step:

```bash
GET http://localhost:8000/task-status/{task_id}
```

3. Monitor Tasks with Flower:

Visit <http://localhost:5555> to view task statuses and worker health in real-time.

{{< image src="<https://i.ibb.co/p3XDY6F/flower.jpg>" alt="flower" position="center">}}

## Use Cases of This Stack

- Email Notifications: Send emails in the background without blocking API responses.

- Report Generation: Create PDF or Excel reports asynchronously.

- Machine Learning Inference: Run ML models in Celery workers for fast, non-blocking API requests.

- Data Pipelines: Process large datasets in background tasks with ease.

## Conclusion

The FastAPI + Redis + Celery + Flower stack with Docker-Compose is a powerful solution for building scalable, asynchronous applications. It ensures that your APIs remain fast and responsive by delegating long-running tasks to Celery workers. With Redis handling message brokering, Flower monitoring the tasks, and Docker-Compose managing the services, this stack becomes easy to deploy and maintain.

Try this setup for your next project! Let me know how it goes.
