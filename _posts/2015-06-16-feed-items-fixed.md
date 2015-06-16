---
layout: post
title:  "Feed items fixed"
date: 2015-06-12 17:00:28
author_name : Chris Sharkey
author_url : /author/chris
author_avatar: chrissharkey
show_avatar : false
read_time : 2
show_related_posts: false
---

You may have noticed that adding contacts via the Autopilot API was resulting in feed items showing "added or updated via Zapier". This was happening because our Zapier integration is built on top of our API and, until today, we weren't distinguishing between Zapier requests and regular API requests.

<br />
We have now updated the system to show "via the API" instead of "via Zapier" so this will alleviate any confusion around the feed items. Existing contacts which were added by the API will still show that they were added via Zapier.

<br />
These changes should be rolled out some time today (Tuesday 16th June, 2015). If you still notice Zapier items appearing and you expect API ones after tomorrow, please email us!

