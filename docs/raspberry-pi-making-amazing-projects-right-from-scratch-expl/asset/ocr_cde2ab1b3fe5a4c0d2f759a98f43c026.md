# Introducing the Internet of Things

The Internet of Things is all about connecting the devices that were previously not connected to the Internet, in order to make them smarter and more controllable. Since the size and cost of electronic devices are dropping exponentially, we can now afford to incorporate them in our daily use, are garden watering systems, home automation/monitoring systems, and so on. The Internet of Things in this way has opened up a whole host of opportunities, the advantages of which are being taken up by hobbyists, startups, and major companies alike. At the heart of the Internet of Things revolution are small micro computers and sensors that allow these devices to connect to the Internet and exchange data between them.

As mentioned before, in this particular chapter we will extend our previous projects by connecting the Raspberry Pi to the Internet, and make additional projects to demonstrate the capability of the Internet of Things.

We will now learn to exchange data from the Twitter API and post our sensor readings to Twitter.

# Installing the Twitter API for Python

**API** (Application Programming Interface) and it allows developers to communicate with various web services. Since Python is, by default, installed on the Raspberry Pi, we will use a Python script to communicate with the Twitter API over the Web. For this, we will use an open source library called **Tweepy**. It works with Python versions as old as 2.6 and is still under active development. So bugs are likely to be fewer and, if any are found, they are likely to be fixed quickly.

> The GitHub repository for Tweepy can be found at the following link, so that new issues can be submitted and the code can be understood much more clearly:
> 
> https://github.com/tweepy/tweepy

We will now proceed to install it and learn how to use it to do things such as sending tweets.

This library can be installed using pip, with the following command:

```
sudo pip install python-twitter
```