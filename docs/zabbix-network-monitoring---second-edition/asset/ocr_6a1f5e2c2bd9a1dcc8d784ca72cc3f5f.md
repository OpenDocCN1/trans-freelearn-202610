# # Frontend

+   - **Q:** I have installed the Zabbix frontend. What's the default username and password?
+   - **A:** The username is Admin, and the password is zabbix.
+   - **Q:** I'm setting up Zabbix from an SVN checkout. When I switch languages in the frontend, nothing happens.
+   - **A:** In the frontend directory, in the locale subdirectory, there's a `make_mo.sh` script. It compiles the needed mo files out of the translation source po files—run it. Note that it will need Gettext tools, and the webserver might have to be restarted afterwards.

# # Backend

+   - **Q:** Zabbix is working correctly, but some/all graphs are not displayed.
+   - **A:** Refer to the Apache error log for more details. Usually, this is caused by the PHP script memory limit being too low—if that is the case, increase it by setting the `memory_limit` parameter to a higher value and restarting the webserver. Another possible cause is a broken `conf/zabbix.conf.php` file—verify that it does not have any weird characters, especially at the end of the file.
+   - **Q:** Complex views, such as screens with many elements, sometimes fail to load. What could be causing this?
+   - **A:** Like the previous problem, check that the PHP memory limit has not been exceeded. Additionally, check the PHP script timeout (`max_execution_time` parameter) and increase it if necessary.
+   - **Q:** My graphs have gaps.
+   - **A:** It's not only graphs—data is missing in the database as well. This problem should be resolved by finding out what causes the data loss. Common reasons for this are:
+     - **Network problems:** If the network is unreliable, data will be missing.
+     - **An overloaded monitored device:** For example, if you have added a switch with many ports and are monitoring several items on each port very often, try increasing the intervals and disabling unneeded items.
+     - **An overloaded Zabbix server:** It's usually the database. Check the system load on the Zabbix database server, especially `iowait`.
+   - **Q:** I had Zabbix installed and running, but it is suddenly showing me the installation screen again.