Connected Documents
===================

Connected Documents are a way to combine two concepts:
- documents are applications are documents;
- connected objects interact with documents.

Documents are Applications, Applications are Documents
------------------------------------------------------

In the world of CouchDB, documents might be described as metadata (the JSON content of a CouchDB document) and attachments (any type of files, attached to a CouchDB document). However documents might also be viewed as applications; this is obvious for example in the case of CouchApps. In a sense, CouchApps treat an application as a document -- the goal is to build an application, and it is inserted in a CouchDB document. At the opposite end of the spectrum, a dynamic web page (HTML file with JavaScript, images, etc. dependencies) which interprets metadata and content-related attachments can easily be inserted in a CouchDB document; together with the metadata and content-related attachments, it becomes a self-running document (when the main HTML file is interpreted e.g. by a web browser).

Connected Documents recognizes that documents are applications, and conversely. The only underlying assumption is that if a database is required, CouchDB is assumed.

It is expected that a document-cum-application is not a static entity, but is able to modify itself; a simple example is an e-book, whose content is stored as an attachment to the CouchDB document, while the main HTML attachment (the application part) is an e-book reader, and metadata is used for example to store the user's progress through the book. More advanced uses might modify the content, or the application itself.

Since the application is embedded with the content, Connected Documents are natively offline-first, and can be shared amongst users without the need for anything besides CouchDB and a web browser.

Connected objects interact with documents
-----------------------------------------

Inside Connected Documents, physical objects are seen as interacting with their physical environment (as any physical object does), and with documents. This means that physical objects can provide the document-cum-application data about their environment (which is the main meaning of "connected objects" nowadays), can also be commanded by the document to interact with their environment (using various types of actuators connected to the object), but extends to also indicate that objects are able to gather data and react to it on their own, and are able to modify the document when they are connected with it.

Practically this means that objects need to be able to run small applications even if disconnected from the document, and that they must be allowed to push changes back into the document.

Architecture
============

Given those goals, Connected Documents consists of four major components:
- a web platform and services, independent of a particular application domain (be it toys, learning tools, etc.) used to create, deliver, and if needed generate income from, connected documents;
- the web platform works together with a user-device application, be it mobile or PC, to deliver connected documents to that device;
- a hardware electronic platform and its associated firmwares, common to all application domains but customizable in hardware (at production time) and in software (in realtime), which is interfaced with the user-device application;
- and finally, one or more custom documents-cum-applications built for the specific application domain, consisting of documents (which might be called lessons, exercises, games, etc. depending on the domain) which interact with one or more hardware components via the application installed on the user-device.

One document-cum-application provided by the project is a catalog of Connected Documents available on the user-device and its associated web platform.
