# Creating the web page

Our Web-based control panel is going to be a single PHP-driven HTML5 web page which will be **mobile optimized**. HTML5 is the latest mark-up standard for web pages and is supported by most modern smartphones and browsers. We will also create a **cascading style-sheet (CSS)** that will make our page look half reasonable on mobile devices.

To create the web files, I recommend that you use something like the excellent Notepad++ on your desktop computer, rather than doing it directly on the Raspberry Pi. Alternatively, if you are a seasoned web developer, you may already have your IDE of choice.

# The control panel HTML template

The first thing we'll do is create an HTML file that we can use to test our layout before we put the HTML into a PHP file to make it interact with our system. This makes it easier to tweak the way we want it to look beforehand, without the PHP scripts getting in the way.

> This is not a tutorial on Web development—there is a plethora of books out there on that subject—but I hope the code is clear enough for you to work out what's going on. The code I'm going to show you is fully functional, so you can just use what I give you without having to do any more. Hopefully, it makes your control panel look OK too!

The following mark-up gives you a basic control panel with status for our 8 zones, a master arm and disarm switch, and switches to enable or disable any of our zones.

The <head> section of the code contains some <meta> tags that help mobile devices know that it's a mobile-friendly site. In the main <body> mark-up, we have a section for each zone that contains the zone's name and an on/off switch. Each zone is in its container so that we can also highlight a particular zone that needs our attention, for example, if it's triggered.

You can find the full HTML5 markup for our control panel in the `alarm-panel.html` file located inside the code folder of `chapter 7`.