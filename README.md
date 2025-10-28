# Building a Fully Automated AWS CI/CD Pipeline for Node.js — From Scratch to Success

> **A real-world DevOps journey**: Overcoming `ScriptFailed`, `EACCES`, and permission hell to deploy a Node.js app using **AWS CodePipeline, CodeBuild, CodeDeploy, S3, EC2, and GitHub**.

---

![AWS CI/CD Pipeline](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/8x1i1w3q3k4z5v7t9p0r.png)
*My final pipeline in action — Source → Build → Deploy*

---

## Introduction

In this project, I built a **production-grade CI/CD pipeline** for a **Node.js application** using **only AWS native services**:

- **GitHub** → Source
- **AWS CodeBuild** → Build
- **Amazon S3** → Artifact Storage
- **AWS CodeDeploy** → Deploy
- **AWS CodePipeline** → Orchestration
- **Amazon EC2** → Target Server

The app is a simple Express server that returns `"Hello from AWS CI/CD!"` on `port 3000`.

But this wasn’t a smooth ride. I faced **repeated `ScriptFailed` errors**, **`EACCES` permission issues**, and **artifacts with unwanted files**. This blog is the **complete story** — including **IAM roles**, **configuration files**, **debugging**, and **final success**.

---

## Project Structure
