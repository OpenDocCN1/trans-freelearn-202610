Collaborators are not allowed to perform any administrative tasks such as deleting the repository or changing its status from private to public. They can, however, use your repository by executing `docker push` and `docker pull` commands. If you need more specific and detailed access rights setup for your repositories, consider using organization and teams provided by Docker Hub.

![](bbox=[0.13, 0.22, 0.21, 0.28])

It is worth noting, that a collaborator cannot add other collaborators. Only you, as the owner of the repository has administrative access.

# Private Repositories

If you want to store images that you want to keep private, you need to store them in a private repository. It will be visible only to your own account or within an organization or group. Once the repository is marked as private, you can push and pull images to and from it using Docker, the same as with other repositories that are public. The only difference is that you need to be signed in and have access to work with a private repository. Private repositories behave just like public ones; however, they will not be listed on the public registry.

In the repository's `Settings` page, you can switch its visibility, to be either `public` or `private`, as shown in the following screenshot:

![](bbox=[0.1, 0.54, 0.9, 0.72])

If you make your repository private, it will be available to the members of your organization and you can also give access to anyone you choose as a collaborator. As you remember, the free Docker Hub account allows you to have just one private repository, if you want more, you will need to upgrade your plan.