---
layout: post
title:  "Email no longer a required field"
date: 2015-07-01 09:20:00
author_name : Chris Sharkey
author_url : /author/chris
author_avatar: chrissharkey
show_avatar : false
read_time : 2
show_related_posts: true
---

Until today, **Email** was a required field when adding contacts via the API. This is no longer the case. We decided that since Autopilot is a multi-channel marketing tool, that it didn't make sense to assume that everyone would just have email marketing in mind when using our API. This applies to both the "add or update" contact method, and the bulk add contacts method.

<br />
We use **Email** as a way to de-duplicate contacts in our system. If you try to add a contact with the same email using the API, that contact will be overwritten. That is, the contact's contact details will be overwritten, not its activity feed history or statistics. 

<br />
We use this to prevent duplicate email addresses ever being added to this system, this acts as a de-duplicate method to prevent having two of the same contact.

<br />
If you don't provide an email - let's say you just provide **FirstName**, **LastName** and **Phone** - then it will be possible to have duplicates as it stands. It will be up to you to ensure that you do not create duplicates.

<br />
We have plans to overcome this limitation in the very near future by allowing you to specify a **uniqueID** field, which will allow you to provide your own method of updating / de-duplication without having to rely on the **Email** field. Subscribe to our API Changelog (widget below) to be notified when we make changes to our documentation or post here, to be notified when this change is rolled out.
