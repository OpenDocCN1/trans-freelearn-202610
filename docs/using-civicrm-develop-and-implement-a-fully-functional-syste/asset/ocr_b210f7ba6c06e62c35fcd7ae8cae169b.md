11. Re-enable CiviCRM integration and extension modules:
  - Click **Administer | Site Building | Modules**.
  - Check **Enabled** beside the appropriate integration and extension modules that you noted as you disabled them previously.
  - Click **Save configuration**.
  - If you have re-enabled Views integration:
    Click on **Administer | Site Building | Views | Tools**.
    Click on **Clear Views' cache** button.

12. Restore CiviCRM as the home page, if appropriate:
  - Click on **Administer | Site Configuration | Site Information**.
  - In the **Default front page**, change the value to what was saved for this value earlier in the upgrade.

13. Put the site back into production:
  - Click on **Administer | Site Configuration | Site Maintenance**
  - Check **Online**
  - Click on **Save configuration**

14. Review the CiviCRM functionality to ensure that it is working properly.

15. Re-enable the scripts that you had previously used and saved in the upgrade process:
```
$ cd /path_to_docroot/sites/all/modules/civicrm/bin
$ cat /path_to_safe_dir/SavedListOfEnableScripts.txt
```

16. For each of the script file listed, run a command along the following lines to remove the .txt extension in order to re-enable the script:
```
$ mv scriptName.php.txt scriptName.php
```

17. If you are having difficulties, review the troubleshooting suggestions at http://wiki.civicrm.org/confluence/display/CRMDOC/Upgrade+Drupal+Sites+to+3.2#UpgradeDrupalSitesTo3.2-UpgradeTroubleshooting. If you are still having trouble, visit the CiviCRM upgrade forum at http://forum.civicrm.org/index.php/board,8.0.html.

After you have finished your upgrade, you should consider whether you need to enable any new components or configure any new CiviCRM permissions.