
## 1

## Getting Started with OpenLayers

Within the past few years, the popularity of interactive web maps has exploded. In the past, creating interactive maps was reserved for large companies or experts with lots of money. But now, with the advent of free services like Google and Yahoo! Maps, online mapping is easily accessible to everyone. Today, with the right tools, anyone can easily create a web map with little or even no knowledge of geography, cartography, or programming.

Web maps are expected to be fast, accurate, and easy to use. Since they are online, they are expected to be accessible from anywhere on nearly any platform. There are only a few tools that fulfill all these expectations.

OpenLayers is one such tool. It's free, open source, and very powerful. Providing both novice developers and seasoned GIS professionals with a robust library, OpenLayers makes it easy to create modern, fast, and interactive web-mapping applications.

In this chapter we will

- Learn what OpenLayers is
- Discuss some web mapping application concepts
- Make our First Map
- Cover concepts behind OpenLayers, such as Object Oriented Programming
- Provide information on resources outside of this book

## What is OpenLayers?

OpenLayers is an open source, client side JavaScript library for making interactive web maps, viewable in nearly any web browser. Since it is a client side library, it requires no special server side software or settings—you can use it without even downloading anything! Originally developed by Metacarta, as a response, in part, to Google Maps, it has grown into a mature, popular framework with many passionate developers and a very helpful community.

## Why use OpenLayers?

OpenLayers makes creating powerful web-mapping applications easy and fun. It is very powerful but also easy to use—you don't even need to be a programmer to make a great map with it. It's open source, free, and has a strong community behind it. So if you want to dig into the internal code, or even improve it, you're encouraged to do so. Cross browser compatibility is handled for you—it even works in IE6.

OpenLayers is not tied to any proprietary technology or company, so you don't have to worry so much about your application breaking (unless you break it). At the time of writing, support for modern mobile and touch devices is in the works (with many proof of concept examples), and should be in the official library in the near future—if they aren't by the time you're reading this.

OpenLayers allows you to build entire mapping applications from the ground up, with the ability to customize every aspect of your map—layers, controls, events, etc. You can use a multitude of different map server backends together, including a powerful vector layer. It makes creating map 'mashups' extremely easy.

## What, technically, is OpenLayers?

We said OpenLayers is a client side JavaScript library, but what does this mean?

### Client side

When we say client side we are referring to the user's computer, specifically their web browser. The only thing you need to have to make OpenLayers work is the OpenLayers code itself and a web browser. You can either download it and use it on your computer locally, or download nothing and simply link to the JavaScript file served on the site that hosts the OpenLayers project (http://openlayers.org). OpenLayers works on nearly all browsers and can be served by any web server or your own computer. Using a modern, standard-based browser such as Firefox, Google Chrome, Safari, or Opera is recommended.

### Library

When we say library we mean that OpenLayers is an API (Application Programming Interface) that provides you with tools to develop your own web maps. Instead of building a mapping application from scratch, you can use OpenLayers for the mapping part, which is maintained and developed by a bunch of brilliant people.

For example, if you wanted to write a blog you could either write your own blog engine, or use an existing one such as WordPress or Blogger and build on top of it. Similarly, if you wanted to create a web map, you could write your own from scratch, or use software that has been developed and tested by a group of developers with a strong community behind it.

By choosing to use OpenLayers, you do have to learn how to use the library (or else you wouldn't be reading this book), but the benefits greatly outweigh the costs. You get to use a rich, highly tested and maintained code base, and all you have to do is learn how to use it. Hopefully, this book will help you with that.

OpenLayers is written in JavaScript, but don't fret if you don't know it very well. All you really need is some knowledge of the basic syntax, and we'll try to keep things as clear as possible in the code examples.

> If you are unfamiliar with JavaScript, Mozilla provides phenomenal JavaScript documentation at https://developer.mozilla.org/en/javascript.

## Anatomy of a web-mapping application

First off—what is a 'web-mapping application'? To put it bluntly, it's some type of Internet application that makes use of a map. This could be a site that displays the latest geo-tagged images from Flickr (we'll do this in Chapter 11), a map that shows markers of locations you've traveled to, or an application that tracks invasive plant species and displays them. If it contains a map and it does something, you could argue that it is a web map application. The term can be used in a pretty broad sense.

So where exactly does OpenLayers fit in? We know OpenLayers is a client side mapping library, but what does that mean? Let's take a look at the following screenshot:

![](img/1b8cd151c9f278555f9cf255b20d244e_28_0.png)

This is called the Client / Server Model and it is, essentially, the core of how all web applications operate. In the case of a web map application, some sort of map client (e.g., OpenLayers) communicates with some sort of web map server (e.g., a WMS server or the Google Maps backend).

### Web map client

OpenLayers lives on the client side. One of the primary tasks the client performs is to get map images from a map server. Essentially, the client has to ask a map server for what you want to look at. Every time you navigate or zoom around on the map, the client has to make new requests to the server—because you're asking to look at something different.

OpenLayers handles this all for you, and it is happening via asynchronous JavaScript (AJAX) calls to a map server. To reiterate—the basic concept is that OpenLayers sends requests to a map server for map images every time you interact with the map, then OpenLayers pieces together all the returned map images so it looks like one big, seamless map. In Chapter 2, we'll cover this concept in more depth.

### Web map server

A map server (or map service) provides the map itself. There are a myriad of different map server backends. A small sample includes WMS, Google Maps, Yahoo! Maps, ESRI ArcGIS, WFS, and OpenStreet Maps. If you are unfamiliar with those terms, don't sweat it. The basic principle behind all those services is that they allow you to specify the area of the map you want to look at (by sending a request), and then the map servers send back a response containing the map image. With OpenLayers, you can choose to use as many different backends in any sort of combination as you'd like.

OpenLayers is not a web map server; it only consumes data from them. So, you will need to be able to access some type of web map service. Don't worry though. Fortunately, there are a myriad of free and/or open source web map servers available that are remotely hosted or easy to set up yourself, such as MapServer.

![](img/1b8cd151c9f278555f9cf255b20d244e_29_0.png)

> Throughout this book, we'll often use a freely available web mapping service from OSGeo, so don't worry about having to provide your own.

With many web map servers you do not have to do anything to use them—just supplying a URL to them in OpenLayers is enough. OSGeo, OpenStreet Maps, Google, Yahoo!, and Bing Maps, for instance, provide access to their map servers (although, some commercial restrictions may apply with various services in some situations).

## Relation to Google / Yahoo! / and other mapping APIs

The Google, Yahoo!, Bing, and ESRI Mappings API allow you to connect with their map server backend. Their APIs also usually provide a client side interface (at least in the case of Google Maps).

The Google Maps API, for instance, is fairly powerful. You have the ability to add markers, plot routes, and use KML data (things you can also do in OpenLayers)—but the main drawback is that your mapping application relies totally on Google. The map client and map server are provided by a third party. This is not inherently a bad thing, and for many projects, Google Maps and the like are a good fit.

However, there are quite a few drawbacks.

- You're not in control of the backend
- You can't really customize the map server backend, and it can change at any time
- There may be some commercial restrictions, or some costs involved
- These other APIs also cannot provide you with anything near the amount of flexibility and customization that an open source mapping application framework (i.e., OpenLayers) offers

## Layers in OpenLayers

So, what's with the Layer in OpenLayers? Well, OpenLayers allows you to have multiple different 'backend' servers that your map can use. To access a web map server, you create a layer object and add it to your map with OpenLayers.

For instance, if you wanted to have a Google Maps and a WMS service displayed on your map, you would use OpenLayers to create a GoogleMaps layer object and a WMS layer object, and then add them to your OpenLayers map. We'll soon see an example with a WMS layer, so don't worry if you're a little confused.
