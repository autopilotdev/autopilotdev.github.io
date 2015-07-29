---
layout: post
title:  "Time after timeout"
date: 2015-07-28 22:00:00
author_name : Chris Sharkey
author_url : /author/chris
author_avatar: chrissharkey
show_avatar : false
read_time : 4
show_related_posts: true
---

Last night we had 3 clients experience issues with timeouts when making calls to our API. This is something which the Autopilot team did not expect, since API calls receive the second highest priority in our system.

<br />
It is worth noting that even in a timeout situation on the Autopilot API, your request will still be completed. We issue a timeout to protect the open connections on the API servers, rather than to say that the task itself has failed. Our system is fault tolerant and will make sure it completes a task unless there is an outright error in the input it receives.

<br />
The cause of the timeouts on Monday night was our system doing work that it didn't have to, delaying our responses to API consumers. Each server in our cluster contains a "core" process which handles giving out work to the hundreds of worker nodes which it controls. These "core" processes were mistakenly parsing all the input they received for bulk requests, and since they each run on a single thread, were delayed in performing their primary duty of giving tasks to workers and sending back responses to clients.

<br />
We hadn't hit this issue before because most of our API and Zapier clients use the single add/update contact method rather than the bulk API. Heavy use of our Bulk API exacerbated this issue and meant that even though we had plenty of worker nodes available to fulfill API requests, they couldn't be given the work fast enough since the "core" nodes were busy doing unnecessary work.

<br />
To remedy this situation we have taken the following action:


- Removed the code which was doing the unnecessary work.

- Added automated testing to detect situations like this in the future.

- Informed the clients who experienced this issue of the problem and resolution.

At Autopilot, we value our API clients and will strive to continue to provide the best platform for marketing automation. We're sorry that this situation occurred, and will continue to work to anticipate any such errors in the future to prevent any issues for our clients.
