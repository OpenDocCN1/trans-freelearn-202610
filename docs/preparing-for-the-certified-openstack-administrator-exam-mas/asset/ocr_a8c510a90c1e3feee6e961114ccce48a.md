# Exam objectives

Let's take a look at the Neutron exam objectives.

The following figure breaks down various Neutron-related objectives you should know on the COA exam. As you can see in Figure 6.3, all of these tasks can be completed on both the Horizon dashboard and CLI. Remember: time is of the essence during the exam, and in many cases, it can be much faster to tackle a task on the dashboard.

![](bbox=[0.18, 0.29, 0.81, 0.66])

Figure 6.3: Neutron exam objectives can be completed on the Horizon dashboard or CLI

# Exam objective - managing tenant networks

Tenant networks are a necessary part of life for the OpenStack user. By default, a non-admin user can create multiple tenant networks (up to the max project quota) and attach multiple subnets to them. The same project/owner philosophy is applied to networks and subnets: any newly created network and subnet will be owned by the project to which a user was scoped at the time of its creation. Only users within the project will be able to manage the newly created networks and subnets. Although users can share networks with specific projects via RBAC rules, this is outside the scope of the COA review and not a required objective.