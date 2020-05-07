---
title: Draft - Support Email Tracking
date: "2020-05-03"
---

Curious to know if your customers are actually reading the emails you're sending them? Tracking pixels are commmonly used in marketing—and they're easy enough to implement in a support context as well. 

<!-- end -->

Monitoring when a user views of a page in your support portal is straight forward, but the JavaScript based approach tradtionally used in such scenarios simply isn't an option when it comes to emails. In this example, I'll demonstrate how you can use Zendesk and Segment to track when a customer reads an email from your support tool by embedding what's commonly referred to as a _tracking pixel_. 

## Tracking Pixels

Tracking pixels are quite cool. Tradtionally, they're 1x1 transparent images that you embed in something like an email. Browsers needs to fetch (i.e., GET) all images—including tracking pixels—when something like an email is opened, which means that the host of the image can keep an eye on activity related to that image to determine when the email is viewed by the recipient.  


```
<img src="https://api.segment.io/v1/pixel/track?writeKey=6BvvVzTMFYmqISKFnC8Jj3WcUUDpMdK0&userId={{ticket.requester.id}}&event={{"Email Opened" | url_encode}}&properties.updated_at={{ticket.updated_at_with_timestamp}}&properties.email={{ticket.requester.email | url_encode}}">
```

## Email Templates

When you add a _comment_ in Zendesk, it doesn't necessarily generate an email notification to the requester of the case. Instead, Zendesk leaves it up to your trigger logic to determine the recipients of any case related notifcations. The default Zendesk notification trigger sends  notification to both the requester (the end-user that created the ticket in most cases), and anybody else cc'd on the ticket. In this case, we only want ot know when the requester opens the email, so we'll need to change this to only email the requester and create another trigger to email the cc's. 

Once the cc's have been removed, you can add your pixel to the notification template as shown here: 

![Train Tracks](./traintrack.jpg)

## Verification

Once your trigger is updated, send your email from Zendesk, and check the Segment debugger to verify that your pixel is working. 

![Train Tracks](./traintrack.jpg)

## Implications

At some level, it doesn't really matter when someone opens their support email as long as their problem is resolved. Behind the scenes, though, you can use this information to streamline both the end user, and agent support experience. 

Potential Use Cases:
- Sending a reminder to a customer that hasn't read the response from the support team. 
- Prioritizing issues that are being read more frequently.
- Optimizization of agent distribution based on when your customers are active. 
- Closing requests with long periods of no activity. 

Keep in mind that this is only part of the picture when it comes to user activity. 