---
layout: post
title:  "Getting contacts on a list with bookmarks"
date: 2015-06-23 14:24:00
author_name : Chris Sharkey
author_url : /author/chris
author_avatar: chrissharkey
show_avatar : false
read_time : 5
show_related_posts: false
---

Most of the API calls in the Autopilot API are simple one-and-done propositions, but getting all of the contacts on a list can be a little trickier. 

<br />
Lists in Autopilot can contain a couple, thousands, hundreds of thousands or millions of contacts. For this reason, we can't send back all of the contacts at once, or we might blow up your JSON parser. 

<br />
To this end, we have a "bookmark" system, whereby your initial call to get a list of contact contains the contacts themselves, the total number of contacts on the list, and a bookmark value, which you can use to get the next "page" of contacts. Here is an example from my chrissharkey instance:

<br />
**REQUEST**

<br />
chris-mac-2:~ chris$ curl --include      --header "autopilotapikey: 2abb8f8beaff4c2cb4dcfde8a28f0b66"   'https://api2.autopilothq.com/v1/list/contactlist_F342AF0A-1916-43AE-8F06-3F78DDC55E9B/contacts'

<br />
**RESPONSE**

<br />
HTTP/1.1 200 OK
Last-Modified: Tue, 23 Jun 2015 17:43:23 GMT
Content-Type: application/json
Content-Length: 97826
Date: Tue, 23 Jun 2015 17:43:27 GMT
Connection: keep-alive

<br />
{"contacts":[{"_rev":"11-b94fedc81f256ba1332a93faa43fe2c7","company_priority":false,"Name":"6671 20k","LastName":"20k","FirstName":"6671","Twitter":"","Email":"20k_0006671@mailinator.com","Company":"","lists":["contactlist_F342AF0A-1916-43AE-8F06-3F78DDC55E9B"],"created_at":"Tue, 12 May 2015 21:41:31 GMT","updated_at":"Fri, 19 Jun 2015 23:48:16 GMT","contact_id":"person_012596B2-8477-4744-9D3E-45E2903B56C4"},{"_rev":"12-209fdb69451e0df8f2a7376d7686e6c0","company_priority":false,"Name":"13356 20k","LastName":"20k","FirstName":"13356","Twitter":"","Email":"20k_0013356@mailinator.com","Company":"","lists":["contactlist_F342AF0A-1916-43AE-8F06-3F78DDC55E9B"],"created_at":"Tue, 12 May 2015 21:32:58 GMT","updated_at":"Fri, 19 Jun 2015 23:43:25 GMT","contact_id":"person_0125BFEA-1B7B-4D38-9A1F-184297170EDB"}],"total_contacts":20000,"bookmark":"person_012596B2-8477-4744-9D3E-45E2903B56C4"}

<br />
So as you may be able to tell, I have snipped down the 100 contacts that this API call returned to two, just for the interests of saving space on the internet. As you can see above, this list contains 20000 contacts, we can tell that because the *total_contacts* value says so, and directly after it, the bookmark, which is *person_012596B2-8477-4744-9D3E-45E2903B56C4*

<br />
So now, since I have 20000 contacts on this list, I want to get the next 100. To do so, I adjust my request to contain the bookmark on the end of the URL, like so:

<br />
**REQUEST**

<br />
chris-mac-2:~ chris$ curl --include      --header "autopilotapikey: 2abb8f8beaff4c2cb4dcfde8a28f0b66"   'https://api2.autopilothq.com/v1/list/contactlist_F342AF0A-1916-43AE-8F06-3F78DDs/person_012596B2-8477-4744-9D3E-45E2903B56C4'

<br />
This then returns the next 100 contacts on the list. You can repeat that process as many times as is needed to retrieve all of the contacts on the list.

<br />
Obviously for very large lists, 10,000 and above, this will be way too slow since the calls need to be made sequentially. Please contact us if you have a requirement for this and we will give you access to additional methods which will allow larger batch sizes, or streaming. 

<br />
The official documentation for this method is available on Apiary here: [http://docs.autopilot.apiary.io/#reference/api-methods/get-contacts-on-list](http://docs.autopilot.apiary.io/#reference/api-methods/get-contacts-on-list)
