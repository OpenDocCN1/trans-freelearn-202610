
```javascript
const express = require('express');
const geoip = require('geoip-lite');
const device = require('express-device');
var useragent = require('useragent');
useragent(true);
var model = require('./trackModel');
const app = express();

app.use(device.capture({parseUserAgent: true}));
app.enable('trust proxy');

app.get('/api/track', function(req, res) {
   console.log('tracking code' + JSON.stringify(req.query));
   let ip = req.headers['x-forwarded-for'] || req.connection.remoteAddress || req.socket.remoteAddress || req.connection.socket.remoteAddress;
   var agent = useragent.parse(req.headers['user-agent']);
   var ua = useragent.is(req.headers['user-agent']);
   let geoLocation = geoip.lookup(ip);
   var buf = new Buffer([
      0x47, 0x49, 0x46, 0x38, 0x39, 0x61, 0x01, 0x00, 0x01, 0x00,
      0x80, 0x00, 0x00, 0xff, 0xff, 0xff, 0x00, 0x00, 0x00, 0x2c,
      0x00, 0x00, 0x00, 0x00, 0x01, 0x00, 0x01, 0x00, 0x00, 0x02,
      0x02, 0x44, 0x01, 0x00, 0x3b
  ]);
  res.set('Content-Type', 'image/png');
  res.end(buf, 'binary');
  let trackData = {
    'trackingCode': JSON.stringify(req.query),
    'ip': ip,
    'deviceType': req.device.type,
    'deviceName': req.device.name,
    'device': req.device,
    'agent': agent,
    'location': geoLocation
  };
  var trackInfo = model({data: trackData});
  trackInfo.save(function(err) {
      if (err) {
        console.log(err);
      }
  });
  console.log(trackData);
});

app.listen(3000);
console.log("Started at 3000");
```

Now, run the code and hit the same API, that is, `http://localhost:3000/api/track?code=1234456`. You should now see the data in your Azure collection.

Congratulations! Now you can send any number of emails, add that tracking code to them, track the data, and store it using the code that we have developed.

I hope you have gained some good insights about the email tracking use case in this chapter.

# Cosmos DB Deployment and Management

## Summary

In this chapter, we have, of course, learned things related to email marketing! We learned how we can design a scalable email sender, which can send a huge number of emails.

We also looked at email tracking and figured out a way to track the emails that we send. We wrote the code to extract the tracking information, such as device, location, open rate, and so on. We then integrated the Cosmos DB MongoDB API to store that tracking information.

In the next chapter, we are going to learn more about the deployment part of the Cosmos DB and the best practices for developing a Cosmos DB application.

## Turnkey distribution

So far, we have designed, developed, and tested an application built on top of a Cosmos DB and Azure infrastructure. The time has come to deploy it and let the world know about your new app!

In this chapter, we are going to learn about the few deployment concepts of Cosmos DB.

We will also cover the following topics:

- Turnkey distribution
- Failover handling
- Database deployment
- Backup and restore
- Deploying a custom app over the Azure network
- Monitoring the traffic and usage

You can email me at `shahid@codeforgeek.com` or tweet me at `@codeforgeek`.

Before you say, "Shahid, we have learned this in previous chapters, why do it again now?", I want to hold you up there. I know we did cover this topic in our previous chapters, but I would like to revisit it, because it's really important for your application deployment plan.

If you are a DevOps, you can thank me later (they know what I mean).

Cosmos DB provides a competitive feature, which none of the other managed database providers, such as Amazon, Rackspace, and so on, offer, at least at the time of writing. Hence, it becomes a very important selling point of Cosmos DB.

Let's revisit it again and see how it can help your deployment plan. So, you can replicate your data across more than 30 regions on the planet with just a few clicks of a button (not really one click, but very simple). That's what turnkey global distribution means.

So, a possible use case in the deployment plan could be as follows. Suppose you have an SaaS-based application and you have a huge customer base in Germany, but your application is deployed at a New York data center. Now, based on the data transfer rate, you can spin up one read region in Germany (if they provide one there) or near to Germany to facilitate those customers in a much better way.

Mainly, the replication is meant for the failover conditions; if some node fails in a particular region, then another node with the same data takes over and customers never know that anything has changed.

For example, refer to the following screenshot from [Chapter 2](#f8c36a85-28c9-452c-8c81-e31b4050b672.xhtml), *Cosmos DB Global Turnkey Distribution System:*

![](assets/b09775c4-f8db-4fa4-a1f5-b255c775f374.png)

Here, my write region is based in Southeast Asia and I can pick and choose any read region from across the globe. There are tons of data centers in the states, very few in Russia, and none in Africa (which isn't fair; Africa is blooming in IT).

All you need to do is pick a region and see the magic happen; you get your replication done.

With this mention of replication, let's dive into the next important topic of deployment, that is, failover handling.

## Failover handling

When something fails, what's going to happen?

This is the key question of any deployment, because it matters. Consider applications like WhatsApp; if they go down, even for a minute, then the user base is going to go crazy. With your application, too, if the system fails, then you are going to lose users but, more importantly, you are also going to lose your reputation.

Cosmos DB is a managed database provider and I believe that they care about failover a lot. There are generally two ways to handle failover:

- Automatic failover
- Manual failover

Let's discuss each of them.

### Automatic failover

In rare cases of a system outage, automatic failover tries to recover and reassign the traffic to the existing regions. There are generally two cases where this applies:

- Read region failover
- Write region failover

If any read regions fails, Cosmos DB automatically marks them offline, in order to avoid any existing traffic trying to route to that region (to avoid `404` and `502` errors), and, using the internal consensus, figures out the next read region and drives the traffic to that region.

This is pretty much what Cosmos DB offers and this is, I believe, what every other database provider offers. The key feature is the re-syncing of the data when the affected region is online again. Cosmos DB updates that region with the data that it missed during the affected time and everything returns to a normal state again.

If any write region fails, then Cosmos DB looks for another write region. If your application has only one write region and that failed, Cosmos DB will then convert any read region, based on their relative priorities, to a write region and makes the application run smoothly.

When the original write region comes back online, it will re-sync the data and the original write region will become a write region again, as it was supposed to be.

We have seen automatic failover; now let's look at the manual failover.

### Manual failover

You can manually change the write region, depending on your needs.

For example, if you are aware that lots of people perform writes from India in the day time and your write region is in the United States, you can manually change the write region to India in IST time and change it back to the United States during the day time.

Although that's not suggested, you can also look over the few concepts of failover handling, such as the one I mentioned earlier, or based on constant traffic. It's totally up to you.
