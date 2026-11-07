All you need to do now is pick up the repository from the list. You will be presented with the `Automated Build` configuration screen. Here, you can set the visibility of the resulting image (public or private). By default, Docker Hub will tag the resulting images with the names of the branches in your Git repository. The Git `push` command to the `master` branch will trigger a build and tag the image with the `latest` tag. A git `push` to a branch other than `master` will trigger the build of an image from that Git branch. The result will then be tagged with this branch name.

TIP: You can create multiple Automated Builds per repository and configure them to point to specific Dockerfile's or Git branches.

This is usually sufficient and it's a very effective setup, but you can customize the behavior if you need to. Just click on the Click here to customize link to open a rule editor:

![](bbox=[0.1, 0.4, 0.9, 0.65])

In the **Autobuild Tags** editor, you can specify the tags that specific kind of git push (branch or tag) will have. As you can see, the result of the `master` git branch build will be tagged `latest` by default. There is a lot of flexibility here, you can add or remove rules as you like. For example, a build result of push to the specific branch will be always tagged differently from the other branches.

TIP: You can always change the tagging configuration later, after creating an `Automated Build` in the `Build Settings` page.