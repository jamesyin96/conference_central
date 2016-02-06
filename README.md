App Engine application project for the Udacity web developer course.

## Language
- Python

## Setup Instructions
1. Update the value of `application` in `app.yaml` to the app ID you
   have registered in the App Engine admin console and would like to use to host
   your instance of this sample.
1. Update the values at the top of `settings.py` to
   reflect the respective client IDs you have registered in the
   [Developer Console][1].
1. Update the value of CLIENT_ID in `static/js/app.js` to the Web client ID
1. (Optional) Mark the configuration files as unchanged as follows:
   `$ git update-index --assume-unchanged app.yaml settings.py static/js/app.js`
1. Run the app with the devserver using `dev_appserver.py DIR`, and ensure it's running by visiting your local server's address (by default [localhost:8080][2].)
1. (Optional) Generate your client library(ies) with [the endpoints tool][3].
1. Deploy your application.

## Task 1: Add Sessions to a Conference
- Session: 
It's a class containing basic information about a session. Properties include:
name: Each session must have a name(title), so we should use ndb.StringProperty(required=True)
highlights: It can be keywords, descriptor or abstraction, so use ndb.StringProperty()
speaker: we also want to know the name of the speaker, but so far we don't need to know more about a speaker, so use ndb.StringProperty()
duration: It's how long the session last, so use ndb.IntegerProperty()
typeOfSession: we have different kinds of sessions, but if the user does not specify which type, we can let it be "Unknown", so use ndb.StringProperty(default='Unknown')
date: Similar to conference date, we need to have a date for the session, so use ndb.DateProperty()
startTime: To inform people to attend the session at the right time, we also need to have a property indicating the start time of the session, so use ndb.TimeProperty()

- Speaker:
It's a class containing information about a speaker. This class is not used for now. But it can extend the session information about the speaker if people want to know more about the speaker. This class can also be used for implementing a speaker related endpoint.
Peoperties are:
name: ndb.StringProperty(required=True)
age: ndb.IntegerProperty()
industry: It represents which industry for field this speaker comes from, so use ndb.StringProperty()

## Task 2: Add Sessions to User Wishlist
- Wish list

## Task 3: Work on indexes and queries
-

## Task 4: Add a Task


[1]: https://console.developers.google.com/
[2]: https://localhost:8080/
[3]: https://developers.google.com/appengine/docs/python/endpoints/endpoints_tool