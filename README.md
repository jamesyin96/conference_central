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
### Design
#### Session: 
It's a class containing basic information about a session. Properties include:
- name: Each session must have a name(title), so we should use ndb.StringProperty(required=True)
- highlights: It can be keywords, descriptor or abstraction, so use ndb.StringProperty()
- speaker: we also want to know the name of the speaker, but so far we don't need to know more about a speaker, so use ndb.StringProperty()
- duration: It's how long the session last, so use ndb.IntegerProperty()
- typeOfSession: we have different kinds of sessions, but if the user does not specify which type, we can let it be "Unknown", so use ndb.StringProperty(default='Unknown')
- date: Similar to conference date, we need to have a date for the session, so use ndb.DateProperty()
- startTime: To inform people to attend the session at the right time, we also need to have a property indicating the start time of the session, so use ndb.TimeProperty()

#### Speaker:
It's a class containing information about a speaker. This class is not used for now. But it can extend the session information about the speaker if people want to know more about the speaker. This class can also be used for implementing a speaker related endpoint. Peoperties are:

- name: ndb.StringProperty(required=True)
- age: ndb.IntegerProperty()
- industry: It represents which industry for field this speaker comes from, so use ndb.StringProperty()

### Endpoint APIs
- getConferenceSessions(websafeConferenceKey) -- Given a conference, return all sessions
- getConferenceSessionsByType(websafeConferenceKey, typeOfSession) Given a conference, return all sessions of a specified type (eg lecture, keynote, workshop)
- getSessionsBySpeaker(speaker) -- Given a speaker, return all sessions given by this particular speaker, across all conferences
- createSession(SessionForm, websafeConferenceKey) -- open only to the organizer of the conference


## Task 2: Add Sessions to User Wishlist
### Design
Since each session does not particularly belong to a user, we don't want each user to have an entity copy for each session in his/her wishlist. Instead, we just need to save a list of keys that can represent sessions in the wishlist.

To do this, we add a sessionWishList property into Profile kind. This property is string and repeated, which takes less space than storing the whole session infomation. When we want to retrieve session information, we just need to get the key first and then fetch information using the key.

We don't want to force people to register for the meeting in order to add sessions to wishlist, so everyone can add sessions into their wishlist.

### Endpoint APIs
- addSessionToWishlist(SessionKey) -- adds the session to the user's list of sessions they are interested in attending
- getSessionsInWishlist() -- query for all the sessions in a conference that the user is interested in
- deleteSessionInWishlist(SessionKey) -- removes the session from the user’s list of sessions they are interested in attending

## Task 3: Work on indexes and queries
### Additional queries
- getOngoingConferences() -- get all conferences that are ongoing, this might be helpful for people to search for meeting they can attend for their current time.
- getSessionsByDateRange(startDate, endDate) -- get all sessions that are held for a given date range. This can help people plan their schedule more efficiently before they go to the meeting.

### Query problem
Let’s say that you don't like workshops and you don't like sessions after 7 pm. How would you handle a query for all non-workshop sessions before 7 pm? What is the problem for implementing this query? What ways to solve it did you think of?

This problem is a query that requires two inequality (not workspace and before 7 pm). Usually we can not do two inequality in one datastore query, but luckily, the kinds of sessions that are available is not unlimited so we can transform the inequality into equality. Notice that of all the operators in datastore query, there is a "IN" operator which can represent member of (equal to any of the values in a specified list), so we can make the allowed type of sessions in a list and query for session type IN allowed type list.

The endpoint API for this query is getPreferredSessions()

## Task 4: Featured speaker & Add a task
### Featured speaker query
The endpoint API implemented here is getFeaturedSpeaker(), what is does is that it will first query the Memcache for current featured speaker, if there's no featured speaker for now, it will try to find a featured speaker from all sessions.

### Scheduled task
For each one hour, the server will run the function that sets the featured speaker in Memcache. This can help reduce traffic and improve response because cache is faster and featured speaker does not need to be stored in the database.

[1]: https://console.developers.google.com/
[2]: https://localhost:8080/
[3]: https://developers.google.com/appengine/docs/python/endpoints/endpoints_tool