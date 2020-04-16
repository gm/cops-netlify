---
title: Tracking Article Ratings
date: "2020-04-15"
---

Article ratings are a great indication of whether or not your knowledge base articles are deflecting case submissions. Let's explicitly monitor for article ratings on both a generalized, and user specific basis.

<!-- end -->
---
Assuming you've gone through the process of integrating a tool like Segment into your knowledge base, the process of tracking article ratings is easy. 

There are manual ways to listen for clicks (e.g., add event listeners to buttons or links) on a given element of a page that will do exactly what we're doing via a GUI in this exercise, but that's worthy of it's own series of posts. In this case, we're going to use a beta feature from Segment called Visual Tagger to add tracking to your article ratings without the need to write a single line of code. 

## Visual Tagger

The Visual Tagger is available when you select a source in your Segment project. Once you've installed the required browser plugin, you're good to go. 

You'll likely need to log out of your Zendesk account in order prevent undesired redirects back to the Agent Console when opening the Visual Tagger. This can be done prior to opening Visual Tagger, or directly from the Visual Tagger interface. 

Once you see your knowledge base in the Visual Tagger, click _Add Event_, and navigate to an article page. To add an event for a postive article rating, click the _Add Event_ button, and simply click on the `Yes` button in the rating section. Name your event, make sure it only applies to article pages, and you're all set! Follow the same process with the `No` button. 

![Add Event](./add_event.gif)

After publishing, it can take a few minutes for your new events to load into knowledge base. Exercise a bit of patience (or grab a coffee), refresh you knowledge base, and then head over to the Segment Debugger to test them out. When you rate an article, you can expect to see the rating event in a matter of seconds. 

Keep in mind that if you've already logged in, the rating itself will be associated with the logged in user. This can be a great way to identify users that are having a particularly bad time with your docs. 

![Event Debug](./event_debug.gif)

## Discussion

Behind the scenes, this tool is updating the Javascript that is loaded from Segment each time a user loads your knowledge base. In fact, if you dig into the code itself, you'll quickly notice that Segment is simply added a GUI wrapper to the process of adding listeners to elements on the page—exactly the same as the manual method I mentinoed in the intro. 

For the sake of a pretty demo, I opted to deviate slightly from best practice here. If I were implementing this in a production environment, I'd take a slightly different approach to makes the data a bit cleaner for future analysis. Rather than firing an individual event for each a postive and negative rating, I'd likely craete a `articleRating` event with a parameter of `isHelpful = true/false`. This would allow me to more easily select all article rating for analysis in the future. 

![Event Improved](./event_improved.png)

One potential weakness of this approach is the fact that measuring clicks means that articles could theoretically be rated more than once (two clicks on `yes`) or could chnage over time (`yes` then `no`). If you're doing aggregate analysis of article ratings, you'll likely want to select only the most recent rating for a given user (as determined by either the contents of an `Identify` call, or using the `anonymousId` of a user who hasn't logged in). 



