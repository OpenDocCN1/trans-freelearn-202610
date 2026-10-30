+   - **Magento Configuration - Index:** Instead of the indexes being updated at the moment when you make a change in the catalog through the admin panel, you can set the reindex process to be done by schedule according to the cron job configuration. You can make that change in the **SYSTEM | Index Management** admin panel page. From there, you will see that you can set the indexes to be updated by schedule using the mass action:

![](bbox=[0.104, 0.243, 0.896, 0.593])

+   - **Magento Configuration - Production Mode:** When the server and Magento have been configured, you should set Magento to run in **Production Mode**. In the production mode, static view files are served from the cache instead of going through the fallback mechanism from Magento. In addition to that, errors are logged by Magento in the `var` folder and they are not displayed to the user on the storefront. You can enable the production mode by running the following command in the Magento 2 command-line tool:

```
php bin/magento deploy:mode:set production
```

You will see output similar to the following screenshot: