It is perfectly normal that, most of the time, you will import the usual packages from dbt Hub; nevertheless, we believe that is useful to know some advanced possibilities for eventual future use.

# Using packages to build internal shared libraries

If your company has multiple dbt projects, there is a good chance that you are applying the same best practices and patterns in all of them. As a consequence, a substantial number of macros and utility models are used with little to no changes in all or many of these projects.

Examples of shared macros might involve ingesting data (using macros, which we will see in the following chapters), generic tests that you keep using (such as against hash conflicts), or format conversion that you need to apply often (such as removing or adding padding zeros on strings).

Examples of shared models can include monitoring performance and resources in on-site deployments or consumption in SaaS platforms, providing reporting on security, such as users and role access, or providing audits on activity, such as queries run by a user or users accessing some info.

In these cases, the temptation to use copy and paste and reuse models or macros is strong, but if you go back to the beginning of this chapter and read again the Why copy and paste kills your future self section, you will be reminded that is not a good idea inside a project and, as you can guess, even across projects.

The solution is to refactor the common macros and models into a standalone dbt project that will become the first version of your shared library. You will then evolve it according to the needs that you will have across your projects, such as when we extended our current_from_snapshot macro when the need for an extra feature arose.

Designing a library is not a simple thing, as you need to balance the flexibility that you want to give to users with the simplicity of use. The following guidelines will help you to get started:

+   - Prefer small, focused packages to a catch-all package. It’s easy to import three or five different packages, but you cannot import just half of one.

+   - Especially when including models that will be materialized at every run of your project, err on the side of making multiple, small packages with high cohesion so that your users can pick the functionalities they want without any extra baggage.
+   
+   Provide a simple way to configure the models, using dbt variables or environment variables, and document how to do the configuration.

+   - When you are making a package that consists only of macros or tests, you can err on the side of making fewer, bigger packages, as unused macros and tests just sit there without bothering anyone. A good example is the dbt_utils package that provides a set of quite different macros, with the common denominator being their general use.