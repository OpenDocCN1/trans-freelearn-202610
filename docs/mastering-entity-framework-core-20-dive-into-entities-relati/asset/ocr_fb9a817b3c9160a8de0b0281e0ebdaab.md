The same approach could be applied on `GetPostByUrlQuery` and `GetPaginatedPostQuery` to render posts anonymously.

The cross-tenant update is handled using multi-tenant implementation with data context validation in this section. This makes the behavior consistent with the implementation we performed in the row-level security section as well.

# Summary

We started with authentication implementation, which is the foundation for row-level security and multi-tenancy. Then, we explored row-level security and how we could manage them using filter and block predicates. Finally, we started exploring multi-tenancy and its different models and deep dived into the implementation in our blogging system. We have come to a conclusion in our EF Core journey, along with building a complete ecosystem for the blogging system.

We had explored Entity Framework Core by building a blogging system, the features were included in the system with the intention of covering topics. So ideally we could leverage the knowledge we had acquired so far to build a complete blogging ecosystem. We could develop our own .NET Core blogging system which could be deployed in any operating system, which saves our hosting cost a lot since we have a lot cheaper hosting options using Linux operating system. Glad I could help you in acquiring EF Core knowledge, the use case is not limited to blogging system but it is vast, you could now build any system you have in mind rapidly using EF Core ORM now. Happy coding!