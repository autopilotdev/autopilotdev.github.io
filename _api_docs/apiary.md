FORMAT: 1A
HOST: https://api.autopilothq.com

# Autopilot REST API Documentation
Autopilot is easy-to-use software for multi-channel marketing, it enables marketers to create marketing journeys and convert  
their leads into customers.

The Autopilot REST API is designed to allow you to integrate your app with the Autopilot platform both to improve and  
customize your experience in Autopilot, and improve the data being pushed into your app. Example applications of the API  
include:

# @TODO: hasMore with start key, allow _all_contacts and _all_contacts_and_companies
# @TODO: Document rate limiting
# @TODO: Document 405 Method Not Allowed for valid urls which don't support a particular type

- Sending leads from Autopilot in your own app
- Adding contacts from your app into Autopilot
- Triggering Autopilot journeys from your app
- Keeping unsubscribes in sync between Autopilot and your app
- Adding to the Autopilot activity feed
- Enhance contact profiles with custom fields and data from your app
- Get data from your app into and out of Salesforce using Autopilot's deep integration

Of course, there are many other uses for the Triggers and Actions provided below, and we invite you to use them in any way  
which enhances your usage of Autopilot. This is a new API and as such we [welcome your suggestions](http://docs.autopilot.apiary.io/#autopilotappbasics)
on how it can be improve and what you would like to see added.

If you have any question about how to accomplish something specific with our API, please [submit a Zendesk ticket](http://docs.autopilot.apiary.io/#autopilotappbasics) as we will  
be able to advise you on how to accomplish what you are after.

The document you are currently reading is the Autopilot API Documentation. More information, example apps, and the  
Autopilot developer blog can be found on the [Autopilot Developer Portal](http://developers.autopilothq.com).

We also have a [Zapier integration](http://developers.autopilothq.com), enabling integration of your Autopilot account with many web applications and CRMs  
which you are already familiar with. To read more about our [Zapier integration](http://developers.autopilothq.com), please visit the [Autopilot Developer Portal](http://developers.autopilothq.com).

We have tried to keep the Apiary docs closely coupled with the functionality our API provides, to read about other topics related to the API, please see the following:

- Zapier Integration
- Rate Limits
- API Features coming soon
- More topics
- How to update existing contacts
- How to create and set custom fields
- How to trigger Autopilot Journeys from the API

# Group Authentication

### Authentication

Autopilot uses HTTP Basic Authorization over HTTPS to identify your requests. When Autopilot receives your request, it checks  
the API key and if it does, your request it fulfilled. 

### Getting an API key

To get an API for your Autopilot account:

1. Login to your Autopilot account at https://login.autopilothq.com  
2. Go to the Settings section  
3. Go to "API Keys"  
4. Click "Generate"
5. Copy & Paste the API key

If you don't already have an Autopilot account, you can sign up for a free trial here https://autopilothq.com

### Protecting your API key

Your API key can be used to control your account, so you need to make sure that you protect it. Simple ways to avoid
your API key being accessed by anyone else are:

* Always use HTTPS when accessing the Autopilot REST API (it is all we accept anyway)
* Do not send the API key over email
* Do not store the API key in your version control system

The safest way to use your API key is to store it in your database or on a file on your web server and access it at 
runtime in your application.

### Regenerating your API key

If your API key is every accidentally made public or put in a place where it could be accessed by an untrusted
source, you can regenerate it to make a new one.

To do so:

1. Login to your Autopilot account at https://login.autopilothq.com  
2. Go to the Settings section  
3. Go to "API Keys"  
4. Click "Regenerate"  
5. Copy & Paste the API key

Note that this will cause your previous API key to stop working immediately.

### Apiary mocking server

Please feel free to take advantage of Apiary's built-in examples and mocking server. Please note though that when using the  
live API, that you will need to provide the `Authorization` header as described below. Apiary doesn't support Authentication  
at the moment, so you will be able to do all of your test requests without providing an API key.

### Feed Items

For every API request that you make which is associated with a contact, an activity feed item is added for that contact.

Here are some examples:

(example1)
(example2)

# Group Triggers

Triggers answer the question: *What events can I listen for coming out of Autopilot?*

A trigger is fired any time an event listed below happens, for which you have a "REST Hook" set up. A REST Hook is a URL endpoint which
you provide using the `/v1/hook` method, it will have data POSTed to it when the event occurs.

## Adding a hook


## Supported Events

Note that in all of these events, both the `contact_id` and `email` are included for the contact in the
response in case you prefer to key off email address instead of our internal id. This prevents forcing you to make an 
additional API call to GET the contact just to find out the email address from the `contact_id`.

### Contact added (Event key: contact_added)

Any time a contact is added to or updated in Autopilot, this event fires.

Contacts can be added to Autopilot via a variety of methods:

* Manually added
* Spreadsheet import
* Salesforce sync
* API Call (including your own)
* Zapier event
* SegmentIO event

***Rate***

It is possible that there will be 10s of 1000s of contact_added events which happen in a small space of time, this 
would happen in the case of a spreadsheet import or a Salesforce sync. You need to make sure that your system is 
capable of handling these volumes.

***Retries***

If your REST Hook url is unavailable, returns an error code (anything other than 200 OK), the Autopilot API will retry
up to 3 times to get the request through. After this, the request will be lost.

***Updated flag***

One crucial thing about the contact_added event is that it happens **when contacts change too**. Autopilot contacts are
uniquely keyed off email address such that no two contacts can have the same email address. For this reason, you need to
check to see if you have a contact with a given email address 

```
{
  "event": "contact_added",
  "contact": {
  }
}
```


### Contact unsubscribes (Event key: contact_unsubscribes)

This event is triggered when a contact is added to a list.

The main ways this event could be triggered are: 

* The contact is manually added to the list using the contacts section of the Autopilot UI
* A journey has a step which adds contacts to a list and the contact reaches it
* An API call adds the contact to a list
* Contacts are imported to the list

Note that both the `contact_id` and `email` are included for the contact in case you prefer
to key off email address instead of our internal id. This prevents forcing you to make an 
additional API call to GET the contact just to find out the email address from the `contact_id`

```
{
  "event": "contact_added_to_list",
  "contact_id": "person_ED75BA78-2405-4564-B24C-F2B8F936C7C6",
  "email": "chris@autopilothq.com",
  "list_id": "contactlist_ED75BA78-2405-4564-B24C-F2B8F936C7C6"
}
```

### Contact Added To List (Event key: contact_added_to_list)

This event is triggered when a contact is added to a list.

The main ways this event could be triggered are: 

* The contact is manually added to the list using the contacts section of the Autopilot UI
* A journey has a step which adds contacts to a list and the contact reaches it
* An API call adds the contact to a list
* Contacts are imported to the list

```
{
  "event": "contact_added_to_list",
  "contact_id": "person_ED75BA78-2405-4564-B24C-F2B8F936C7C6",
  "email": "chris@autopilothq.com",
  "list_id": "contactlist_ED75BA78-2405-4564-B24C-F2B8F936C7C6"
}
```

### Contact Removed From list (Event key: contact_removed_from_list)

This event is triggered when a contact is removed from a list.

Note that the `contact_removed_from_list` event will not be triggered if the list itself is deleted. Likewise, if
the contact is deleted this event will not be triggered.

The main ways this event could be triggered are: 

* The contact is manually removed from the list using the contacts section of the Autopilot UI
* A journey has a step which removes contacts from a list and the contact reaches it
* An API call removes the contact from a list

```
{
  "event": "contact_removed_from_list",
  "contact_id": "person_ED75BA78-2405-4564-B24C-F2B8F936C7C6",
  "list_id": "contactlist_ED75BA78-2405-4564-B24C-F2B8F936C7C6"
}
```

### Contact Entered Segment (Event key: contact_entered_segment)

This event will be triggered when a contact "enters" a segment, that is, a change is made to a contact
that means it meets the segment criteria.

```
{
  "event": "contact_entered_segment",
  "contact_id": "person_ED75BA78-2405-4564-B24C-F2B8F936C7C6",
  "email": "chris@autopilothq.com",
  "segment_id": "contactlist_ssegED75BA78-2405-4564-B24C-F2B8F936C7C6"
}
```

### Contact Left Segment (Event key: contact_left_segment)

This event will be triggered when a contact "leaves" a segment, that is, a change is made to a contact
that means it no longer meets the segment criteria.

{
  "event": "contact_left_segment",
  "contact_id": "person_ED75BA78-2405-4564-B24C-F2B8F936C7C6",
  "email": "chris@autopilothq.com",
  "segment_id": "contactlist_ssegED75BA78-2405-4564-B24C-F2B8F936C7C6"
}

# Group Actions

Actions are API methods which allow you to actions in your Autopilot instance. Actions answer the question
*"How do I get data into Autopilot and trigger journeys"*.

**Action Rate Limits**

We have put some basic throttling in place for version 1 of the Autopilot API. Currently, we limit you to 5 API
calls per second. This limit was set more as a sanity limit than as a way to actually stop you from interacting
with our system at the rate you desire.

The Autopilot API, as with the rest of Autopilot is *horizontally scalable*. This means that as we start to hit
performance capacity, we can simply add new nodes to the relevant service to be able to handle higher rates of
requests. Over time, we will expand the nodes dedicated to the API and increase our API limits if we need to.
Please *contact us* if you think the rates are too low for you, we will work with you to selectively increase them
to get the right balance for us both.

One thing to note is that we currently do not have any Bulk API operations such as adding, deleting or retrieving
contacts in bulk. This was done deliberately so that we could have the API out sooner. We will be adding bulk
operations in the future.

**Apiary Mock Server**

Please feel free to use the Apiary mocking server which is provided as part of this documentation. While it will
help you to make sure that you are calling the correct URLs, it doesn't handle Authentication and won't give the
correct response codes to you based on the system actually doing work.

A better method for testing with our system while you integrate is to simply *sign up for a trial account*, generate
an API key, run all your tests using that instance and key, and then switch over to your company's account when
you are ready to go live. If you let us know when you are done with your test instance we can delete it.

# REST Hook [/v1/hook]

## Register a REST Hook [POST]

Rather than having your system continuously poll to find out when there is new information to trigger from, we allow you
to register a URL which we will call when these events occur, called a "REST Hook". Whenever an event you have registered
for occurs, for example a new contact being added to Autopilot, we will send a HTTP POST to your URL with information about
that event for your system to act upon. 

The **triggers** section of this document describes the available events and the data which they will send through, the currently
supported triggers are:

- `contact_added`
- `contact_unsubscribes`
- `contact_added_to_list`
- `contact_removed_from_list`
- `contact_entered_segment`
- `contact_left_segment`

To register a new REST Hook, you need to sent a post request to `/v1/hook`, with the following parameters:

- `event` - This is the trigger event name that you wish to be told about.
- `target_url` - This is the URL in your API which you want our API to POST to when this event occurs.

When you request is valid, you will receive a *201 Created* status. From this point on, when the event you register for happens, your
URL will be POSTed to with the event data. Please read the relevant **trigger** section to see the data which you will receive.

You may register more than one `target_url` for a given event, when the event occurs *each* of the registered target URLs will be
called with the event data. You may not, however, register the same `target_url` more than once, if you try you will receive a *409 Conflict*.

When your `target_url` is POSTed to, our system will react in different ways depending on how your system responds:

1. If we gets a `410 Gone`, we will delete your REST Hook from our system and not call it again.
2. If we get a timeout, we will try again up to 3 times, in reasonably quick succession, eventually giving up.
3. If we get a status which **is not** between `200-204`, we will try again up to 3 times, in reasonably quick succession, eventually giving up.
4. If we get a status which **is** between `200-204`, then we have a successful call.

In cases **2.** and **3.**, once the request has failed 3 retries, then the trigger event is lost and will not be replayed. If is
your responsibility to make sure that once registered, your target URL is successfully responding. If your system has trouble staying 
online, or keeping up with the pace of requests, then a good design pattern is to have a dedicated service to receive and buffer the 
requests to a volatile (Redis) or non-volatile (Postgresql, MongoDB) database, and then have a second service which picks up those
requests and processes them at a pace which your system can handle.

+ Request (application/json)

        {
            "event": "contact_added",
            "target_url": "https://myapi.mycompany.com/ap14112546"
        }
            
+ Response 201 (application/json)

        {"id": "hook_ED75BA78-2405-4564-B24C-F2B8F936C7C6"}

+ Response 400 (application/json)

        {"error": "Bad Request", "message": "No autopilotapikey header provided."}

+ Response 401 (application/json)

        {"error": "Unauthorized", "message": "Provided autopilotapikey not valid."}

+ Response 429 (application/json)

        {"error": "Too Many Requests", "message": "You have exceeded your request rate of 5 r/s."}

+ Response 500 (application/json)

        {"error": "Internal Server Error", "message": "(various)"}

# REST Hook [/v1/hook/{id}]

## Unregister a REST Hook [DELETE]

When you no longer wish your `target_url` to be POSTed to when the registered Autopilot event happens, you may unsubscribe
an endpoint URL by providing the `id` which was given to you when you registered.

If you no longer have the `id` available, just send through the `target_url` in place of `id` and we will look it up and
delete it based on URL for you.

If you've REALLY forgotten everything and you just want to remove any `target_url`s which are listening to a particular event,
pass through the event name, e.g. `contact_added` through as the `id`, and we will delete all associated URLs for this event.

A `204` response means that the deletion was successful as requested. All other response are described by the error message
that you will receive, and are also detailed below.

+ Response 204

+ Response 400 (application/json)

        {"error": "Bad Request", "message": "No autopilotapikey header provided."}

+ Response 401 (application/json)

        {"error": "Unauthorized", "message": "Provided autopilotapikey not valid."}

+ Response 429 (application/json)

        {"error": "Too Many Requests", "message": "You have exceeded your request rate of 5 r/s."}

+ Response 500 (application/json)

        {"error": "Internal Server Error", "message": "(various)"}

# Contact [/v1/contact]

## Add or update contact [POST]

Autopilot uses a document based database. This means that a contact document is simply one which conforms
to a particular set of parameters. As such, it is possible to add and store any additional data in a 
contact document that you like. 

For custom fields to be used in Autopilot itself, however, you need to do the following:

1. Add a custom field in Autopilot (*Help Guide*)
2. Add the field to a `custom_fields` hash in the contact document

+ Response 400 (application/json)

        {"error": "Bad Request", "message": "No autopilotapikey header provided."}

+ Response 401 (application/json)

        {"error": "Unauthorized", "message": "Provided autopilotapikey not valid."}

+ Response 429 (application/json)

        {"error": "Too Many Requests", "message": "You have exceeded your request rate of 5 r/s."}

+ Response 500 (application/json)

        {"error": "Internal Server Error", "message": "(various)"}

# Contact [/v1/contact/{contact_id_or_email}]

## Get contact [GET]

**Understanding the format of a contact document**

If you need to get a group of contacts, the most efficient thing to do is use the **Bulk get contacts**
method described below. This will use less API calls and be more efficient in terms of speed and HTTP
overhead. 

+ Parameters
    + contact_id_or_email (string) ... Either the Autopilot contact_id e.g. `person_xxxxxx`, or the contact's email address.

+ Response 200 (application/json)

        {"message": "Could not find contact"}

+ Response 404 (application/json)

        {"error": "Not Found", "message": "Could not find contact."}

+ Response 400 (application/json)

        {"error": "Bad Request", "message": "No autopilotapikey header provided."}

+ Response 401 (application/json)

        {"error": "Unauthorized", "message": "Provided autopilotapikey not valid."}

+ Response 429 (application/json)

        {"error": "Too Many Requests", "message": "You have exceeded your request rate of 5 r/s."}

+ Response 500 (application/json)

        {"error": "Internal Server Error", "message": "(various)"}

## Delete a contact [DELETE]

This method will remove the contact from your database and stop them from continuing in any journeys, 
being in any segments or lists, and receiving emails, SMS and other communications. If you simply 
wish to **unsubscribe** a contact, or **eject a contact from a journey** then you should use the 
***unsubscribe** and **eject** methods below for a more appropriate approach.

Deleting a contact is permanent, and there is no way to undo it. At Autopilot we keep backups on an 
hourly, daily, weekly and monthly basis. However, when we restore the data from the backup, you lose 
everything which has transpired since the time the backup was created. 

To delete a contact you need to provide the `:contact_id_or_email` parameter. This allows you to specify
an Autopilot `contact_id` such as `person_9EAF39E4-9AEC-4134-964A-D9D8D54162E7` or an email address. If
there is more than one contact with that email address in the database, they will all be deleted.

On success you will receive a `204 No Content` response. If the contact you are trying to delete doesn't
exist because they have already been deleted, or never existed, you will receive a `404 Not Found` response.

+ Response 204 (application/json)

+ Response 404 (application/json)

        {"error": "Not Found", "message": "Could not delete contact as contact could not be found."}

+ Response 400 (application/json)

        {"error": "Bad Request", "message": "No autopilotapikey header provided."}

+ Response 401 (application/json)

        {"error": "Unauthorized", "message": "Provided autopilotapikey not valid."}

+ Response 429 (application/json)

        {"error": "Too Many Requests", "message": "You have exceeded your request rate of 5 r/s."}

+ Response 500 (application/json)

        {"error": "Internal Server Error", "message": "(various)"}

# List [/v1/list]

## Add list [POST]

Lists are used in Autopilot to group contacts together. A contact can belong to as many lists as you
like. This method allows you to add a new list. It will appear in the tree in the Contacts section
of Autopilot, and you can use the returned `list_id` to add any contacts you like to it by `contact_id`
or `email`.

+ Response 200 (application/json)

        { "list_id": "contactlist_98a45928946bc74e8a5c8c2d7ba30fefe7fd7445" }

+ Response 409 (application/json)

        {"error": "Conflict", "message": "A list with this name already exists."}

+ Response 400 (application/json)

        {"error": "Bad Request", "message": "No autopilotapikey header provided."}

+ Response 401 (application/json)

        {"error": "Unauthorized", "message": "Provided autopilotapikey not valid."}

+ Response 429 (application/json)

        {"error": "Too Many Requests", "message": "You have exceeded your request rate of 5 r/s."}

+ Response 500 (application/json)

        {"error": "Internal Server Error", "message": "(various)"}

## Delete list [DELETE]

+ Response 204 (application/json)

+ Response 403 (application/json)

        {"error": "Forbidden", "message": "Could not delete the list as it it is being used in a journey or segment."}

+ Response 404 (application/json)

        {"error": "Not Found", "message": "Could not delete the list as it could not be found."}

+ Response 400 (application/json)

        {"error": "Bad Request", "message": "No autopilotapikey header provided."}

+ Response 401 (application/json)

        {"error": "Unauthorized", "message": "Provided autopilotapikey not valid."}

+ Response 429 (application/json)

        {"error": "Too Many Requests", "message": "You have exceeded your request rate of 5 r/s."}

+ Response 500 (application/json)

        {"error": "Internal Server Error", "message": "(various)"}

# Automation [/v1/journey/{journey_id}/contact/{contact_id_or_email}]

## Eject contact from Journey [DELETE]

There are many situations in which you wish remove a contact from a journey before they pass through
all of the steps. An example of this is when a customer buys your product, you would not want to 
continue them in a lead nurturing program to turn them into a customer, so you eject them from
the journey. You may also have reasons occuring in your own app which require you to eject someone
from a journey.

To do so, you need to know the `journey_id` of the journey in Autopilot. There is currently no way to get this
programmatically. Please pester us at `support@autopilothq.com` if you need such functionality. However
in the short term, here is how to obtain the `journey_id` for one of your journeys:

1. Login to your Autopilot account
2. Go the the journeys section
3. Select the kourney that you wish to eject contacts from in your API call
4. In the URL in the address bar, copy everything after the last slash, e.g. `campaign_7BF91FA3-459D-4712-B8E9-8A85161800B6`

The reason the identifier is prefixed with `campaign_` and not `journey_` is because we changed our naming.
It has taken great mental fortitude on our behalf to switch to the new and exciting "journey" moniker
when we speak about Autopilot internally. 

As with other API calls, we provide you with the ability to provide either an Autopilot `contact_id` or an
email address as the `:contact_id_or_email` parameter. If you provide an email address, any contact with 
that email address will be ejected, however due to us merging/de-duping contacts based on email address, 
it is unlikely that there will ever be more than one contact with a given email address.

If the journey you supply does not exist, or the contact is not on that journey (or does not exist), you
will receive a `404` status in the response. On success you will receive a `204` status code with no body 
content. This is us being fancy with our knowledge of HTTP status codes, and I hope it impresses you.

+ Response 204 (application/json)

+ Response 404 (application/json)

        {"error": "Not Found", "message": "Could not delete the contact from the journey as the journey could not be found."}

+ Response 404 (application/json)

        {"error": "Not Found", "message": "Could not delete the contact from the journey as the contact was not on that journey."}

+ Response 400 (application/json)

        {"error": "Bad Request", "message": "No autopilotapikey header provided."}

+ Response 401 (application/json)

        {"error": "Unauthorized", "message": "Provided autopilotapikey not valid."}

+ Response 429 (application/json)

        {"error": "Too Many Requests", "message": "You have exceeded your request rate of 5 r/s."}

+ Response 500 (application/json)

        {"error": "Internal Server Error", "message": "(various)"}
