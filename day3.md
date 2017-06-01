# Resilience

(end of module 5)

There are recommended patterns for implementing resilience:

 1. Retry
 2. Valet Key
 3. Sharding

## Retry Pattern

This is just retrying until it works or you exceed the maximum retries. This assumes that errors are transient; if the app is *down* down, then retrying is useless. EF 6.0 has a retry policy built in!

## Valet Key

Ok, so say you expect clients to request lots of different secured resources but you don't want to validate them every time because you get 2M requests/second and that would be expensive. Instead, you have a service which authenticates users and hands out keys. When you access the final resource, it accepts your key(?) [not sure how this last part works and nobody will explain it to me]

## Sharding

There are no resources on this, but this is good: https://engineering.instagram.com/sharding-ids-at-instagram-1cf5a71e5a5c

## Redis Cache

Azure can package up Redis Cache for you and label it "Azure Redis Cache"

--------

# Storing Tabular Data in Azure (module 6)

What if you had registration forms for events but every event took different info? Without a well-defined schema, we can't be relational. So we can use Azure Storage for flexibly-shaped data.

Remember that having storage seperate to the VM or app is good for availability because they don't both go down at once.

 1. Blobs - files, drives, multimedia
 2. Tables - store related records, unstructured tables, efficient lookups
 3. Queues - Simple messages or requests

## Blobs

https://docs.microsoft.com/en-us/rest/api/storageservices/understanding-block-blobs--append-blobs--and-page-blobs

This is file storage. OneDrive is probably built on it. There are different types of blob which are good for different things:

 * Block - ideal for text or binary files (media), can be protected by Microsoft login token, like sharing on OneDrive is.
 * Append - ideal for logging or files with frequent writes.
 * Page - small pages (512 byte) optimized for random read/write - like Page Files or RAM, good for cache. You can overwrite up to 4MB in a write and the max size is 1TB.

 ## Geo-Replication

  - **Locally Redundant Storage (LRS)** - stored on 3 nodes in one datacenter
  - **Zone Redundant Storage (ZRS)** - stored in 3 different datacenters in 1 region unless the region lacks capacity and then another close one is used
  - **Geo Redundant Storage (GRS)** - replicated async to 3 further nodes in your selected secondary datacenter. You can have the GRS read only and then it's called RA-GRS

## Accessing Storage Data

 * Access to resources are authenticated by using a shared key for your storage account
 * You can configure blobs to allow anonymous access
 * You can generate shared access signatures to give a client controlled, temporary access to a storage resource
 * Storage accounts are given two keys that can be regenerated at any time
 * Allows you to rotate keys and regenerate keys on a scheduled basis without your application losing access to the resources

 --------

 ## Azure Storage Tables

Table storage is a NoSQL database - a key-value store

Tables contain entities that are split up across multiple nodes using a 'partition key'

Entities have an index, which is a combination of the partition and row keys
Properties of a particular entity are implemented as a collection of key-value pairs