# # Multi-tenancy

The people living in a gated community could be easily related with the term multi-tenancy. We will be having multiple families living in their own flat/home (a user) within a building/phase (a tenant); together, they will form a gated community that is nothing but multi tenancy. In software terminology, a system supporting a set of users grouped together as a tenant (based on roles) would have their data stored in a single or shared database, and still serving data based on the tenant is called multi-tenancy.

The multi-tenancy application can be classified as follows:

+   - Standalone
+   - Database-per-tenant
+   - Shared multi-tenant

We will explore each one of the applications individually in the next sub-sections.

# # Standalone

The standalone model deals with an individual application and its own database, isolating everything from different tenants, right from database to the application. It helps the developers to customize the application and have different schema for different customers, which will be widely used in product-based companies. Nowadays, companies were drifting toward this approach due to microservices architecture. Also, the need arises based on the volume of the transactions and the sensitivity of data.

The following diagram explains the standalone model, where each application has its own database: