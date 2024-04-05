---
title: "ImpURL"
date: 2024-04-25T10:57:13+08:00
draft: false
categories: ["projects"]
tags: ["project", "python", "web-app", "vue.js", "fastapi"]
---

{{< image src="<https://i.ibb.co/HYLVVy9/impurl.jpg>" alt="impurl" position="center">}}

## Introduction

[ImpURL](https://impurl.nuttaphat.com/) is an experimental and user-friendly web application designed to help you shorten URL links efficiently. In today's fast-paced digital world, the ability to quickly share concise links can streamline communication and improve productivity.

## Inspiration

The primary motivation behind creating ImpURL was to develop a practical web application and CI/CD pipeline connecting GitHub, Azure, and Netlify seamlessly. By integrating these platforms, ImpURL aims to provide users with a reliable and efficient solution for URL shortening.

## Key Features

### API

ImpURL boasts an API server powered by FastAPI, ensuring robustness and high performance. The API allows users to programmatically shorten URLs, retrieve analytics, and manage their shortened links with ease.

### UI

The intuitive and visually appealing web user interface of ImpURL is built on Nuxt3, providing users with a smooth and enjoyable experience. With a clean design and intuitive navigation, users can shorten URLs effortlessly and access advanced features with minimal effort.

### CI/CD Pipeline

{{< image src="<https://i.ibb.co/1KKXP7q/impurl-drawio.png>" alt="CICD" position="center">}}

ImpURL implements a comprehensive CI/CD pipeline to streamline the development and deployment process:

- **Local Development Environment:** Developers can work efficiently in a controlled local environment.
- **GitHub Integration:** Code is hosted on GitHub, enabling collaborative development and version control.
- **Automated Build and Test Processes:** GitHub Actions trigger automated build and test processes, ensuring code quality and reliability.
- **Containerization with Docker:** Docker container images are stored on GitHub Container Registry, simplifying deployment and scalability.
- **Deployment to Azure Web App:** The application is deployed to Azure Web App for reliable hosting and scalability.
- **Deployment to Netlify:** Frontend assets are deployed to Netlify for optimized performance and global accessibility.

## Project Links

- [Visit ImpURL](https://impurl.nuttaphat.com/)
- [ImpURL on Netlify](https://impurl.netlify.com/)

## Conclusion

ImpURL offers a seamless solution for URL shortening, combining powerful backend functionality with an intuitive user interface. Whether you're sharing links on social media, optimizing marketing campaigns, or managing URLs for personal use, ImpURL provides the tools you need to streamline your workflow. Try it out today and experience the convenience of efficient URL management.
