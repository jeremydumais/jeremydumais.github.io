---
layout: post
title:  "Azure : Azure Function - Accessing a Local API with Hybrid Connections on Linux"
date:   2026-06-14 06:30:00 -0400
categories: azure
---

When building cloud-native applications, there are situations where an Azure Function still needs access to resources hosted on a local network or private server.

In my case, I had an Azure Function acting as a subscriber in a messaging system and calling an API hosted on an Ubuntu VPS. To make things work quickly during development, the API was temporarily exposed publicly through a reverse proxy.

Although functional, this approach was not ideal for production. The API was publicly reachable, even if protected, and the overall architecture felt more like a workaround than a proper solution.

The goal was simple: keep the API private while allowing an Azure Function to securely access it.

This is where Azure Hybrid Connections comes in.

## The architecture

The flow looked like this:

```text
Publisher Function
        ↓
Azure Service Bus Topic
        ↓
Subscriber Function (Premium)
        ↓
Azure Hybrid Connection
        ↓
Ubuntu VPS
        ↓
Local API
```

The subscriber receives a Service Bus message and then calls an API hosted on a private server.

Instead of exposing the API publicly through a reverse proxy, the communication happens through an outbound-only secure tunnel initiated by the server itself.

## The first problem: Flex Consumption

My subscriber Function App was initially deployed using the Flex Consumption plan.

Unfortunately, Hybrid Connections are not supported for this scenario.

The solution was to create a new Function App using the **Premium plan**.

One important detail: you cannot directly convert a Flex Consumption Function App to Premium. A new Function App must be created and the code redeployed.

For testing purposes, I used:

```text
Linux
Premium Plan
EP1
```

After deployment, I copied all existing environment variables from the original Function App.

Example:

```text
TargetApiBaseUrl
TargetApiEquipmentPath
TargetApiLoginPath
TargetApiUsername
TargetApiPassword
ServiceBusConnection
```

Most values remained unchanged except for the API base URL, which would later point to the Hybrid Connection endpoint.

## Creating the Azure Relay namespace

Hybrid Connections require an Azure Relay namespace.

In Azure Portal:

```text
Create Resource
→ Relay
```

Configuration:

```text
Pricing Tier: Standard
Region: Same as Function App
```

The namespace acts as the communication bridge between Azure and the server hosting the local API.

At first, the portal terminology was confusing because it asks for a **Service Bus Namespace**, but this is actually the Azure Relay namespace used internally by Hybrid Connections.

## Creating the Hybrid Connection

Inside the Function App:

```text
Networking
→ Hybrid Connections
→ Add Hybrid Connection
→ Create New Hybrid Connection
```

Initially, I tried using:

```text
localhost
127.0.0.1
```

for the endpoint host.

Azure immediately rejects those values because reserved loopback addresses are not allowed.

Instead, a hostname must be used.

My final configuration looked like this:

```text
Hybrid Connection Name:
mylocal-api

Endpoint Host:
mylocal-api.local

Endpoint Port:
8080

Service Bus Namespace:
relay-msg-dev
```

The hostname itself does not need to exist publicly, but it must resolve on the Linux server hosting the API.

## Configuring Linux hostname resolution

On the Ubuntu VPS, I added a local hostname mapping.

Edit:

```bash
sudo nano /etc/hosts
```

Add:

```text
127.0.0.1 mylocal-api.local
```

This makes the hostname resolve locally to the API running on the server.

Before moving further, I verified connectivity:

```bash
curl http://mylocal-api.local:8080/api/health
```

If this request fails locally, Hybrid Connections will also fail.

## Installing Hybrid Connection Manager on Linux

The next step is installing the Hybrid Connection Manager (HCM).

First, install Azure CLI:

```bash
curl -sL https://aka.ms/InstallAzureCLIDeb | sudo bash
```

Login:

```bash
az login
```

Then install HCM:

```bash
sudo apt update
sudo apt install tar gzip wget -y

wget https://download.microsoft.com/download/HybridConnectionManager-Linux.tar.gz
tar -xf HybridConnectionManager-Linux.tar.gz

cd HybridConnectionManager
chmod +x setup.sh

sudo ./setup.sh
```

Verify installation:

```bash
hcm --help
```

## Adding the Hybrid Connection

Once installed:

```bash
hcm add
```

The command launches an interactive configuration.

Select:

* Azure subscription
* Relay namespace
* Hybrid connection

Once completed:

```bash
hcm list
```

Expected result:

```text
Status: Connected
```

This means Azure can now securely reach the local server.

## Updating the Function App configuration

Finally, I updated the subscriber Function App environment variable:

Before:

```text
https://public-api-url.com
```

After:

```text
http://mylocal-api.local:8080
```

The rest of the settings remained unchanged.

Example:

```text
TargetApiEquipmentPath=/api/equipment
TargetApiLoginPath=/api/login
TargetApiUsername=my-user
TargetApiPassword=my-password
```

One thing I noticed during testing: Azure Function environment variables can take a few minutes to refresh after changes.

Restarting the Function App helped force the configuration reload.

```text
Function App
→ Restart
```

## Troubleshooting

Here are the main issues I encountered.

### Function not consuming Service Bus messages

At one point, messages were visible in the subscription, but the subscriber was not processing them.

The issue turned out to be stale environment variables.

Restarting the Function App and waiting a few minutes resolved the problem.

### Hybrid Connection not working

Verify:

```bash
hcm list
```

The connection should show:

```text
Connected
```

Then validate hostname resolution locally:

```bash
curl http://mylocal-api.local:8080/api/health
```

### Reserved hostname error

If Azure says:

```text
This name is reserved
```

avoid:

```text
localhost
127.0.0.1
```

Use a hostname instead and map it in `/etc/hosts`.

## Final thoughts

After implementing Hybrid Connections, the API no longer needed to be exposed publicly through a reverse proxy.

The Function App could securely access the local API while keeping everything private and outbound-only from the server.

Overall, the setup ended up being surprisingly clean once all the moving pieces were understood:

* Azure Function Premium
* Azure Relay
* Hybrid Connection
* Linux HCM
* Local hostname resolution

If you are building Azure Functions that still need to interact with on-premise or privately hosted services, Hybrid Connections is definitely worth considering.

