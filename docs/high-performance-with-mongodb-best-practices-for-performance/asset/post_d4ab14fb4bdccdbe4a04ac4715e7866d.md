
## Chapter 14: Monitoring and Observability 297
Key differences between monitoring and observability ................................................ 297

Core MongoDB metrics and signals ................................................ 299

Operational metrics • 299

Performance-specific metrics • 301

Monitoring with MongoDB Atlas ................................................ 301

Atlas UI features • 301

Real-Time Performance Panel (RTPP) • 302

Performance Advisor • 303

Query Insights • 303

Alerts • 304

Atlas Search issues • 305

Connection issues • 305

Query issues • 306

Oplog issues • 306

Self-managed monitoring tools ................................................ 307

mongostat: real-time activity snapshot • 307

mongotop: Collection-level read/write timings • 308

serverStatus: Comprehensive metrics via the shell • 309

Database profiler: Deep dive into slow operations • 310

Integration with external monitoring systems ................................................ 312

Prometheus + Grafana • 313

Atlas integration for Prometheus • 313

Visualizing with Grafana • 314

Datadog • 315

Application performance monitoring platforms ................................................ 316

OpenTelemetry • 317

Considerations for external tools • 319

### Common performance patterns and what to monitor ........................................ 320
Disk I/O bottlenecks • 320

Cache pressure (WiredTiger cache utilization) • 321

Replication lag and oplog window drift • 322

### Summary ........................................ 324

## Chapter 15: Debugging Performance Issues 325

### Identifying inherently slow operations ........................................ 326
Case study: Troubleshooting cluster performance with Atlas • 326

Diagnosing the cause • 327

Root cause identification • 328

Implementing the solution • 331

Results and learnings • 332

Case study: Unexpected admin query causing collection scan • 332

Diagnosing the cause • 333

Implementing the solution • 334

Results and learnings • 334

### Managing blocked operations ........................................ 335
Case study: Cursor leak investigation • 335

Diagnosing the cause • 335

Root cause identification • 337

Implementing the solution • 337

Results and learnings • 338

Use case: Burst of poorly optimized queries • 339

Diagnosing the cause • 340

Implementing the solution • 341

Results and learnings • 342

Addressing hardware resource constraints ........................................................ 342

Case study: Atlas cluster resource optimization • 343

Diagnosing the cause • 344

Implementing the solution • 345

Results and learnings • 348

Use case: Diagnosing insufficient IOPs in self-managed MongoDB • 349

Diagnosing the cause • 349

Implementing the solution • 349

Results and learnings • 350

Systematic approach to performance troubleshooting ........................................................ 350

Summary ........................................................ 352

## Afterword 353
Key takeaways ........................................................ 353

The performance mindset ........................................................ 354

Practical next steps ........................................................ 354

Future considerations ........................................................ 355

Final thoughts ........................................................ 355

## Index 357

## Other Books You May Enjoy 369

## Preface
In today’s data-driven world, performance is not just a feature; it’s a necessity. As data volumes grow exponentially and user expectations for speed and reliability increase, the ability to build and maintain high-performance database systems has become crucial for competitive applications. MongoDB is a popular, powerful database built for modern application needs. It stores data in documents that align naturally with the data structures used in programming languages. Its distributed architecture provides built-in scalability, high availability, and performance for mission-critical workloads. By understanding MongoDB’s architecture, operational patterns, and optimization techniques, you can fully harness its performance potential to build fast, scalable, and resilient applications with ease.

This book offers a practical guide to understanding and improving MongoDB performance. It covers how to identify the most common performance issues, analyze query and system behavior using built-in tools, and apply proven techniques for optimization. Topics range from schema design and indexing strategies to tuning storage settings and managing system resources. Whether you’re running a single replica set or operating a large sharded cluster, the book aims to help you make informed decisions that improve stability and efficiency in real-world deployments.

### How this book will help you
This book delves into both fundamental concepts and advanced optimization techniques, providing you with the knowledge and practical tools to design, build, and operate high-performance MongoDB deployments at any scale.

Whether you’re troubleshooting a slow query, designing schemas for a new application, or scaling a large cluster to handle growing workloads, this book serves as your trusted resource. The goal of this book is to demystify MongoDB performance optimization and equip you with strategies that work in real-world environments.

The chapters are designed to be modular, allowing you to focus on specific areas of interest, whether it’s fine-tuning indexes, resolving slow queries, or configuring system resources. However, those new to performance tuning may benefit from reading linearly to build a solid conceptual foundation. While some topics naturally build on earlier chapters, most sections are designed to be understandable on their own. This makes the book flexible as both a continuous read and a reference guide for solving specific performance issues as they arise. Throughout the book, you’ll find practical examples, diagnostic techniques, and code snippets that you can adapt to your environment for hands-on learning.

### Who this book is for
This book is written for developers, system architects, DevOps engineers, and database administrators who are serious about MongoDB performance optimization. Whether you’re building data-intensive applications, responsible for scaling and managing MongoDB in production, designing distributed systems for future growth, or integrating MongoDB into CI/CD pipelines, you’ll find practical guidance tailored to your needs. An understanding of MongoDB basics is assumed, and while experience with schema design or database operations is helpful, each chapter builds progressively to support learners at all levels.

### What this book covers
- Chapter 1, Systems and MongoDB Architecture, introduces core systems thinking concepts and shows how they apply to MongoDB’s architecture, performance tuning, and identifying bottlenecks.
- Chapter 2, Schema Design for Performance, covers how document modeling choices directly affect read and write efficiency, one of the most critical aspects of MongoDB performance.
- Chapter 3, Indexes, dives deep into indexing strategies, the primary tool for optimizing query performance.
- Chapter 4, Aggregations, shows you how to use the aggregation framework to perform complex data analysis efficiently.
- Chapter 5, Replication, discusses MongoDB’s replication mechanism and how to leverage it effectively for high availability.
- Chapter 6, Sharding, explains horizontal scaling techniques to distribute data and workloads across multiple servers.
- Chapter 7, Storage Engines, focuses on the WiredTiger, MongoDB storage engine and explains its configuration options and performance characteristics.
- Chapter 8, Change Streams, shows how to build reactive systems with real-time data change notifications.
- Chapter 9, Transactions, covers multi-document ACID transactions, their use cases, and their performance implications.
- Chapter 10, Client Libraries, examines how to configure and use MongoDB drivers for optimal application performance.
- Chapter 11, Managing Connections and Network Performance, details connection pooling strategies and network optimization techniques.
- Chapter 12, Advanced Query and Indexing Concepts, explores advanced indexing topics and MongoDB Query Language (MQL) best practices.
- Chapter 13, Operating System and System Resources, provides guidance on configuring your operating system and hardware for MongoDB workloads.
- Chapter 14, Monitoring and Observability, provides a comprehensive guide to monitoring your MongoDB deployment and establishing proactive performance management.
- Chapter 15, Debugging Performance Issues, equips you with methodologies to identify, diagnose, and resolve performance bottlenecks in production.

### To get the most out of this book
You will require the following software:

| Software covered in the book | Operating system requirements |
|---|---|
| MongoDB version 8.0 | Windows, macOS, or Linux |
| MongoDB Atlas Search | Windows, macOS, or Linux |
| MongoDB Shell | Windows, macOS, or Linux |

After reading this book, we encourage you to check out some of the other resources available at https://www.mongodb.com/developer and https://learn.mongodb.com/.
