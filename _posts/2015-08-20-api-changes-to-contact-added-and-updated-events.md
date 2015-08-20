---
layout: post
title:  "API changes to contact added and updated events"
date: 2015-08-20 12:10:00
author_name : Chris Sharkey
author_url : /author/chris
author_avatar: chrissharkey
show_avatar : false
read_time : 4
show_related_posts: true
---

As Autopilot developers, we are committed to maintaining a consistent interface for our API. Making breaking changes is something that we have and will avoid, you can see this by our namespacing of every API endpoint with a version number.

<br />
However, today we made a change to our API which splits our outgoing REST Hooks for contact record updates into separate **contact_added** and **contact_updated** events. Previously we only had **contact_added** events which would have **new_contact: true** set in the reply JSON for brand new contacts.

<br />
Now, **contact_added** will be triggered when a brand new contact comes into your Autopilot instance from any source (manual add, contact import, form submit, Segment identify, API call, Zapier call, etc), and **contact_updated** will be called when any change is made to an existing contact record.

<br />
For most of you, this will mean no change at all. We have reviewed our API usage and notified affected users. Our documentation has been updated to match the change and we encourage you to read the updates. For technical specifics and code examples, [please view our official documentation for this feature](http://docs.autopilot.apiary.io/#reference/rest-hook-events).
