# Day 1

Constructing Azure Virtual Machines

## Principles

Underprovision - don't overprovision. We can set usage rules to scale up for us later. **Compute on demand**

Azure VM is based on Hyper-V, so you can transfer VHDs between cloud and local

## Types of disk

VHD - Virtual Hard Disk available since server 2005

VHDX - Generation 2 - new features like UEFI. Azure doesn't support this (yet) but Azure will automatically convert these to VHDs for you.

## Replication

Hyper-V Replica is a tech that lets you failover to a new server with <5 seconds of data loss using block-level change 'bits/RPC' on port 80 or 443. In Azure, this service is called Azure Site Recovery. It is charged on Pay As You Use, such that provisioning it is free, and you only start paying when you start using it.

## Images

When you provision a VM and put SQL Server on it, you are charged the licensing cost in the running cost per hour - it's hidden. During setup, you can tick "I have my own license" to negate that - no details needed - but you will be running unlicensed if you are lying.

You can be unlicensed by the above, or if you upload a VHD to run a clone of an internal server without buying additional licensing.

User-contributed VM images: http://vmdepot.msopentech.com

## Uploading an image

 0. RDP onto the physical VM
 0. Open an admin command prompt and run `sysprep`
 0. Use `Capture` button in Management Portal
 
## Kitten and Cattle

If you have to keep a server purring like a kitten because you don't know what will happen if it shuts down, you've already lost. Servers should be cattle - one dies, you replace it.

This is acheivable using the 15-minute snapshots of VMs; it's like system restore, if you mess a machine up, you just restore another machine to the last snapshot from 15 mins ago.

## Virtual networks

## High Availability

Think of an Availability Set as a container. To make a solution highly available, you need to split your provisioned machines across more than one availability set. This is different to a Scale Set (covered later).

AS-1: VM-1A, VM-1B
AS-2: VM-2A, VM-2B

I read this tut about [setting up RabbitMQ on Azure](http://moimtechview.blogspot.co.uk/2015/01/rabbitmq-high-availability-clusters-on.html) which says "The VMs need to be in the same cloud service and the same availability set, to achieve redundancy and high availability" but **this is wrong** - they need to be in different availability sets!

You can specify rules about how many of each machines are up at any time.

Azure Load balancers direct incoming traffic evenly to available machines

Traffic manager will look at the IP you're coming from and take you to the lowest latency server


## Desired State Configuration

Say it takes 2 hrs to install IIS, you don't want to pay-per-minute to do that. Also, if you have a ghosted VHD with IIS already installed, that could mean the machine takes longer to provision.

DSC is a PowerShell exntesion (new language features, new cmdlets) which tells the server, while it is booting/provisioning, to install a set of software, like IIS.

Beta DSCs have X in front of their names.


## Internal/External network - Custom Endpoints

If you have a load of Azure machines and services on a private Azure network, the access is internal only. To add an external access, we can use Custom Endpoints.

Custom Endpoints have an Access Control List which allows you to have not only an IP whitelist, but to specify by MAC address and even time-of-day, as IP addresses are often shared (i.e. within a home or office).

By default a new VM has an ACL in place to deny all.


## Firewall Rules

