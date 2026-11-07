The main difference is the presence of the Teams tab, where you can define teams within your organization. Docker Hub will automatically create the Owners team. The Owners team is kind of special—you need to be a member of the organization's Owners team to create a new repository, a new automated build and also a new team within this organization. If you belong to the Owner team, you can assign access right in the Collaborators section of the repository view. This can be read access (allowing you to view, search, and pull a private repository in the same way as they can a public repository), write access (allowing you to push an image), or admin access (allowing you to modify the repositories properties such as description, collaborators rights, public or private visibility, or delete the repository). You can add or invite users to any of the organization's teams, by selecting the team and entering a username (for current Docker Hub users) or e-mail address, if they are not yet registered:

![](bbox=[0.1, 0.34, 0.9, 0.48])

In result, the invited users will receive an e-mail with an invitation to join the team.

# Collaborators

The Docker Hub introduces a term collaborator. A collaborator is simply another Docker Hub user, who you can give access to your repository, either public or private. You do it in the repository's Settings page, in the Collaborators tab, as shown in the following screenshot:

![](bbox=[0.1, 0.65, 0.9, 0.8])