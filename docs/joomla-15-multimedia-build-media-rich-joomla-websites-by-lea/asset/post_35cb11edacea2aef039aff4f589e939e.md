
## Disabling images, CSS, and JavaScript

For older mobile devices, one of the best development methods available is to disable image CSS and JavaScript on your site pages, and see how it looks. Your site will take on a completely different look and feel with the main styling of the template removed, and you will see the natural layout of elements as they load on your page.

## Take an on-demand approach with your multimedia

If you utilize multimedia in your website, think about offering these to the user when they request it, rather than bombarding the user with this information as they land on your website page.

Videos, images, audio, or other multimedia in your website can be presented to the user via hyperlinks to the intended content. This approach offers choice, and means that the user can choose to visit that content if they wish to, and not have it force-fed to them. If you are displaying the episodes of a podcast using Flash players, consider using a player that loads the media when someone chooses to play. This approach usually leads to well-structured sites that are quick to load and have a healthy "large content" feel to them.

## Load in custom stylesheets or templates for mobile devices

With a combination of PHP, HTML, and CSS, it is possible for your Joomla! site to recognize particular mobile web devices and deliver specific stylesheets for these. By using this method, you can provide a simplified layout for your content, which makes page loading faster and easier for users to navigate your site pages.

There are a number of websites and tools that can help generate PHP code for recognizing particular devices viewing your website. Some PHP and CSS skills are required in order to achieve these results, but with efforts in this area, you can specifically target the mobile market with your Joomla! content. One such useful tool can be found at `http://detectmobilebrowsers.mobi/`.

![](img/0151410972a5c5ea0aab26416e44b1da_262_0.png)

The site generator will allow you to tailor the code for your environment, and then download the applicable code for you to use. It is free for non-profit websites and donations are accepted for commercial use. For those with limited PHP knowledge, but like to get your hands mucky with some code play, this tool will be helpful.

For those with limited development skills, a number of Joomla! Extensions are also available.

## Available extensions

There are a number of mobile web extensions now available for Joomla! that can help to make your site accessible for mobile devices, and decrease the technical knowledge required to make this happen.

## PDA-plugin for Joomla! 1.5 (P.T)

The essence of the PDA-plugin for Joomla! 1.5 is that it activates a PDA template, or a mobile web-friendly template, for your Joomla! site instead of using the default site template:

![](img/0151410972a5c5ea0aab26416e44b1da_263_0.png)

This plugin will determine if it is appropriate to activate itself, based on it recognizing the user agent of a mobile device. There are two ways to use this plugin:

- 1. Display the mobile version of a site for the mobile browser that is on the same link, http://yoursite.com/. It is possible to see both the full and mobile versions of a site depending on the device used for viewing. This is referred to as the multi-client approach.
- 2. Display the mobile version of a site for a subdomain. For example, for http://pda.yoursite.com/. This is referred to as the multi-site approach.

With this plugin, it is possible to use either of these approaches, or both.

### Installation

There are two elements for the PDA-plugin for Joomla! 1.5. Firstly, you install the template, and secondly the plugin using the Joomla! Extension Manager.

The plugin will require publishing and contains a number of configuration options. The way the PDA-plugin works is to activate the PDA Template (or another template if set in the Plugin Parameters) so that your site will be simplified to work on mobile web devices.

This plugin takes the hard work out of placing code in to recognize different devices visiting your website, but you may still need to get your hands dirty by adjusting the included PDA Template to your liking.

To download this extension, visit http://extensions.joomla.org/ and search for "PDA-plugin".

## WAFL: Mobile Content Adaption (P,C,T,M)

**Website Adaption and Formatting Layer (WAFL)** was built to achieve device-specific content adaptation for your Joomla! website. It is still in alpha mode so has not reached the maturity of other Joomla! Extensions yet.

This plugin supports three modes of operation. The selected mode will be activated when the plugin detects that a request is coming from a mobile device:

- 1. Template switching: You will be able to select a template that will be used for mobile requests. The plugin contains a mobile template, which can be used.
- 2. Mobile redirect: The visitor will be redirected to a URL that you can specify.
- 3. Siruna: It is a fully managed solution for creating mobile web applications. When this mode is activated, the plugin will redirect any mobile requests to an instance of the Siruna platform. It will also deliver a mobile template (enriched with information for Siruna) to requests coming from the Siruna platform.

### Installation

There is one extension to install that contains four different files:

- 1. **Plugin**: It intercepts Joomla! events so that mobile users get delivered mobile-enhanced content.
- 2. **Administrator component**: In the WAFL administration-side section, the web administrator will be able to change preferences to suit their site.
- 3. **WAFL template**: It delivers our basic version of a mobile template without taking screen widths and heights into account.
- 4. **WAFL module**: It is in fact a component wrapper to enable module ordering in the WAFL template.

The extension is installed via the Extension Manager.

The administration side of our extension can be found by clicking on **WAFL** in the **Components** menu. Here, you can change the look-and-feel of the mobile website version, or how you would like to have your mobile site configured and displayed.

You will need to check your mobile settings by surfing your website using a mobile device, or even better to look at a number of devices (just as you would when testing with Safari, IE6, IE7, and so on) to make sure all is as you require.

To download this extension, visit [http://extensions.joomla.org/](http://extensions.joomla.org/) and search for "WAFL Mobile Content".

## Mobilebot for Joomla 1.5 (P)

Mobilebot 1.0 for Joomla! 1.5.x can detect visitors using mobile devices and change the Joomla! Template automatically.

![](img/0151410972a5c5ea0aab26416e44b1da_265_0.png)

[ 251 ]

Mobilebot 1.0 for Joomla! 1.5.x has the following features:

- 1. It can detect iPhone, Blackberry, Android, and Opera Mini browsers separately and load a particular template for each of them.
- 2. It has two extra spaces so you can add your own custom mobile devices.
- 3. You can adjust your final HTML code with simple/regular expression PHP replacements. This can be done independently for each particular device.
- 4. If none of the specific devices are detected (or you just have one template for all mobiles), the plugin can also perform a general mobile detection and load a "generic" mobile template if configured accordingly.

### Installation

Mobilebot is a simple plugin extension that is installed via the Extension Manager, and configurations are made within the Plugin Parameters.

To download this extension, visit http://extensions.joomla.org/ and search for "Mobilebot".

## Testing for the mobile web

The following is a list of measures that you can take to improve your Joomla! site's "mobile friendly" presence.

## Disable your stylesheet

As has been mentioned previously, one of the easiest ways to test compatibility of your website is to remove the Cascading Style Sheet from the equation. As your stylesheet will contain an abundance of styling elements, it is important from an accessibility point of view for you to see "if that menu still appears" or "do I still have a search feature showing" on your site when you have removed the styling:
