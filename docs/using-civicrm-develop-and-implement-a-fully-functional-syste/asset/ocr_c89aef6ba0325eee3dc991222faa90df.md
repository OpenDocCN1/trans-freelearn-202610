+   6. If any of the resources in your site reference the full URL or file location, you may need to update these (for example, if your Drupal navigation menu has links to profiles).

+   7. If you are using override directories (`PHP` or `.tpl`), you will need to reset them to reflect the new file location. Go to **Administer CiviCRM | Configure | Global Settings | Directories**. Also visit the **Resource URLs** configuration page to confirm that the values are set correctly.

After completing the move, be sure to test the site! Click through CiviCRM and confirm that the pages load as expected, and the URL remains on your new site (if any outdated links exist, it may send you to your old domain or subdirectory).

# System maintenance

As with any database system, there will be ongoing maintenance steps that should be performed on a periodic basis. Our focus in this section will be on basic maintenance steps specific to CiviCRM. It is outside the scope of this chapter to address the broader topic of server and MySQL database maintenance. Depending on the size of your database and whether you are using a managed hosting service, you should be aware of basic maintenance steps with your database and hosting environment. Also, we will not address scalability-related issues and maintenance. However, as your system grows, you should be aware of the impact on your MySQL database performance and size.

The most important maintenance step with your system is to keep it updated to the latest revision, and not fall too far behind in major version upgrades. As the steps for doing that have been addressed earlier in this chapter, we won't go into detail here; we repeat it only because of its importance:

+   - Revision releases may contain security-related patches which are critical for maintaining a secure and safe database.

+   - Revision releases may contain bug-fix patches which are critical for maintaining a stable and fully operational system.

+   - Major version releases contain new features and functionality. If the new features are not something you will need or use, you may decide not to upgrade when it is first released. That's fine, and certainly not uncommon. But the core development team will only actively develop and patch the current stable version, which means there may be security or bug issues found in older versions which are not addressed. As a general rule, you should avoid falling too far behind in the upgrade cycle for major revisions. Besides, there's lots of great functionality available in the new releases you'll want to benefit from!