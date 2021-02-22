---
title: "HABOOK Databank"
date: 2019-09-03T15:38:09+08:00
draft: false
categories: ["projects"]
tags: ["project", "python", "databank"]
---

{{< image src="/img/projects/habook_databank/logo.jpg" alt="HABOOK DATABANK LOGO" position="center">}}

**HABOOK DataBank** is an in-house web application that simplifies Exploratory Data Analysis for school datasets.
It provides a user with the information that stakeholder need to know. It, not only, provides stakeholders with sortable tabular data,
it also visualises the data to graphs (e.g., heatmaps, linecharts, scatterplots).

```console
** NOTE: The data is altered and left out for confidentiality
```

## How it works

HABOOK DataBank explores the data in the database, then performs data pre-processing. It then returns a ready-to-use data in a form of JSON format. Visualized data are transformed to HTML strings and embedded to JSON as well.

By performing such task, in this case, stakeholders are able to see the progress of product sales. Moreover, it allows school principals to track the progress of teachers regarding their performance (Technological Interaction and Pedagogical Application) as well as the functions used in the system.

## Features

### Get To Know Your Teachers

{{< image src="/img/projects/habook_databank/1.png" alt="pic 1" position="center">}}

With a HABOOK DataBank's feature, a pricipal is able to explore a variety of data. This features allows the principal to briefly glance at teachers' progress and their contributions. In this way, it is clearly be seen that who is frequently contributing the most, and who needs guidance.

Furthermore, this allows a principal see teachers' progress individually in which descriptive statistics is provided in a tabular form as well as visualization.

### Enable Insight To Be Seen

{{< image src="/img/projects/habook_databank/2.png" alt="pic 2" position="center">}}

By looking deeply into details, teachers' **Technological** and **Pedagogical** progress can be clearly observed. Therefore, this piece of information allows the principal to manage, plan, and envision school, classes more effectively.

### Observe the Growth and Data Presentation

{{< image src="/img/projects/habook_databank/3.png" alt="pic 3" position="center">}}

By observing visualised data, the principal is able to see the progress of the overall usage through the years.

The data is both presented in a tabular form and graphs. Moreover, it is validated during the data pre-processing. However, if the data kept in the database is inconsistent or incorrect, the presentations can be slightly less accurate.
