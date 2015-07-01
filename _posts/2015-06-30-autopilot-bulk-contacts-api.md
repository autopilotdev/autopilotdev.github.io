---
layout: post
title:  "Autopilot Bulk Contacts API"
date: 2015-06-30 15:55:00
author_name : Chris Sharkey
author_url : /author/chris
author_avatar: chrissharkey
show_avatar : false
read_time : 5
show_related_posts: false
---

Unsuprisingly, many of our API users are using the API to load large amounts of contacts into Autopilot. In the interests of having our API available sooner we had left out bulk operations, but the downside of this is that our API consumers are having to send hundreds of thousands of API requests to add as many contacts.

<br />
Those dark days are over! Today we have rolled out our Bulk Contacts API which allows you to add up to 100 contacts in a batch.

<br />
To do this, you simply combine the contacts JSON documents in a JSON array. The easiest way to show this is with an example:

<br />
**REQUEST**

<br />
<pre>
curl --include \
     --request POST \
     --header "autopilotapikey: 660f9d61d936487db0d78d546f423b43" \
     --data-binary "{ 
    \"contacts\": [
        {
            \"FirstName\": \"Slarty\",
            \"LastName\": \"Bartfast\",
            \"Email\": \"test@slarty.com\",
            \"custom\": {
                \"string--Test--Field\": \"This is a test\"
            }
        },
        {
            \"FirstName\": \"Jerry\",
            \"LastName\": \"Seinfeld\",
            \"Email\": \"jerry@seinfeld.com\"
        },
        {
            \"FirstName\": \"Elaine\",
            \"LastName\": \"Benes\",
            \"Email\": \"elaine@seinfeld.com\"
        }
    ]

} " \
'https://api2.autopilothq.com/v1/contacts'
</pre>

<br />
**RESPONSE**

<br />
<pre>
HTTP/1.1 200 OK
Last-Modified: Tue, 30 Jun 2015 22:05:04 GMT
Content-Type: application/json
Content-Length: 155
Date: Tue, 30 Jun 2015 22:05:10 GMT
Connection: keep-alive
</pre>

<br />
<pre>
{"contact_ids":["person_247803ED-FD9B-40EF-810A-3EB086400AA6","person_C2EA6D9C-F10E-4542-8FDB-F3B8E7946A64","person_66A7DE87-6139-4BAB-86A8-798D8EB20C4B"]}
</pre>


<br />
If you try to provide more than 100 contacts you will receive a status code of 400 for bad request. This limit is in the interest of avoiding timeouts on large adds. While Autopilot will handle them ok, there is a chance that it will take too long to reply to the HTTP requests and hit a timeout in common libraries. We want to keep things simple so we have set a limit for now.

<br />
The official documentation for this method is available on Apiary here: [https://jsapi.apiary.io/previews/autopilot/reference/api-methods/bulk-add-contacts](https://jsapi.apiary.io/previews/autopilot/reference/api-methods/bulk-add-contacts)
