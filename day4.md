# Designing a Communication Strategy with Queues and Service Bus

Now we have all our microservice thingies, we need them to communicate with one another. We will have `worker` processes to receive and process signals, and `notify` processes to send signals.

## Azure Storage Queues

Look, this is just a queue to put an arbitrary blob into storage. You can mess with it and it's not fussy.

 * A FIFO queue available in storage accounts (stuff queuing up to be stored)
 * Store large quantities of small messages for a large (scalable) number of user consumers
 * A message isn't tied to one queue - if it fails another queue can pick it up

You can:

 * GetMessage - retrieve from the queue and hide it from other clients
 * PeekMessage - Peek at next message before it is handled, without hiding it
 * Dequeue next message - when a message is complete
 * Insert message (at arbitrary position in the queue!) useful for high priority messages
 * UpdateMessage - update existing message content(!) after it has been enqueued. Which seems a bit wibbly wobbly to me. It is probably preferable (architecturally) to add a change order onto the queue instead


## Service Bus

### Service Bus Queue

This is the fussy one for real software engineers. It guarantees FIFO and single delivery.

 > A managed message infrastructure - massive scale, completely managed. Allows decoupled components to communicate sync or async.

* Relayed Messaging
* Publish-Subscribe
* Queues
* Notification Hubs

A Service Bus **namespace** is a logical grouping of Service Bus service instances (a common and predictable address, credentials management)

### Service Bus Relay

This is a configurable exchange. It can be bidirectional if you want.

### Management Credentials

Service Bus uses a Shared Access Signature (SAS) to authenticate access to the messaging entities within the namespace. You can also use a simple web token (SWT) or SAML token from a provider.

Once you grant access to the queue - **they have access to the queue**.

### Service Bus Notification Hubs

PUSH messages for mobiles... bleh...


--------

# Securing Azure

## Azure Active Directory

'Azure AD Connect' lets you use single sign on to get into Office365, company web apps, and domain login, all using the azure AD creds.

Azure Active Directory Premium corresponds to Microsoft Identity Manager 2016 (https://www.microsoft.com/en-us/cloud-platform/microsoft-identity-manager)

### Management

You can manage tenants through: Microsoft Azure AD Portal, Windows Intune Account Portal, Microsoft Azure Management Portal, or Office 365 Account Portal.

In the Management Portal, you can create, modify, and delete user accounts, and manage passwords.

### MFA

For multi factor authentication, 

