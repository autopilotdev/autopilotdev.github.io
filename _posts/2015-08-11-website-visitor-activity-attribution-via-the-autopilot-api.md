---
layout: post
title:  "Website visitor activity attribution via the Autopilot API"
date: 2015-08-11 12:00:00
author_name : Chris Sharkey
author_url : /author/chris
author_avatar: chrissharkey
show_avatar : false
read_time : 4
show_related_posts: true
---

User attribution is a crucial factor in Marketing Automation. Autopilot currently attributes website visitor activity by either a form submission or clicking a link in an email. We track visitor activity anonymously and then once we identify who the user is, we assign that history to the user.

<br />
Through our customers, we are seeing a variety of additional ways in which to identify users that we would love to allow through our system. For this purpose, we have added a feature to our Autopilot tracking script and API which will let you identify users in any way you choose.
 
<br />
How it works is this:
 

1. Our tracking code looks for a hidden field on your web page with the name or id "_autopilot_session_id". (Also available in JS via: var sessionId = AutopilotAnywhere.sessionId;)

2. We insert a value into that field which allows us to identify the anonymous user on our end.

3. You store that value however you like: through AJAX to your server, through a form submission, etc.

4. When you submit your next "Add Contact" API call, which adds or updates a contact, include the _autopilot_session_id value to have that user's activity history associated with the contact you are adding or updating/identifying.

5. From this point on, all activity for this user on your website will be recorded against this contact identity.
 
<br />
This means that even if you don't use Autopilot's form submission tool, you are still able to identify contacts and track their activity. It also gives you more opportunity to identify known users.
 
<br />
For technical specifics and code examples, [please view our official documentation for this feature](http://docs.autopilot.apiary.io/#reference/api-methods/addupdate-contact), see the section "Associating Sessions".
