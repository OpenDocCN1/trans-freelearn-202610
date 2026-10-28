The following diagram illustrates the preceding flow:

![](bbox=[0.177, 0.131, 0.823, 0.296])

Figure 11.9: RLS implementation flow

In the next section, we look at different RLS implementation approaches.

# # Common RLS implementation approaches

There are usually two different approaches to implementing RLS in Power BI Desktop: static RLS and dynamic RLS. In the following few sections, we look at both approaches by implementing real-world scenarios.

# ## Static RLS implementation

A static RLS approach is when we define rules that statically apply filters to the data model. For example, in *Figure 11.2*, we created a static RLS rule to filter the **Internet Sales** amounts by currency when the currency equals **AUD**. While static RLS is simple to implement, depending on the scenario, it can get quite expensive to maintain and support. Moreover, static RLS is sometimes just enough to satisfy business requirements.

Let us go through a scenario; Adventure Works is an international organization with a few security groups within **Azure Active Directory (Azure AD)** or Microsoft 365, separating users based on their geographical locations. The business wants to implement RLS so that Australian users