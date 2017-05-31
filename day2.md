# Day 2

## Web Apps

 * PaaS
 * Fully managed
 * Flexible dev languages and deploy methods
 * Scalable and autoscaling (at certain price tiers)
 
## Web Deploy

We started a lab project using the Web+SQL project template. This binds the database to the web project, so that the publish profile transforms the connection string to use the sibling database. Very cool!

You can override any web.config appSettings key/value in the Azure portal! Also cool! I guess this bounces the app for a moment like changing the web.config would. I don't think it changes the actual web config.

The lab comes with a ready built solution which is an events tracking website with code-first database migrations which get imported to the live DB when you publish.

--------

## SQL on Azure

Lots of info about SQL-in-VM vs SQL Azure. Things to consider:

 * Licensing
 * Auth scheme
 * Compatability


--------

## Microsoft Operations Management Suite

http://mms.microsoft.com

https://stegriffdev.portal.mms.microsoft.com

A monitoring portal for IT and DevOps

--------

## Lab - Entity Framework migrations in ASP.Net

--------

## Designing for Resilience

One app is not enough - we need to be able to start multi-instances of a web app

Traditional: Is your website overloaded? Start more **servers** (starts web server, app, database ... that's more than we need!)
Now: All we really need to start up is *either* more IIS processes to handle requests *or* more app code - maybe both, but we want the flexibility

### Micro services!

 > yeah boi

Down with monliths, split the business processes into 'Partitioning Workloads' - each a little web app

Imagine Instagram.net:

 * Web UI - Web App
 * SignalR hub - VM
 * Image processor - Background Worker
 * Thumbnail storage - Storage Service

	An image is uploaded
	Is stored in a Onedrive-like bucket of files
	Send a notification to service bus
	SB tells a web app 
	WA resizes it for all different devices
	Cached images go to storage blobs
	More notifications
	Clients pull the data with SignalR and show the thumbnail with a cute ping sound
	

### Load balancing

There are HW and SW load balancers, but we recommend SW. In fact a physical one is sold on Azure as a dedicated VM which is prepacked with load balancing software!

 * Have the same service provided by multiple instances.
 * Use a load balancer to distribute requests across all.
 * Importantly, this gives you a **single point of entry** to the app service

Different LB algorithms are available

 
### Sticky Sessions

What about a mobile which is likely to drop connection?
We can have an LB specifically for Mobile, which sets Stickiness so that mobile goes back to the same boxen and the session is not lost even though the connection drops in and out.


### Queues

A queue mediates the messages to the app instances. When it can't reach one instance of an app, instead it sends it to another. If they're all refusing, the message goes into a 'failed' queue.


### State Management


