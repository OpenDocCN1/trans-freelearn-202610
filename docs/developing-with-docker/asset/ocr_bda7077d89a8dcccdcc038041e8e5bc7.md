If you select Link GitHub, you will need to do one more choice. You can give Docker Hub public and private access or a limited, read-only, public access to your repositories, as you can see on the following screenshot:

# Public and Private (Recommended)

+   - Read and Write access to public and private repositories. (We only use write access to add service hooks and add deploy keys)
+   - Required if you want to setup an Automated Build from a private GitHub repository.
+   - Required if you want to use a private GitHub organization.
+   - We will automatically configure the service hooks and deploy keys for you.

# Limited Access

+   - Public read only access.
+   - Only works with public repositories and organizations.
+   - You will need to manually make changes to your repositories in order to use Automated Build.

There's a big difference between the two. The first option gives read and write access to public and private repositories, it means that Docker Hub will be able to add service hooks and deploy keys to your repositories automatically. Also, this read-write access is mandatory, if you want to set up an Automated Build from a private GitHub repository. The limited, read-only access means that you will be able to create an Automated Build repository only with public repositories. Also, in order to use the automated build feature, you will need to make changes to the repository on your own.

Giving Docker Hub access to your private repository is a lot more convenient and recommended, but consider using only the limited access if you have security concerns.