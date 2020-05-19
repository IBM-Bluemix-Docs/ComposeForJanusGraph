---

Copyright:
  years: 2017,2019
lastupdated: "2019-01-11"

keywords: janusgraph, compose

subcollection: ComposeForJanusGraph

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}
{:tip: .tip}

# Settings
{: #dashboard-settings}

These features allow you to adapt your {{site.data.keyword.composeForJanusGraph_full}} service to better suit your needs and requirements.


## Upgrade Version

If a new version of the database is available, a drop-down menu appears, from which you can select which version you would like to upgrade to. Otherwise, your service is on the newest version available, and the pane displays the current version information.

![The Version pane](./images/janusgraph-version-show.png "The Version pane")


## Scaling Resources

If your service needs additional storage, or you want to reduce the amount of storage allocated to your service, you can do this by scaling resources.

1. Navigate to your service's _Overview_ page.
2. In the _Deployment Details_ panel, click **Scale Resources**. The Scale Resouces page opens.

    ![The Scale Resources page](./images/janusgraph-scale-show.png "The Scale Resources page")

3. Adjust the slider to raise or lower the storage allocated to the {{site.data.keyword.composeForJanusGraph}} service. Move the slider to the left to reduce the amount of storage, or move it to the right to increase the storage.
4. Click **Scale Deployment** to trigger the rescaling and return to the dashboard overview. 

When the scaling is complete the _Deployment Details_ pane updates to show the current usage and the new value for the available storage.

### Scaling Memory

If you have a relatively small data set but need to execute complex queries, you may keep the storage nodes at the default size of 5 GB, and scale the memory of the Engine nodes up to make your queries more performant. To add memory to Janus Graph Engine nodes, please contact support.


## Changing the password

You might find it necessary to change the password of your service.

1. Go to the _Change Password_ panel. 

  You can use the randomly generated password that is created for you, or you can type your own password into the field. To regenerate a new random password, click the dice. 
  
  ![Updating the JanusGraph password](./images/janusgraph-update-password.png "The automatic password generator")

2. Click **Update Password**. You are asked to confirm the change.
3. Click **Update Password** in the dialog to confirm the new password, or click **cancel** to cancel the change. The _Deployment Details_ pane shows the progress of the running job.

**Note:** Changing the password changes the credentials that you and your services use to connect, and invalidates your service's connection string. It can also result in downtime.

### Updating Connected Applications

Changing the password invalidates the existing connection string and generate a new one. This can cause a service interruption until connected applications are updated with the new connection string.


## Whitelists

If you want to restrict access, to your databases, you can whitelist specific IP addresses or ranges of IP addresses on your service. When the whitelist is empty, it is disabled and the deployment accepts connections from any system on the internet.

![Whitelisting IPs](./images/janusgraph-whitelist-show.png "The whitelist fields.")

### IP addresses
The *IP* field can take a single complete IPv4 address or IPv6 address with or without a netmask. Without a netmask, incoming connections must come from exactly that IP address. 

Although the *IP* field allows for IPv6, no Compose deployments are currently available to IPv6 networking, so these addresses cannot be filtered on.

### Netmasks

Use a netmask to allow a connection from a specified range of IP addresses. The IP address must be fully specified, which means entering, for example, 192.168.1.0/24 rather than 192.168.1/24.

### Description
The *Description* can be any user-significant text for identifying the whitelist entry - a customer name, project identifier, or employee number, for example. The description field is required.

### Compose Services
Whitelist entries are automatically added to Compose's servers to allow them to connect.

### Removal
To remove an IP address or netmask from the Whitelist, click *Remove* next to the IP address.
When all entries on the whitelist are removed, the whitelist is disabled and all IP addresses are accepted by the TCP access portals or Mongos router portals.
