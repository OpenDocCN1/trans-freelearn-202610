# # Downloading the Poky metadata

After having installed all the necessary packages, it is time to download the source from Poky. This is done through the Git tool:

```
$ git clone git://git.yoctoproject.org/poky (branch master)
```

![](bbox=[0.0, 0.15, 0.99, 0.665])

# ## Note

Another method involves directly downloading a tar .bz2 file from this repository: https://www.yoctoproject.org/downloads

To avoid all hazardous and problematic manipulations, it is strongly recommended to create and switch to a specific local branch:

```
$ cd poky
$ git checkout daisy -b work_branch
```