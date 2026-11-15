
## VPS Hosting Costs

The monthly cost mostly depends on the amount of CPU, memory, storage, and bandwidth you choose, as shown in *Figure 15.2*:

![](Images/B34553_15_02.png)

Figure 15.2: Typical monthly costs for a VPS

You should be able to find a VPS hosting provider for about US$5-10 per month, which will be enough to have a proper web server to host your Django projects. But to get lower costs, you often have to commit to one or more years of hosting, so expect to pay $60-$120 for a year of hosting. Many readers may prefer just to host locally and never actually deploy to a public server.

**Prompt**: What are the trade-offs between VPS, PaaS, and serverless for a Django app?

## SQLite as the Database

People often say SQLite is not suitable for production, but that is too broad. SQLite is a file-based database that excels at simplicity and reliability, but it has limits around concurrent writes.

For TallyApp, SQLite is a reasonable choice if:

* Traffic is modest
* Database writes are short-lived
* The application runs with a small number of worker processes
* Write-ahead logging (`WAL`) is enabled

The database itself is just a file on disk. Django reads and writes it using the SQLite library. There is no database server to install or manage.

**Prompt**: Please explain write-ahead logging (`WAL`) in SQLite and why it matters for concurrency. At what point does SQLite become a bottleneck in a production Django app?

### Database Persistence and Migrations

One of the most important deployment rules is this: *never treat your database as disposable*.

Your application code can be replaced at any time. Your database must survive deployments.

To achieve this:

* Store the SQLite database file outside the application code directory
* Update code separately from data
* Use Django migrations to evolve the database schema

When you deploy a new version of TallyApp, you run migrations against the existing database. Data remains intact while the schema changes incrementally. This mirrors how migrations work with larger databases such as PostgreSQL. The commands are similar even though the database engine is different.

### Backing Up Your Data

Even for a learning project, backups matter. Users will assume their survey responses persist.

SQLite provides built-in mechanisms for creating consistent backups of a live database. A typical approach is:

* Create a snapshot using SQLite’s backup facilities
* Store the snapshot in a dated backup directory
* Optionally copy it off the server

This can be automated with a daily scheduled task. The key idea is consistency. Always use SQLite’s own tools to create backups rather than copying the database file blindly, especially if the application may be running while the backup is taken.

### Updating the Site Safely

A typical update cycle for TallyApp looks like this:

1. Upload or pull the latest code
2. Install or update dependencies
3. Apply migrations
4. Collect static files
5. Restart the application service

Because data lives outside the code directory, updates are routine and low-risk. This is an important mental shift from local development, where deleting and recreating databases is common.

### When to Move to PostgreSQL

When traffic grows or writes become frequent and unpredictable, SQLite will start to show its limits. This is not a failure. It is simply the point at which the application has outgrown a file-based database.

The next step is usually PostgreSQL running on the same VPS. The architecture remains almost identical. Nginx and Gunicorn stay the same. Django stays the same. Only the database backend changes.

This is one of Django’s strengths. Database backends are swappable with minimal application changes if you have followed best practices.

**Prompt**: Please walk me through migrating a Django project from SQLite to PostgreSQL safely.

### Scaling Further with a Managed Database

Beyond a single VPS, databases are often moved to managed services. At that point:

* Application servers can be scaled horizontally
* Databases gain automated backups and replication
* Operational complexity shifts from you to the provider

TallyApp does not need this level of infrastructure, but understanding the path matters. Your Django project can grow with your needs.

## Summary of Deployment Options

Deploying TallyApp to the internet does not require exotic tools or complex cloud services. A small VPS, a conventional web stack, and a disciplined approach to data persistence are enough.

More importantly, this chapter establishes patterns that will serve you well beyond this project. The same principles apply whether you are deploying a learning app, a startup prototype, or a production service with real users and real expectations.

With a clear deployment approach in place, the next step is to ensure that your application remains secure and resilient once it is exposed to the unpredictable environment of the public internet.

# Protecting a Django Website from Common Attacks

Once your site is live on the public internet, it will start receiving traffic from sources you did not invite. Some will be legitimate users. Many will be automated bots scanning for weaknesses. This is normal.

For a beginner Django project like TallyApp, the goal is not to eliminate all risk. That is unrealistic. The goal is to avoid obvious mistakes, reduce the impact of common attacks, and make the site resilient enough that it keeps working even under nuisance traffic.

Django already includes many built-in protections, but they only help if you use them correctly and deploy your application with care.

You might make the following assumptions:

* **People will try to bring your site down**: Partly true, but not in a targeted way. Most traffic will be automated bots. The architecture above handles that well.
* **A single server might be fragile**: Correct. A single Droplet is a single point of failure. But for a demo site, that is acceptable if you have backups and can redeploy quickly.
* **You need strong protections from day one**: Not really. You need basic Django settings and server protections, not heavy infrastructure. The combination of Django best practices + Cloudflare + basic server hardening covers most real risks.

## Understanding the Most Common Attacks

Most attacks against small web applications are not sophisticated. They are automated and repetitive, looking for easy wins.

The most relevant categories for a Django app are shown in *Table 15.1*:

| Name | Description |
|------|-------------|
| **Cross-site scripting (XSS)** | Attackers try to inject malicious JavaScript into pages viewed by other users. Django templates escape output by default, which blocks most simple cases. Problems usually arise when developers deliberately bypass escaping. |
| **Cross-site request forgery (CSRF)** | A user is tricked into submitting a request they did not intend, such as changing account details. Django includes CSRF protection middleware, but it must be enabled and used correctly in forms. |
| **SQL injection** | Malicious input attempts to alter database queries. Django’s object-relational mapper (`ORM`) protects against this by using parameterized queries, but unsafe raw SQL can reintroduce the risk. |
| **Authentication abuse** | Bots attempt to guess passwords or reuse leaked credentials. Login pages are a common target. |
