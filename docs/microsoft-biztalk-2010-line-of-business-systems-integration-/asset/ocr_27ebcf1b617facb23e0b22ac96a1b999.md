Once we have the root nodes we use the Import property of BizTalk Server schemas to import the two update schemas into the composite one:

![](bbox=[0.244, 0.192, 0.756, 0.389])

With our schemas imported, we need to ensure that all parent nodes have the correct Data Structure Type specified. As you can see in the following image, we have two Insert nodes underneath the Request root node. Within each of these Insert nodes we provide the Data Structure type for our SpecialOffer request and SpecialOfferProduct request. Similarly, we do the same for RequestResponse nodes:

![](bbox=[0.27, 0.517, 0.729, 0.8])