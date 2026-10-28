+   9. Enter `static` and press the *Enter* key:

![](bbox=[0.254, 0.138, 0.714, 0.204])

Figure 5.5 – Naming the directory static

+   10. Right-click on the `static` directory you just created and select **New | Directory** again.

+   11. Enter `landing` and press *Enter*. This will implement namespacing of the static files directory, as we discussed earlier:

![](bbox=[0.261, 0.335, 0.706, 0.397])

Figure 5.6 – Naming the new directory reviews to implement namespacing

+   12. Download the `logo.png` file from `https://raw.githubusercontent.com/PacktPublishing/Web-Development-with-Django-Second-Edition/main/Chapter05/Exercises.01/business_site/landing/static/landing/logo.png` and move it into the `landing/static/landing` directory.

+   13. Start the Django dev server. If it is not already running, then navigate to `http://127.0.0.1:8000/static/landing/logo.png`. You should see the image being served in your browser:

![](bbox=[0.141, 0.616, 0.827, 0.872])

Figure 5.7 – Image served by Django