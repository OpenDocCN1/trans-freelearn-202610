# Becoming a sharp shooter with code libraries

So, does this mean that I'm suggesting that you stay away from code libraries entirely? No, not at all! Part of the reason why freelancing is possible (and affordable for clients) is that you don't have to create 100% of an app's functionality from scratch.

Instead, I've learned to be selective about the code libraries that I integrate. Let's look at a couple of the code libraries that I regularly use on projects:

+   - **Pundit**, for building a permission structure. Pundit is a lightweight code library that is easy to customize and doesn't lock you into a narrow set of permission features.
+   - **Devise**, for authentication. Writing an authentication system from scratch is time consuming. The Devise gem allows Rails developers to quickly integrate features such as registration, login, logout, and advanced components such as secure password retrieval. Additionally, Devise is customizable and I've rarely run into a situation where it didn't work for an app's requirements.

Do you notice how these code libraries operate like helper libraries for specific features? My rule for integrating code libraries into projects is that I pick out packages that assist with small elements of a project. And I shy away from silver bullets that promise to take care of large portions of a project's functionality, but are difficult to customize.