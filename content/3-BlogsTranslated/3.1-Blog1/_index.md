---
title: "Blog 1"
date: "2025-10-04"
weight: 1
chapter: false
pre: " <b> 3.1. </b> "
---

# AWS Open Source Blog
## AWS joins the DocumentDB project to build interoperable, open source document database technology
**by Rashim Gupta on 24 AUG 2025 in Amazon DocumentDB, Announcements, Database, Open Source | Permalink**

At AWS, we design cloud services that give customers the freedom to choose technology that best suits their needs. Our commitment to interoperability with open standards and open source technologies is a key reason customers choose AWS. This is one of the reasons why we launched Amazon DocumentDB (with MongoDB compatibility) in 2019.

Amazon DocumentDB is a serverless, fully managed, MongoDB API-compatible document database service. Amazon DocumentDB serves tens of thousands of customers globally across all industries. For example, United Airlines uses Amazon DocumentDB to modernize their ticket ordering workflow. Capital One uses Amazon DocumentDB for its credit decisioning application. FINRA leveraged Amazon DocumentDB to revamp how regulatory filings are collected on behalf of their customers to support its mission.

Today, AWS joins the open source DocumentDB project under the stewardship of the Linux Foundation, reiterating our commitment to choice and interoperability for customers. Microsoft launched the DocumentDB project in January 2025. Microsoft has moved the project to The Linux Foundation, where it will be independently led and governed. The project aims to provide the developer community with a PostgreSQL-based document database, with complete visibility into the architecture and implementation of the engine. Because the project is open source under the permissive MIT license, developers and organizations alike can migrate existing applications with little to no change or build new applications. AWS has joined the technical steering committee of the Linux Foundation project and will contribute to the further advancement of this important technology.

In this post, we present three reasons why AWS joined the DocumentDB open source project.

**1. MongoDB API compatibility**
First, DocumentDB has a goal to provide 100% compatibility with the MongoDB API. The MongoDB API is the most popular document database API, and we want to see it continue to succeed. By joining this project, we are helping customers access the same compatibility, performance, and functionality, no matter where they run their applications – in AWS, other clouds, on-premises, or locally on their desktops.

**2. Feature innovations**
Second, open source accelerates innovation. With multiple database vendors and cloud providers, including Microsoft and Yugabyte, joining the project, we expect the community to not just close the gap with MongoDB API compatibility but also improve performance, features, and developer experience. Each organization brings their expertise to the table. With multiple companies and contributors supporting this project, customers will be able to leverage the advancements made in the project, while still being compatible with the MongoDB API.

**3. Built on PostgreSQL**
Third, DocumentDB is built on PostgreSQL, which has become the preferred relational database for many enterprise developers and startups, with its reputation for reliability, features, and extensibility that’s backed with nearly 35 years of active development. AWS is recognized as a major contributor to the PostgreSQL open source community, providing contributions to the core database software, extensions, drivers, and project governance. AWS has contributed to PostgreSQL features for high availability, major version upgrades, improved performance for queries and maintenance operations, JDBC driver, extensions like pgvector, and community operations. For example, Trusted Language Extensions (pg_tle) for building extensions on restricted filesystems, and pgactive for added resiliency and flexibility for moving data between two or more active databases. Now, by joining the DocumentDB project, we will bring together our expertise on Amazon DocumentDB and PostgreSQL into improvements on the open source project that will benefit customers.

## Looking ahead
Since its inception, AWS has been the best place for customers to build and run open source software in the cloud. AWS is proud to support open source projects, foundations, and partners. We are committed to bringing the value of open source to our customers, and the operational excellence of AWS to open source communities.

As part of our open source commitment, AWS has a long history of contributing to open source projects. For example, we have large number of contributions to Lucene, Valkey, containerd, PostgreSQL, Rust, and OpenSearch. We’ve learned a lot from those experiences, including the importance of getting involved and contributing upstream to the open source projects that we build on and that our customers depend on. AWS remains committed to DocumentDB for the long term because we firmly believe in the power of open source to increase innovation. Similarly to Valkey and OpenSearch, we will continue to bring the innovations we have built in Amazon DocumentDB to the open source DocumentDB project so that customers can continue to take advantage of those capabilities, no matter where they choose to run their software.

While the DocumentDB project, stewarded by Linux Foundation, has a similar name to Amazon DocumentDB, they use different software under the hood. Amazon DocumentDB is a MongoDB API-compatible document database that is built by AWS. The project, stewarded by Linux Foundation, on the other hand, while also being MongoDB compatible, uses an open source engine that is built as an extension on PostgreSQL. This is a different engine than the one used in Amazon DocumentDB. AWS will continue to invest in both Amazon DocumentDB and open source DocumentDB akin to how we invest in Amazon OpenSearch Service and OpenSearch. Moving forward, we will start contributing Amazon DocumentDB innovations to the open source project, and adopt features and capabilities from the open source DocumentDB engine to our managed Amazon DocumentDB service over time. We will announce these changes in our What’s New posts over the coming months.

> “It’s great that Microsoft, AWS, and others are joining forces to work on DocumentDB, an open source implementation of a MongoDB-compatible API on top of PostgreSQL. Microsoft and AWS already work together to enhance Postgres, so it is logical they would use the high-quality Postgres source code and leverage its extensibility to meet the need for an open source document database,” said Bruce Momjian, founding member of the PostgreSQL core development team. “The idea of using Postgres in this way has been around for a long time so I am glad it is now getting serious traction. DocumentDB should be an interesting alternative to users wanting an open source implementation, and for database users who just want a simpler interface to PostgreSQL.”

> “DocumentDB fills a critical gap in the document database ecosystem, attracting contributors, users and champions. What’s even more exciting is it provides an open standard for document based applications, like what SQL did for relational databases,” said Jim Zemlin, executive director of the Linux Foundation. “By joining the Linux Foundation, DocumentDB is securing its open source future and helping chart a new path for NoSQL database standards and community-driven innovation.”

We are excited to contribute to DocumentDB project as one of many stakeholders. Please join us on GitHub to continue open source development on DocumentDB. You can also visit our website here to learn more and get involved.

**Rashim Gupta**  
Rashim Gupta is a Senior Manager, Product Management at AWS. Over the last seven years, Rashim has led product for multiple database services at AWS. He currently leads the PM teams for Amazon DocumentDB, Amazon Timestream, and Amazon Neptune. Prior to his current role, he led the launch for Valkey in Amazon ElastiCache and Amazon MemoryDB. He has also worked on storage and compute services in AWS, analytics in Microsoft Azure, and developer experience at Meta.
