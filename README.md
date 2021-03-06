# Juxt

Juxt is a platform I am building that aims to automate fundamental and technical analysis of publicly traded companies, and allows independent analysts to share ideas.

Users of this platform have the ability to create watchlists to receive notifications for recent updates, read blog posts from other users, perform technical analysis with a comprehensive charting feature, and more.

Automated analysis is available with a subscription, and will provide insight on market sentiment, potentially profitable trading strategies, and analytics from  ML models trained on historical market conditions.

### Service Architecture

_Arrows denote the flow of data._

![Service Architecture](https://github.com/pererasys/juxt/blob/main/resources/architecture.png?raw=true)

This application follows microservice methodologies which allows me to easily implement and test new features without the hassel of maintaining a monolithic codebase. While the Market and Blog API's could likely be joined with the GraphQL/Core service, my Kafka consumers will be interacting with these services and I didn't want to go through the GraphQL layer to make this happen. I found it easier to create separate services for these interactions, so I've moved all the business logic to REST API's.

### Tech Stack

| **Client**    | **GraphQL**   | **Market API & Blog API** | **Real-time Analysis (Kafka Consumers & Producers w/REST API)** |
| :------------ | :------------ | :------------------------ | :-------------------------------------------------------------- |
| NextJS        | Apollo Server | ExpressJS                 | Faust                                                           |
| Apollo Client | MongoDB       | MongoDB                   | RocksDB                                                         |
| TypeScript    | TypeScript    | TypeScript                | Python                                                          |

**Some notes**

This are my currently implemented tech stack. For some of the analysis I have been experimenting with Neo4j and ElasticSearch, so some aspects of this are likely to change.

### Testing

| **Client**        | **GraphQL**                                 | **Market API & Blog API**                   | **Real-time Analysis (Kafka Consumers & Producers w/REST API)** |
| :--------- | :------------------------------------------ | :------------------------------------------ | :-------------------------------------------------------------- |
| Jest       | Jest (mocking)                              | Jest (mocking & API testing)                | Faust's built-in Agent tester                                   |
| testing-library (DOM testing)  | mongodb-memory-server (service integration) | mongodb-memory-server (service integration) | pytest (mocking)                                                |
|            | apollo-server-testing (graphql integration) |                                             |                                                                 |


### CI/CD

Currently the CI process is handled with CircleCI. CircleCI is a CI/CD tool which automates testing, building, and deployment for each of the services.

### Monitoring & Logging

Right now I've done an initial monitoring/logging setup in the GraphQL service using Sematext. I've found that their platform is very easy to use and setup, and I should be able to use it with all of my various services once I begin Alpha testing.

### Deployment

I am currently considering my options for deployment. I am going to be using AWS resources, so some options I am considering include: EKS, ECS, MongoDB Atlas, Neo4j Aura, Elasticsearch, MSK, Neptune, and more.
