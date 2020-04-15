---
title: Segment & Zendesk
date: "2020-04-14"
---

When properly integrated into your customer support knowledge base, Segment can provide some incredible insight into your customers' self-service experience. Let's integrate Segment with Zendesk Guide—it only takes a few minutes.

<!-- end -->

---
An integration with [Segment](https://www.segment.com/) involves throwing a couple of snippets of Javascript into your Zendesk Guide template, and then configuring destinations where Segment will ship the data for analysis or automation. For now, I'll focus on the integration. It's straight forward, but let's take it step by step. 

## Step 1 - Obtain Segment Snippet
If you haven't, [create a Segment account](https://app.segment.com/signup/). You'll be guided through the process of creating a workspace, and subsequently a project. Once you have a project, you can either follow the tutorial to "Install the Segment snippet on your website", or create a new source—a Javascript website, in this case. 

![Segment Snippet](./segment_snippet.png)

The Javascript snippet above is the first thing we're going to be adding to your Zendesk Guide template. Copy that snippet to your clipboard and proceed to _Step 2_. 

## Step 2 - Integrate Segment
In your Zendesk Guide admin panel, navigate to your theme settings (click the eye icon in the left-hand navigation), and click "edit code" to open the template editor. 

Select `document_head.hbs` and paste the entire snippet from _Step 1_ directly under any existing content. 

Once you've published the changes, you've successfully integrated Segment's Analytics.js snippet into your knowledge base! To verify, you can take a peek at the debugger in Segment. You should see `Page` events pop up anytime you navigate to a new page in your knowledge base.

![Segment Page Debug](./segment_page_debug.png)

## Step 3 - Add Identify Calls

Now that you can "see" when users navigate to new pages in your knowledge base, we need to figure out exactly who that user is (when possible). Segment uses something called an `Identify` call to associate activities with an individual. 

Navigate back to your Zendesk Guide theme code editor, and paste the following into the `script.js` file: 

```
document.addEventListener("DOMContentLoaded", function() {
    if (localStorage.ajs_user_id !== `"${HelpCenter.user.email}"` && HelpCenter.user.role !== 'anonymous') {
        analytics.identify(HelpCenter.user.email, {
            email: HelpCenter.user.email,
            role: HelpCenter.user.role,
            orgs: HelpCenter.user.organizations
        });
    }
});
```
_Disclaimer: I'm not a software engineer. This works, but I'm certain there are better ways it could be handled._

Simply put, the Javascript snippet above sends an update to Segment when a page loads after a user has logged in (at which point, they're no longer anonymous). 

For demonstration purposes, I chose to identify the user by their email, but it would be straightforward to use some other unique data to identify the user as well (such as the Zendesk User ID, or an ID from an external system). 

Once published, you can verify that that you are identifying users in Segment by checking out the debugger in Segment. 

![Segment Identify Debug](./segment_identify_debug.png)

## Discussion

While you're now collecting all this great user activity, you'll need to configure Destinations in Segment to start taking action on your data. I have a strong preference towards shipping the raw data to something like BigQuery, but if you're looking for something that requires less SQL, you can always just ship that data directly to something like Google Analytics. 

At this point, we're collecting both the identity of the user visiting your knowledge base, and the pages that they navigate to. Later, we'll discuss the addition of event tracking to further augment this dataset. 

Oh, and because Zendesk makes it annoying, <a href="https://gist.github.com/gm/036914a7434b693569c239a228851c66#file-get_zd_user_id" target="_blank">here's an example</a> of how you could get it Zendesk User ID via JQuery.