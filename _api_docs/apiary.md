FORMAT: 1A
HOST: https://api.autopilothq.com

# Autopilot API and Developer Guide
Autopilot is the Automation Layer for Marketing. It consists of an extensive suite of marketing 
tools and integrations which empower marketers to drive qualified leads into their sales funnels, 
segment customers, and automate their marketing. 

The [Autopilot Developer Portal](http://developers.autopilothq.com) is available here: [http://developers.autopilothq.com](http://developers.autopilothq.com)

This API is designed to allow you to add new tools to the Autopilot platform, you might want to do
this if:

- Your company wants to use Autopilot but you need to integrate parts of your own backend
- You have a service which you think would be valuable to Autopilot users and want to sell it or otherwise make it available
- You want an easier way to control Salesforce through a sensible API

# Group Quick Start

If you're interested in building an app for Autopilot, you need to do the following steps
as a minimum:

1) Sign up for a trial account if you don't already have one (http://www.autopilothq.com)

2) Read [Autopilot App Basics](http://docs.autopilot.apiary.io/#autopilotappbasics), 
[Creating an app in Autopilot](http://docs.autopilot.apiary.io/#creatinganappinautopilot) and 
[Add an Ingredient to your App](http://docs.autopilot.apiary.io/#addinganingredienttoyourapp) below.
You'll need at least one ingredient in order to make your app useful.

3) Read the section on your favourite language's client library, we have libraries for:

* [Node.js](http://docs.autopilot.apiary.io/#nodejsclientlibrary)
* [PHP](http://docs.autopilot.apiary.io/#phpclientlibrary)
* [Ruby](http://docs.autopilot.apiary.io/#phpclientlibrary)
* [Python](http://docs.autopilot.apiary.io/#phpclientlibrary)

The Autopilot API uses HTTP as a protocol and is fully detailed below. The client
libraries are provided for speed and convenience and each fully implements the API 
so you can just include the library and get working on your app immediately without worrying about
the nuances of our API.

# Group Autopilot App Basics

### What is an Autopilot App

Autopilot apps provide a way to integrate with an automate external APIs and system. You can make an app
to automate processes within your business using your internal API, or you can provide an app to be
shared with others to promote the use of your service.

An App in Autopilot consists of one or more "ingredients". Ingredients are actions, triggers and conditions.
A trigger is some sort of event which happens in your app which would initiate an automation flow, an action
is something which your app does for the current batch of contacts in an automation flow, and a condition is
something which checks the status of some value for the current batch of contacts in an automation flow.

You can think of an app as being a set of related functions that your external system provides. You can
create as many apps as you want, but grouping similar ingredients make sense.

For example, the Twilio app provides the following ingredients:

* Send SMS (Action)
* Send MMS (Action)
* Make Call (Action)
* Receive Call (Trigger)
* Receive SMS (Trigger)

The minimum requirement for an app is to have one ingredient. 

### Autopilot Basics

At the core of Autopilot is the Autopilot canvas. This allows contacts to flow through a series of objects, 
each of which takes some sort of action with the data it is presented. For example, a flow might trigger 
when a form is submitted, send the contact a welcome email, wait 3 days, see if the user has opened the
email, send a reminder if they haven't and make them a lead in Salesforce if they have.

There is an infinite (well not really, but there's many) combination of objects that can be made, and part
of adding to the Autopilot API consists of adding new triggers, actions and conditions.

![alt text](http://cl.ly/image/1Z2L3b0v0i3U/autopilot1.png "Autopilot Canvas")

There are 3 kinds of object which are allowed on the Automation Canvas: triggers, actions and conditions. 
Additionally, outcome paths decide what happens after an object has completed its work.

### Terminology

| Terminology             | Description   | Example  
| :-------------    |:------------- | :-----
| `App`             | A group of ingredients | Apiary App
| `Ingredient`      | A trigger, action or condition | Send an SMS action, Received SMS Trigger
| `Trigger`         | An event which initiates a flow | Contact added to list
| `Action`          | An action which takes place when reached in the flow | Send Email
| `Condition`       | A condition which is checked and multiple outcomes are available | Form field status
| `Outcome path`    | the relationship and sequence events between the Triggers, Actions, and Conditions that comprise an Autopilot campaign. 
| `Flow`            | A series of connected triggers, actions and conditions created by an Autopilot user
| `Recipe`          | A pre-defined flow created to show marketers best practices

### App availability and sharing

By default, any app you create will only be available on the Autopilot instance that you create it on.
An Autopilot instance is simply your company's separate system and database on the Autopilot system.
This means that when you first create an app, it is available only for internal use for your company
on the instance on which it was created.

If you have more than one Autopilot instance, you are able to share your app with those by specifying
the Autopilot instance ids in your app's settings. This way if you have a group of instances associated
with your account, you can share the functionality of your app with them.

Finally, it is possible to request for your app to be made public. Presently, there is no scope to charge
for your app but *that will change*. You soon be able to specify a monthly fee for your app which
clients who choose to install it will need to pay in order to keep using it.

# Group Creating an app in Autopilot

### Logging in

The first step in creating an app is done in the App Builder section of the Autopilot admin system, so
you will need to be able to login to your organisation's account in order to create the app.

If you haven't been provided a login yet, ask the person who set up your Autopilot account to add you
to the "My Team" section by following these instructions: <http://support.bislr.com/knowledgebase/articles/147461-my-team-adding-new-users-to-your-account>

### Adding an App

#### Step 1 - Open App settings

Click on the *Settings* icon and then select *API*

#### Step 2 - App store info

Give your app a `Title`, `Author`, `Short Description` and `Long Description`. Don't worry too much
if you don't have all the details just yet, you can come back at any time. Everything is optional 
except for `Title`, although there are requirements on these fields when it comes to submitting 
your app to the store later.

#### Step 3 - Take note of your API key & secret

Each App has its own API key and secret. Make note of these now, as you will need them in
order to implement the backend portion of your ingredients.

Please see the [Authentication](http://docs.autopilot.apiary.io/#apiauthentication) section for more information on how to use these details.

#### Next step - Add your first ingredient

Add your first ingredient. The next section of this document explains what is involved in creating
an ingredient.

# Group Adding an ingredient to your app

### Adding an Ingredient to your App

#### Step 1 - Open App

Click on the app that you wish you add an ingredient to:

**screenshot**

#### Step 2 - Add a new Ingredient

Click the *Add new ingredient* button

#### Step 3 - Select Trigger, Action or Condition

Select what type of ingredient you wish to create.

You should use a `trigger` when:

* You want an event to result the start of a flow.
* You want to integrate events from external systems.

**Example:** user registers on your system.

You should use an `action` when:

* You want your system to perform a task or function in the course of an Autopilot flow.
* You want to receive data from the Autopilot system.
* You want to perform an action and take different paths based on the outcome.

**Example:** sending an email.

You should use a `condition` when:

* You want to give the user the option to take different actions based on pre-existing data
* You want the user to be able to take different actions based on events *which have previously occurred*.

**Example:** checking whether a user is currently up to date with payments

#### Step 4 - Configure your Ingredient

No matter what type of ingredient you are creating, you need to nominate some basic details:

`name` - The name of the ingredient, as it will appear to users, e.g. "Send SMS".

`identifier` - A short string uniquely identifying this ingredient. This is most significant for triggers for which it is used to identify which trigger to activate when calling it using the API.

`shortDescription` - A brief description of what this ingredient does, e.g. "Sends an SMS".

`longDescription` - An optional longer description for mouseovers and detail in the store, e.g. "This ingredient allows you to send SMS messages to contacts in the current flow using the Twilio API".

`endpoint` - Relevant only to actions and conditions, this should be the full URL to your REST API which implements an action or condition as described in this document. If you haven't made it yet, or someone else will be making it for you, leave this blank for now, you can enter it later and the system will remind you to do so.

### Ingredient designer

Our visual ingredient designer allows you to select the colors and icon for your ingredient.
This gives you the opportunity to brand your ingredient so that it will be easily identifiable
to potential customers if you include it in the store.

In any case, make sure that your icon does the best to convey what the ingredient does. When
an ingredient is on the canvas, not much text will be able to be displayed, so having an icon
which reminds the user what the ingredient does will help.

### Outcome wheel

The outcome wheel is available for actions and conditions only. Triggers strictly initiate a
flow and cannot have multiple outcomes. 

For an `action`, the outcome wheel contains the different outcomes which can come from the 
action which your system is performing. For example, this might be a "success" or "failed"
outcome.

For a `condition`, the outcome wheel contains the available alternatives for the status which
the object is checking. For example, "currentCustomer", "overdueCustomer", "previousCustomer".

Your outcome wheels can have up to 6 outcomes.

The name of each outcome must be an alphanumeric string. This is what you will use in the 
response to the API call which will be made to your backend. You will return a javascript
object containing keys representing each of the outcomes and including arrays of the list
of member ids which meet each criteria.

Before your ingredient will go live, our system will need to test your backend code to make
sure it provides results for each of the outcomes (even if they are empty arrays).

**Note**: *For all actions and conditions, a default `onContinue` outcome is always included. All
contacts will pass through the `onContinue` if it is used by the user by default, your app
will not be responsible for, or able to change this.*

### Settings panel creation

There are potentially several details that your ingredients are going to need in order to 
perform their function for the client. For example, you might require an API key for your 
app to identify the customer, some settings which configure the app to suit them, or identifying
a specific piece of data to act on in your back end.

For that reason, each ingregient contains a settings panel which can ask for a series of input from
the user before the ingredient is considered *configured*. If an ingredient in an Autopilot
flow is not configured, the user will not be able to publish the flow. So you can be sure
that if your ingredient has compulsory data, that it will be provided before your backend
is hit.

# Group Testing your ingredients in flows

### Testing an ingredient

The easiest way to test ingredients for your apps is by simply using them on an Automation flow on
the instance on which you created them. This involves:

1) Create a new Automation flow


2) Find the ingredient which you want to test in the right hand panel


3) Create a basic flow using a test list of contacts

A good idea is to maintain a list of "test" contacts in your Autopilot instance. This might include
you and various incarnations of your email address. If you use Google app you can do things like
chris+test1@autopilothq.com, chris+test2@autopilothq.com, etc.

You can then set up a flow like the following to test your ingredient:

Trigger Timer -> Select list of contacts "Test" -> Your ingredient -> Add to list "Test Complete"

### Test instance

If you are making your app on your company's Autopilot instance and don't feel comfortable testing on there,
you may request a test instance from us which we can set up for the purpose of app creation. Just send
us an email at support@autopilothq.com with the subject "Test instance" and we will get one going for you.

In this case, once you have finished testing your app, you can licence it to your main Autopilot instance, 
which will then be able to install the app from the store.

# Group Sharing your app with other instances

Any apps you create in your Autopilot instance are by default only available to that instance.
If you have other Autopilot instances that you would like to share the app with, you can do
so by adding them in the *permissions* section of the app settings.

**settings screenshot**

You may enter any other instance id to share the app with that instance. Users of that instance
will then have the opportunity to install your app.

**Note:** *Once you share the app with another instance and they install the app, you will
not be able to revoke access to the app. The reason for this is that it may break flows
that the other instance that contains ingredients from your app.*


# Group Making your app public

If you would like to share or sell your app, the first step is to apply to make the app
public. To do this select the *Make My App Public* option in the app settings screen.


# Group API: Authentication

### Note on Apiary mocking server

Please feel free to take advantage of Apiary's built-in examples and mocking server. Please note though that when 
using the live API, that you will need to provide the `Authorization` header as described below.

### Note on Client Libraries

If you choose to use one of the available client libraries, then authentication is handled for you. The
client libraries also provide good examples of how to implement this scheme.

### Authentication Method

The Autopilot signing and authentication of REST API requests is based heavily on the AWS Signature Version 2 
<http://docs.aws.amazon.com/AmazonS3/latest/dev/RESTAuthentication.html>.

Authentication is the process of proving who you are to the system. The Autopilot API uses a custom HTTP 
scheme based on a keyed-HMAC (Hash Message Authentication Code) from authentication. To authenticate a request, 
you concatenate select elements of the request for form a string. You then use your Autopilot secret key to 
calculate the HMAC of that string. You then add this signature as a parameter of the request as a HTTP header.

When Autopilot receives your request, it checks the secret key and runs the same calculation on your request 
to see if the signatures match. If it does, your request it fulfilled. 

### The Authentication Header

The Autopilot API uses the standard HTTP `Authorization` header to pass authentication information.

```
Authorization: Basic AutopilotKey:Signature
```

Your Autopilot API key and secret are available in your Autopilot Admin. You can regenerate these at any time, 
and there is only one key and secret per Autopilot instance.

The `AutopilotKey` will be the same in every request, wheras the `Signature` should be different per request.

The following is an example of how to calculate the HMAC `Signature` to sign your requests:

```
StringToSign = HTTPVerb + "\n" +
               Content-Type + "\n" +
               Date + "\n" +
               Resource

Signature = crypto.createHmac('sha256', AutopilotSecret).update(data).digest('base64')

Authorization = "Basic:" + " " + AutopilotKey + ":" + Signature;
```

### Explanations of the above variables

| Field             | Description   | Example  
| :-------------    |:------------- | :-----
| `HTTPVerb`        | The type of HTTP request being made | POST
| `Content-MD5`     | An MD5 hash calculation of the body of your request | 9e107d9d372bb6826bd81d3542a419d6 
| `Content-Type`    | The content type being requested in the headers | application/json 
| `Date`            | Date in RFC 822 format | Sun, 06 Nov 2014 08:49:37 GMT
| `Resource`        | The path part of the URL without domain or query string | /automation/trigger
| `AutopilotSecret` | The Autopilot Secret as provided to you in your Autopilot Admin | 13098c67fe342dd9d92bbc201c3c7484
| `AutopilotKey`    | The Autopilot Key as provided to you in your Autopilot Admin | 2c55ff5479cdc15853883a488d6ea712

Using the above example, we get the following

### Full example using example values

```
StringToSign = "GET" + "\n" +
               "application/json" + "\n" +
               "Sun, 06 Nov 2014 08:49:37 GMT" + "\n" +
               "/automation/trigger"

Signature = crypto.createHmac('sha256', "13098c67fe342dd9d92bbc201c3c7484").update(data).digest('base64')

Authorization = "Basic:" + " " + "2c55ff5479cdc15853883a488d6ea712" + ":" + Signature;
```

Select the following link for examples in various languages:

<http://api.autopilothq.com/docs/auth-examples>

### Available libraries

We also offer a standard Node.JS NPM module which fully implements this API. There are more examples in 
this document on how to use the library, but regarding authentication, this is how you do it:

```javascript
var Autopilot = require('autopilot');
autopilot.setAutopilotKey('2c55ff5479cdc15853883a488d6ea712');
autopilot.setAutopilotSecret('13098c67fe342dd9d92bbc201c3c7484');
autopilot.getAutomationFlows(function(err, autopilotFlows) {
  if (err) {
    console.log("Could not get Autopilot flows", err);
  }
  else {
    console.log("Found these Autopilot flows", JSON.stringify(autopilotFlows));
  }
});
```

Find out more about our various pre-build libraries here:

<http://api.autopilothq.com/docs/libraries>

# Group API: Command Stream


# Command Stream [/v1/commandStream]

## Subscribe to command stream [GET]
The command is a one-way stream which you connect to, so that you app can receive all commands from
the Autopilot system.

The command stream is used for the following things:

* Receiving heartbeats to confirm that your app is online and responding correctly.
* Receiving commands to run actions and check conditions which you must respond to using [finishTask](http://docs.autopilot.apiary.io/#apiactionandconditions).
* Receiving logs

For your app to work with Autopilot, it must be connected to the Autopilot stream at all times. If 
you use a client library (outlined later in this document), maintaining a connection to the stream
will be handled for you. If you are implementing your app with the REST API as described here, it is
your responsibility to keep the stream connected.

You only connect one command stream for your entire app which covers all ingredients and all autopilot
instances. Where relevant, the data will be provided to tell you which autopilot instance to act on.
In general terms you will only be provided with autopilot instances to work with which have both
permission to use your app (i.e. you gave them a licence or your app is public) and have also installed
your app. The Autopilot system takes care of this for you so you don't have to worry about it.

To connect to the command stream, you simply send a GET request to **/v1/commandStream**. Essentially,
this connection will then never be terminated, the response will be streamed to you as commands come
in. A regular HEARTBEAT will also be sent through the stream to both make sure the connection stays
alive and also to check that your app is responding correctly. There is a section below on how to
handle heartbeats.

Note that although there is no payload to this request, you do require the Authorization header as
described above, which will contain your AutopilotKey, thus identifying the app stream which you
are connecting to.

### Parsing the command stream

The command stream is the response to your GET request and the system will keep writing to this stream
until it is closed. If you restart your app and the stream is lost, the Autopilot system will buffer
any commands that it was going to send to your app until it is back online and then start sending them
when the connection is re-established.

Individual commands will be separated by a newline character. Although commands will tend to be written
all at once, it is not sufficient to consider each chunk of data recieved to be a single line, instead
a better way of handling this is to buffer the data you are receiving until you detect a newline character
and then parse the command prior to that.

A Javascript example:

```
var buffer = "";

function processCommand (command) {
}

function dataReceivedFunction (data) {
  buffer += data.toString();
  while (buffer.split("\n").length > 1) {
    var parts = buffer.split("\n")
    processCommand(parts[0]);
    buffer = parts[1];
  }
}
```

### Heartbeats

Heartbeats are sent on a regular basis by Autopilot to both keep the connection alive, and to check that
the connection is still able to receive data. Heartbeat commands consist of the capitalized word
HEARTBEAT, followed by a colon and then the current Unix timestamp.

For example:

```
{"cmd": "HEARTBEAT", "at": "1407804024"}
```

It is safe to ignore heartbeat messages.


### Log lines

Log lines are also sent through the command stream so that you can see any errors that your app is having, 
or get other general information about the system. Log lines will be in the format `LOG:TYPE:Log message`

So some examples of log lines might be the following:

```
{"cmd": "LOG", "at": "1407804025", "type": "ERROR", "content": "You do not have permission to write to the 'demo' instance."}
{"cmd": "LOG", "at": "1407804025", "type": "INFO", "content": "Contact member_123948723497 added successfully."}
{"cmd": "LOG", "at": "1407804025", "type": "WARN", "content": "Two out of maximum of 3 command streams connected."}
```

### Actions and conditions

The stuff we are really interested in as an Autopilot app developer are the `action` and `condition` commands
which tell our app what to do and provide the details of the contacts who we need to do it for. `Triggers`
are different because it is our app which initiates the action, so these are discussed in the "API: Triggers"
section of this document.

Here is an example action:

```
{
   "cmd": "ACTION", 
   "at": "1407804025",
   "taskId": "text_wfwiruh239r82u3r",
   "identifier": "sendSMS"
   "autopilotId": "demo",
   "settings": {
        "smsText": "Hi --first_name--, I'm sending you an SMS"
   },
   "contacts": [
        {
          "_id": "contacts_person_76BA0335-6BE2-4ECE-835E-4556FA5B5F6F",
          "_rev": "3-d6cc9c1b7cd338bd288fc61086a0fb48",
          "FirstName": "oiooio",
          "LastName": "ioioio",
          "Company": "bislr",
          "twitter": "",
          "created_at": "Mon, 11 Aug 2014 23:06:47 GMT",
          "Phone": "555-555-555",
          "MobilePhone": "555-555-555",
          "Title": "io",
          "Fax": "555-555-555",
          "MailingStreet": "califronia",
          "MailingCity": "san francisco",
          "MailingState": "ca",
          "MailingPostalCode": "93020",
          "MailingCountry": "USA",
          "Birthdate": "01//01/01",
          "Industry": "comps",
          "NumberOfEmployees": "100",
          "company_priority": false,
          "Name": "oiooio ioioio",
          "app": "contacts",
          "instance_id": "person_76BA0335-6BE2-4ECE-835E-4556FA5B5F6F",
          "Website": ""
        }
   ]
 }
```

*Note: I have added the newlines for convenience of viewing, the only new lines in the real command steam
are to deliniate commands.*

Whenever your app receives an ACTION or CONDITION command, it is the apps' responsibility to execute the
requested action and then respond with a [finishTask](http://docs.autopilot.apiary.io/#apiactionandconditions) 
call, which is described in the next section.

There are several, important values which will always be present in an ACTION or CONDITION command:


`cmd` - In this case we are looking for commands which have the literal value "ACTION" or "CONDITION".

`at` - This is a UNIX timestamp of the time at which the command was sent.

`taskId` - This is a unique identifier for the task we are currently executing, we will need to it identify the task we are responding to when calling [finishTask](http://docs.autopilot.apiary.io/#apiactionandconditions).

`ingredientIdentifier` - This is the identifier that you set when creating your ingredient in the Autopilot admin, it might be something like `sendSMS`

`autopilotId` - This is the current Autopilot instance that we are acting on behalf of for this call. Autopilot will enforce permissions in terms of licenced/installed apps, so if you get a request for a given identifier, you can be fairly sure you have permission to act on it.

`settings` - This will be a hash containing the settings as configured by the person using your ingredients in their automation flow.

`contacts` - This will be an array containing the full contact record for each of the contacts which your action or condition should act on for this request. The reason the full contact details are provided is to avoid forcing you to do make additional requests just to look up the contact details for the contacts which you definitely have to act on anyway.


+ Request (application/json)

        {
        }

+ Response 500 (application/json)

        {"error": "error", "message": "(various)"}

+ Response 403 (application/json)

        {"error": "unauthorized", "message": "You are not authorized to connect to the commandStream using this api key."}



# Group API: Action and Conditions

# Responding to actions and conditions [/v1/finishTask]

## Finish Task [POST]

When you have received a command from the command stream to execute an action or check a condition
it is your responsiblity to reply to Autopilot within 5 minutes. Ideally you will reply much more
quickly than this depending on what action you are performing.

To reply, you need to post to the command stream. At a minimum your POST request needs to contain
the following data:

`taskId`: this should be the taskId which you received in the original command

`autopilotId`: this is the autopilotId which you were sent in the original command. This will be checked
to see if it matches before the flow is continued, so it is important that it is correct.

`results`: this is an optional hash which contains values for the different outcomes defined for this
action or condition. If you do not provide it, all contacts will flow into the `onContinue` outcome
only. In the case of a condition, this will result in all contacts getting "stuck" on your ingredient,
so it is important that you provide the correct results object.

To do this you need to split contacts into different arrays which correspond with the names of
each of the outcomes that you defined in the outcome builder. Below we see the example of `onSent`
and `onNotSent`. While contacts can appear in multiple outcomes, it is not recommended.

+ Request (application/json)

        {
            "taskId": "task_wiweufhwiefhwiefuhwf",
            "autopilotId": "demo",
            "results": {
                "onSend": ["member_xxxxx", "member_yyyyy"],
                "onNotSent": ["member_zzzzz"]
            }
        }

+ Response 200 (application/json)

        {"credits": 438}

+ Response 500 (application/json)

        {"error": "error", "message": "(various)"}

+ Response 404 (application/json)

        {"error": "error", "message": "Task not found for this autopilotId"}

+ Response 403 (application/json)

        {"error": "unauthorized", "message": "Your request was not authorized."}
        
        
# Group API: Triggers

# Trigger [/v1/trigger]

## Call Trigger [POST]

+ Response 403 (application/json)

        {"error": "unauthorized", "message": "Your request was not authorized."}

# Group Node.js Client Library

Here at Autopilot, we develop almost exclusively using Javascript and Node.js. For that reason we have
created a full implementation of the Autopilot API as a Node.js NPM module for your convenience. All you
need to do create a new app, or add Autopilot support to your existing app is to include our library,
set up an app and optionally incredients in your Autopilot Admin, and get your API key and Secret.

The library will take care of authentication and signing all of your requests, so you can completely
disregard the sections in this document on auth, http, etc. and just focus on what makes your app
unique.

From there, you define your app's behaviour like a normal Node.js app, with a few special callbacks
and helpers we provide. Below, I'll walk you through a sample app, which should be quick & easy to 
understand.

## Breakdown of an example Node.js Autopilot app

[View the full example app on Github](https://github.com/autopilothq/autopilot-api)

The initial setup requires you to include the npm module 'autopilot-api'

To install this you'll need to run: `npm install autopilot-api --save`

Following, we setup our initial file with API and Secret credentials:

```
var autopilot = require('autopilot-api');
autopilot.setApiKey('3fzp8pvi');
autopilot.setSecret('0h9i19k9');
```

Once this is setup we can start making direct calls to the Autopilot API for Autopilot instances 
which have our app installed. You are automatically authorized to work on your own Autopilot instance,
that is, the one which you set up the app on to get your API key and secret. In this example let's
consider that to be called `demo`. Your app, of course, will be able to operate on other instances
later for anyone who installs your app within your company if you share it, or anyone if you make 
your app public and people install it.

The idea is to make your app capable of working with any Autopilot instance, and let the Autopilot
system and its app management tools which you control to decide which instances your code should
operate on. For a more detailed understanding of app sharings and permissions, **select this link**.

### Actions & Conditions

The next step for our app is to write definitions for any `actions` or `conditions` which we created
in the ingredient builder. Actions and conditions are triggered when a contact reaches them in a
published Autopilot flow for a given instance.

For example, a person might have a flow which looks like:

```Trigger Timer -> Select Contacts from List -> Send SMS -> Add to List 'All Sent'```

Where `Send SMS` is your ingredient. You have set up your `Send SMS` ingredient in the ingredient
builder like so:

If you haven't done this and need help, please see the **App Builder** section of this document.

To define the behaviour for `actions` and `conditions` we need to do similar to the following.
The difference with a condition is that we would normally be checking something in our database
and definitely having multiple outcomes. Below represents an action in the sense that it does work
(i.e. sends the SMS) and both an action and condition in the sense that it has multiple outcomes
which split the contacts coming into it:

```
var async = require('async');
var autopilotTwilio = require('autopilot-twilio');
var autopilot = require('autopilot-api');
autopilot.setApiKey('3fzp8pvi');
autopilot.setSecret('0h9i19k9');

autopilot.action('sendSMS', function (contacts, settings, callback) {
  var results = {onSent: [], onNotSent: []};
 
  async.forEach(contacts, function(contact, cb) {
    autopilotTwilio.sendSMS(contact.cell, settings.smsTxt, function(err, status) {
      if (err) return cb(err);
      if (status === 0) {
        results.onSent.push(contact.id);
      }
      else {
        results.onNotSent.push(contact.id);
      }
    );
  }, function(err){
    if (err) {
      callback("An error has occurred: possibly out of credits");
    }
    else {
      callback(null, results);
    }
  });
});

```

As seen above, we define our behaviour by calling the `action` or `condition`  methods, passing in
the name of the action or condition as we set it up in the ingredient builder (don't worry you'll 
see an error printed out when you run your program if you get it wrong) along with a function which
takes three parameters.

`contacts` - This parameter is an array of the contacts which have reached this action or condition.
You can count on this variable *always* being a Javascript array, so there is no need to do any 
type checking. If the step is invoked but there is no contacts, the array will simply be empty. The
contacts will contain the full basic information for the contact including custom fields. We do this
because usually ingredients will need to act on or use a certain portion of this contact's data, so
to save additional calls and overhead (and to save you work!) we just send you through the latest
info right along with the request.

`settings` - These are the settings that the Autopilot user has inputted into your settings panel(s)
for your app, as you defined them. So if your panel looked like this:

**screenshot**

Your settings object would look like the following:

```
{
  smsText: "Hi --first_name--, thought I'd send you a delightful SMS",
}
```

`callback` - This callback method is designed to allow the Autopilot flow to continue for the current
batch of contacts after your object is finished doing its work. The system is very patient and will
give you sufficient time to complete your task, but you should make sure your task finishes within
*5 minutes*. If it doesn't, the task will be considered a failure an contacts will get 'stuck' on your
step.

Callback expects very specific data to be given to it. In the case where there was an error executing your task for ALL CONTACTS, you can callback with a generl
     error to say that the task has failed:

```
callback("An error has occurred: possibly out of credits");
```

If your ingredient is an ACTION and does not have any specific outcomes on the outcome wheel
then all contacts will simply travel down the "onContinue" path and you don't have to do anything
except for the following. This is not allowed for conditions which must have different outcomes.

```
callback(null);
```

For actions with multiple outcomes, and all CONDITIONS, you must provide a Javascript object
as the second parameter to callback which contains arrays of member ids which need to travel
down that outcome path. No matter what you do, they will all travel down the "onContinue" path
too, but this allows you to split the flow of members into different outcomes.

```
callback(null, {"onSent": ["member_xxxx", "member_yyyy"], "onNotSent": ["member_zzzz"]});
```

### The general listener and program flow

Once you have defined your action and condition handlers you need to ensure two things. Firstly,
that your program continues running. If it isn't running then it won't be available to handle
incoming action and condition requests from Autopilot. Secondly, it needs to be listening to 
the Autopilot command stream to get the incoming commands and data which are instructing it what
to do.

Fortunately we have made this especially simple for Node.js developers and all you need to do 
is include the following, preferably at the end of your action and condition definitions:

```
autopilot.listen()
```

This one little command will take care of a lot for you:

* Subscribing to the Autopilot command stream for your app
* Re-connecting the stream if it gets disconnected
* Killing off other connections for your API key if they exist
* Making sure your program keeps running
* Answering HEARTBEATs received from the Autopilot command stream so your program continues to register as being online.

Without this line being included in your program, nothing will happen.

### Triggers

The next type of ingredients you will need to handle in your app are triggers. Triggers are events
which happen in your system which the Autopilot user wants to use to trigger their Autopilot flows.

You can define triggers for absolutely anything, but triggers have one major requirement: they must 
act on an existing contact in the Autopilot instance. If a contact doesn't exist and you want to 
trigger based on them, then you have to create the contact first. An example might be the easiest way
to explain this.

Let's say you were making a trigger ingredient which triggered a flow whenever someone submitted
a survey on SurveyMonkey. Your code might look something like this:

```
var autopilotSurveyMonkey = require('autopilot-surveymonkey');
var autopilot = require('autopilot-api');
autopilot.setApiKey('3fzp8pvi');
autopilot.setSecret('0h9i19k9');

/* This is just an example of an event coming from your own code or from
   a library which you want to implement as an Autopilot app. If the app 
   you are using isn't event driven, you can either poll for changes and 
   then call the Autopilot trigger when you get new data, or implement
   the Node.js EventEmitter yourself */
autopilotSurveyMonkey.on('surveySubmitted', function(surveyDetails) {

  autopilot.findOrCreateContact(
    surveyDetails.email,
    {
       firstName: surveyDetails.firstName,
       lastName: surveyDetails.lastName,
       email: surveyDetails.email
    },
    function (member_id, member) {
      autopilot.trigger('surveySubmitted', [member_id], function (settings, callback) {
        

      });
    }
  );
});

```

**NOTE TO SELF: HOW TO TRIGGER BASED ON SETTINGS**

As you can see, the trigger is defined in a similar way to `actions` and `conditions`. You give
two parameters to the trigger method: the name of the trigger as defined in the Autopilot app 
builder, and a handler function. The main difference being that the handler function only has two 
parameters: the settings that the user inputs based on the settings pangel you created in the Autopilot 
App Builder, and a callback function.

`settings` - These are the settings that the Autopilot user has inputted into your settings panel(s)
for your app, as you defined them. So if your panel looked like this:

**screenshot**

Your settings object would look like the following:

```
{
  smsText: "Hi --first_name--, thought I'd send you a delightful SMS",
}
```

`callback` - This callback method is designed to allow the Autopilot flow to start once your trigger
has finished executing. Often triggers have very little work to do other than the act of triggering,
but it is at this point you will need to identify the contact or contacts that we are triggering for.
See below for how to accomplish this.

Since triggering is a deliberate action, it never expects you to send through an error. If there 
is an error in preparing to trigger for something, this is the issue of your application and you
should handle it yourself. If you do call the callback with an error, it will just be ignored:

```
callback("Don't send errors on triggers, they will just be ignored");
```

It is also not acceptable for a trigger to callback with no contacts. If there are no contacts,
then there is nothing to trigger, so why bother doing it? The following will also be ignored:

```
callback(null);
callback(null,[]);
```

The only acceptable callback format for a trigger is the following, calling back with null as
the error and an array of member ids as the second parameter:

```
callback(null, ["member_xxxx", "member_yyyy"]);
```

The reason we still accept the error parameter even though it is ignored, is because this is a 
convention across the whole Autopilot Node.js library and it can lead to confusion if the function
signatures change between different calls, a la PHP.

The two main issues in terms of triggers that need to be addressed are the following.

#### If I can only provide member_ids to the trigger, how do I get them?

Good question. The best way to do this is use the `addContact` method to find or create the contact(s)
that you wish to work with. `addContact` uses `email` as the primary key, so if it finds an existing
contact in the current Autopilot instance, it will return it for you, and if none exists then it 
will create it for you. There are 3 merge strategy when it comes to updating existing contacts:

1) `merge` - This is the default, any new fields you provide that do not exist on the contact will be
added, and the new contact data will be returned. Fields which the contact has already will be ignored.

2) `overwrite` - All fields you provide will be overwritten, fields you don't provide will be kept.

3) `skip` - If this contact already exists, don't do anything to its data, just return it.

Here is an example call to `


### Error logging and the universal log

One of the biggest frustrations of building any new app is not getting good feedback on where you 
are going wrong if things aren't working. Or, if things are working, what is happening at any 
given time. For this reason we provide two methods of feedback for you to track your app.

#### App Monitoring

Please see the section on **App monitoring**, essentially in the App Builder in Autopilot you can
see the number of calls your app is making, uptime/downtime, errors and a general log.

#### In-app logging

**Watching the log**

```
autopilot.log.tail('all')
autopilot.log.tail('all', {autopilotId: 'demo'});
```

This will use console.log to output log lines as they come in from the command stream. In the 
second example we restrict it to log lines for the Autopilot instance called `demo`. You can
also define you own custom handler for each line which comes through so you can divert the output
elsewhere or otherwise parse it. In this example the options object is blank:

```
autopilot.log.tail('all', {}, function(line) { console.log(line); });
```

**Watching just the errors**

```
autopilot.log.tail('errors');
```

As above you can pass a handler function as a second parameter to handle the lines yourself.

**Getting the whole log**

```
autopilot.log.cat('all', {autopilotId: 'demo'}, function(line) { console.log(line); };
```

Note be careful with this call as it is going to stream your entire historical log. You can either 
specify 'all', 'error', or 'info' to filter the log. The `autopilotId` in the options object is
optional.

**Adding to the log**

```
autopilot.log.info('demo', 'Instance is out of credits');
autopilot.log.warn('demo', 'Low on credits', {credits: 5});
autopilot.log.error(null, 'System outage');
```

The Autopilot log methods accept an optional autopilotId as their first parameter and a log line
as their second parameter and an optional javscript object as the third parameter which can store
meta data along with the log.

You should include an autopilotId wherever possible, that way it will be associated with that instance
and the user will have the benefit of seeing any errors or meta data in their log. It will also
make debugging your app easier both for you and the Autopilot team.

# Group PHP Client Library

PHP is one of the most commonly used and accessible web programming languages. For that reason we have
created a full implementation of the Autopilot API as a PHP library for your convenience. All you
need to do create a new app, or add Autopilot support to your existing app is to include our library,
set up an app and optionally incredients in your Autopilot Admin, and get your API key and Secret.

        


