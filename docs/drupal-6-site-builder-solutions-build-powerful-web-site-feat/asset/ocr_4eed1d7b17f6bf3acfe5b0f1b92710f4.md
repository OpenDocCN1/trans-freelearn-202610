+   5. You can also add images to the info windows, in addition to text.
+   6. If you add more than one div to a link, the info window will be displayed in several tabs, one for each div.

# Using links to change the map

Good Eatin' Goal: Add links to each restaurant location and have the map automatically zoom in to the location.

Additional modules needed: GmapEZ (http://drupal.org/project/gmapez).

# Basic steps

If you want to allow customers to be able to jump easily from location to location, you can add links within the text surrounding the map, which will cause GmapEZ to automatically re-center the map to the location that you linked it to.

+   1. You will first need to add IDs to each anchor (<a>) tag.
+   2. For example, we will modify our links as follows.
+   ```
+   <a id="original" href="http://maps.google.com/maps?q=111+Main+Street,+Parker,+CO&ie=UTF8&ll=39.521985,-104.784765&spn=0.030853,0.077248&z=14&iwloc=cent">
+   </a>
+   ```
+   3. We can now link to the location from the text by creating another anchor tag as follows:
+   ```
+   <a href="#original" class="ZOOM">Original Location</a>
+   ```
+   4. The #original indicates that the browser should link to the anchor tag on the same that has an ID of original.

# Content sharing, bookmarking, and twittering—oh my!

After you have built high quality content for your web site, you need to make sure that everyone knows about it. One of the best ways to do this is by letting your customers add your content to social bookmarking sites such as Digg, del.ico.us, technorati and more.

# Diggin' Digg It with DiggThis

Good Eatin' Goal: Give our customers the opportunity to vote on our content at Digg, to help draw traffic to the site.