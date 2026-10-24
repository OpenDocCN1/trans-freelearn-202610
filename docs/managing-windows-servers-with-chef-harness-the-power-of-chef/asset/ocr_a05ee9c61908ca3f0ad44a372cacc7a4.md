# # Fetching from GitHub

In order to use these resources, you will want to either download them from GitHub
or use Git to clone them to a local system. If you are not interested in working with
them as Git repositories (that is, version controlled source code), downloading them
will be the simplest way to get them. If you go to the previous URLs, you will see
a link titled **Download ZIP** on the right-hand side of the screen, as shown in the
following screenshot:

![](bbox=[0.1758, 0.2812, 0.8242, 0.5828])

If you opt to download them as ZIP files rather than clone them with Git, you will
want to ensure the directories are named to match the proper name of the cookbook
(`chef_handler`, `windows`, `iis`, and `umbraco`). For example, if you clone the `umbraco`
cookbook, it will be put in an `umbraco-cookbook` directory, whereas decompressing
the ZIP files may result in an `umbraco-cookbook-master` directory. Once you have
them downloaded and renamed, install them on your Chef server by running the
following command from the directory where all four cookbooks are located:

`knife cookbook upload -o . chef_handler windows iis umbraco`

Chef will validate that each cookbook’s dependencies are satisfied (or are in the
process of being uploaded) as you upload them. The `chef_handler` cookbook must
be installed in order for the `windows` cookbook to work, which is needed to support
the `iis` cookbook, and then finally the `umbraco` cookbook can be installed as its
dependencies have been met.