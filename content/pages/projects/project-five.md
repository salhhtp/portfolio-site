---
type: ProjectLayout
title: 'CommerceFlow: A Microservices-Based E-Commerce Platform'
date: '2025-03-12'
client: ''
description: >-
  CommerceFlow is a production-grade portfolio project built with TypeScript,
  NestJS, and Docker. It showcases my expertise in backend development and
  system architecture by leveraging a microservices-based design. This project
  demonstrates my ability to design scalable systems, implement robust testing
  strategies, and integrate CI/CD pipelines—all while writing clean,
  maintainable code.
featuredImage:
  type: ImageBlock
  url: 'https://assets.stackbit.com/components/images/default/post-4.jpeg'
  altText: Project thumbnail image
  caption: ''
  elementId: ''
media:
  type: ImageBlock
  url: 'https://assets.stackbit.com/components/images/default/post-4.jpeg'
  altText: Project image
  caption: Caption of the image
  elementId: ''
addTitleSuffix: true
colors: colors-a
backgroundImage:
  type: BackgroundImage
  url: /images/bg2.jpg
  backgroundSize: cover
  backgroundPosition: center
  backgroundRepeat: no-repeat
  opacity: 100
---
## Table of Contents

- [Project Overview](#project-overview)
- [Architecture and Design](#architecture-and-design)
- [Technical Stack](#technical-stack)
- [Services Overview](#services-overview)
  - [Catalog Service](#catalog-service)
  - [Order Service](#order-service)
  - [API Gateway](#api-gateway)
- [Code Samples](#code-samples)
- [Automated Testing](#automated-testing)
- [CI/CD Pipeline](#cicd-pipeline)
- [How to Run Locally](#how-to-run-locally)
- [Repository Link](#repository-link)
- [Visual Assets and Presentation Suggestions](#visual-assets-and-presentation-suggestions)
- [Conclusion](#conclusion)

---

## Project Overview

**CommerceFlow** is built to exemplify best practices in AI and backend development. By leveraging a microservices architecture, the project isolates business domains into separate services, each responsible for a distinct part of the system. An API Gateway consolidates requests and routes them to the proper service, allowing for dynamic request handling and centralized cross-cutting concerns.

**Key Highlights:**
- **Modular Microservices:** Each service is independently developed in TypeScript using NestJS.
- **Dynamic API Gateway:** Acts as a reverse proxy to forward requests based on URL paths.
- **Robust Automated Testing:** Extensive end-to-end and integration tests using Jest and Supertest.
- **Containerization:** All services are containerized with Docker and orchestrated with Docker Compose.
- **Continuous Integration:** GitHub Actions ensures code quality with automated builds and tests.

---

## Architecture and Design

CommerceFlow is designed using a microservices architecture that includes the following components:

- **Catalog Service:**  
  Manages product data (e.g., product creation, retrieval) using an in-memory store.
  
- **Order Service:**  
  Handles order processing with endpoints for creating and fetching orders.

- **API Gateway:**  
  Serves as the unified entry point, dynamically routing requests to the appropriate service. It supports multiple HTTP methods and uses environment variables to manage service endpoints.

The architecture promotes scalability, separation of concerns, and ease of maintenance.

### Architecture Diagram

> **Suggestion:** Create and include a clean diagram using Lucidchart, Draw.io, or a similar tool that visualizes:
> - The API Gateway at the center.
> - The Catalog Service and Order Service as separate nodes.
> - Arrows showing request routing from clients to the gateway and from the gateway to each service.

---

## Technical Stack

- **Language:** TypeScript
- **Framework:** NestJS
- **Testing:** Jest, Supertest
- **Containerization:** Docker, Docker Compose
- **CI/CD:** GitHub Actions
- **Architectural Patterns:** Microservices, API Gateway

---

## Services Overview

### Catalog Service

- **Port:** `3001`
- **Functionality:**
  - **GET /products:** List all products.
  - **POST /products:** Create a new product.
  - **GET /products/:id:** Retrieve a specific product.
- **Data Storage:** In-memory store (for portfolio demonstration).

### Order Service

- **Port:** `3002`
- **Functionality:**
  - **GET /orders:** List all orders.
  - **POST /orders:** Create a new order.
  - **GET /orders/:id:** Retrieve a specific order.
- **Data Storage:** In-memory store.

### API Gateway

- **Port:** `3000`
- **Functionality:**
  - Routes `/catalog/*` requests to the Catalog Service.
  - Routes `/orders/*` requests to the Order Service.
  - Supports GET, POST, PUT, DELETE methods.
- **Dynamic Configuration:** Uses environment variables to determine target hosts, making it flexible for Docker or local development.

---

## Code Samples

### API Gateway Controller (gateway/src/gateway.controller.ts)

```typescript
import { Controller, All, Req, Res } from '@nestjs/common';
import { Request, Response } from 'express';
import axios, { AxiosResponse } from 'axios';

@Controller()
export class GatewayController {
  @All('*')
  async proxy(@Req() req: Request, @Res() res: Response) {
    const originalUrl = req.originalUrl;
    let targetUrl = '';

    // Use environment variables with defaults
    const catalogHost = process.env.CATALOG_SERVICE_HOST || 'catalog-service';
    const orderHost = process.env.ORDER_SERVICE_HOST || 'order-service';

    if (originalUrl.startsWith('/catalog')) {
      targetUrl = `http://${catalogHost}:3001${originalUrl.replace('/catalog', '')}`;
    } else if (originalUrl.startsWith('/orders')) {
      targetUrl = `http://${orderHost}:3002${originalUrl.replace('/orders', '')}`;
    } else {
      return res.status(404).send('Not Found');
    }

    try {
      let axiosResponse: AxiosResponse;
      const method = req.method.toLowerCase();

      if (method === 'get') {
        axiosResponse = await axios.get(targetUrl);
      } else if (method === 'post') {
        axiosResponse = await axios.post(targetUrl, req.body);
      } else if (method === 'put') {
        axiosResponse = await axios.put(targetUrl, req.body);
      } else if (method === 'delete') {
        axiosResponse = await axios.delete(targetUrl);
      } else {
        throw new Error(`Method ${req.method} not supported by the gateway`);
      }

      res.status(axiosResponse.status).send(axiosResponse.data);
    } catch (error: any) {
      res.status(error.response?.status || 500).send(error.response?.data || error.message);
    }
  }
}