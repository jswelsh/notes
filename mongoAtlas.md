#MongoDB Atlas

####Triggers 
triggers allow you to execute server-side logic ini response to database sevents or according to a schedule. Atlas provides two kinds of *Triggers* : **Database** and **Scheduled** triggers

#####Database Triggers
allow you to execute server-side logic whenever a document is added, updated, or removed in a linked cluster.

database triggers are used to implement complex data reactions such as updating data in one document when a related document changes or interacting with an external service when a new document is inserted.

Database triggers use MongoDB change streams to listen for changes in watched collections and map them to database events.

#####Scheduled Triggers
 allow you to execute server-side logic on a regular schedule that you define using CRON expressions. Use scheduled triggers to do work that happens on a periodic basis, such as updating a document every minute, generating a nightly report, or sending an automated weekly email newsletter.


#####MongoDB Realm Functions
provide server-side logic. Triggers execute a MongoDB Realm Function that you specify. Each trigger is associated with exactly one MongoDB Realm Function.