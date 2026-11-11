# Library

When we say library we mean that OpenLayers is an API (Application Programmer Interface) that provides you with tools to develop your own web maps. Instead of building a mapping application from scratch, you can use OpenLayers for the mapping part, which is maintained and developed by a bunch of brilliant people.

For example, if you wanted to write a blog you could either write your own blog engine, or use an existing one such as WordPress or Blogger and build on top of it. Similarly, if you wanted to create a web map, you could write your own from scratch, or use software that has been developed and tested by a group of developers with a strong community behind it.

By choosing to use OpenLayers, you do have to learn how to use the library (or else you wouldn't be reading this book), but the benefits greatly outweigh the costs. You get to use a rich, highly tested and maintained code base, and all you have to do is learn how to use it. Hopefully, this book will help you with that.

OpenLayers is written in JavaScript, but don't fret if you don't know it very well. All you really need is some knowledge of the basic syntax, and we'll try to keep things as clear as possible in the code examples.

> If you are unfamiliar with JavaScript, Mozilla provides phenomenal JavaScript documentation at https://developer.mozilla.org/en/javascript.

# Anatomy of a web-mapping application

First off—what is a 'web-mapping application'? To put it bluntly, it's some type of Internet application that makes use of a map. This could be a site that displays the latest geo-tagged images from Flickr (we'll do this in Chapter 11), a map that shows markers of locations you've traveled to, or an application that tracks invasive plant species and displays them. If it contains a map and it does something, you could argue that it is a web map application. The term can be used in a pretty broad sense.