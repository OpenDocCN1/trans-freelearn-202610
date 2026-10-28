4. Create a new run configuration to execute manage.py runserver for the project. You can reuse the Bookr virtual environment again. The Run/Debug Configurations window should look similar to the following figure when you’re done:

![](bbox=[0.12, 0.18, 0.84, 0.7])

Figure 6.4: Run/Debug Configurations for Runserver

You can test that the configuration has been set up correctly by clicking the Run button, then visiting http://127.0.0.1:8000/ in your browser. You should see the Django welcome screen. If the debug server fails to start or you see the Bookr main page, then you probably still have the Bookr project running. Try stopping the Bookr runserver process and then starting the new one you just set up.

5. Open settings.py in the form_project directory and add "form_example" to the INSTALLED_APPS setting.