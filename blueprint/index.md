---
title: Update the presence of a Genesys Cloud user based on a Salesforce presence change and vice versa
author: kristen.oliphant
indextype: blueprint
icon: blueprint
image: 
category: 3
summary: |
  This Genesys Cloud Developer Blueprint demonstrates how to configure Genesys Cloud for Salesforce Service Cloud Voice to use enhanced status syncing. Out of the box, the package provides high level status management. This blueprint will show a way to provide more detailed status mapping between Genesys Cloud and Salesforce Omni-Channel.
---

This Genesys Cloud Developer Blueprint demonstrates how to configure Genesys Cloud for Salesforce Service Cloud Voice to use enhanced status syncing. Out of the box, the package provides high level status management. This blueprint will show a way to provide more detailed status mapping between Genesys Cloud and Salesforce Omni-Channel.

This blueprint provides an example Salesforce Apex class that demonstrates how to handle the status change events and how to respond. The events are triggered by both Salesforce status changes and Genesys Cloud status changes. This blueprint can become fully functional when you replace the ids in the class with the ids from your Genesys Cloud and Salesforce orgs.

{diagram}

## Scenario

Customers who utilize Workforce Engagement Management will require a deeper status integration than what is available out of the box in our Genesys Cloud for Salesforce Service Cloud Voice package today. 

## Solution

This blueprint explains how to extend the GenesysSCVExtension.Status class of the Genesys Cloud for Salesforce Service Cloud Voice package. It shows you how to setup your Apex class to listen to the Lightning Message Channel status change events from both Genesys Cloud and Salesforce. It shows you the format of the incoming and outgoing data and demonstrates how you can customize your status mapping.

## Solution components

* **Genesys Cloud** - A suite of Genesys cloud services for enterprise-grade communications, collaboration, and contact center management. You create and manage statuses in Genesys Cloud.
* **Salesforce** - The Salesforce cloud customer relationship management (CRM) platform. 
* **Genesys Cloud for Salesforce Service Cloud Voice** - The Genesys Cloud integration that embeds Genesys Cloud inside Salesforce Service Cloud Voice.
* **Genesys Cloud for Salesforce Service Cloud Voice managed package** - The managed package that contains all the installation components necessary to run Genesys Cloud for Salesforce Service Cloud Voice.
* **Genesys Cloud API** - A set of RESTful APIs that enables you to extend and customize your Genesys Cloud environment. This solution uses the API to get status ids.

## Prerequisites

### Specialized knowledge

* Administrator-level knowledge of Genesys Cloud
* Administrator-level knowledge of Salesforce and programming experience with Apex code

### Genesys Cloud account

* A Genesys Cloud license. For more information, see [Genesys Cloud pricing](https://www.genesys.com/pricing "Opens the Genesys Cloud pricing page") on the Genesys website.

* The solutions engineer assigned the following permissions
  *  **Integration** > **salesforceServiceCloudVoice** > **Add**
  *  **Integration** > **salesforceServiceCloudVoice** > **Edit**
  *  **Integration** > **salesforceServiceCloudVoice** > **View**

TODO: Need permanent link address
For more information, see [Requirements for Genesys Cloud for Salesforce Service Cloud Voice](https://help.mypurecloud.com/?p=166994 "Opens the Requirements for Genesys Cloud for Salesforce Service Cloud Voice article") in the Genesys Cloud Resource Center.

### Salesforce account

TODO: Need permanent link address
* A Salesforce organization with the Genesys Cloud for Salesforce Service Cloud Voice integration installed and configured. For more information, see [Install the Salesforce Service Cloud Voice package](https://help.mypurecloud.com/?p=39356/ "Opens the Install the Salesforce Service Cloud Voice package article") in the Genesys Cloud Resource Center.

* The Salesforce organization and the Genesys Cloud for Salesforce Service Cloud Voice integration configured for enhanced status syncing. For more information, see TODO: Need permanent link address in the Genesys Cloud Resource Center.

* The solutions engineer assigned a System Administrator profile. For more information, see [Standard Profiles](https://help.salesforce.com/articleView?id=standard_profiles.htm&type=5 "Opens Standard Profiles") in the Salesforce documentation.

## Implementation steps

### Create a new Apex class in Salesforce

1. Create the **`StatusSyncManager`** class in Salesforce
  1. In the Developer Console, click **File** > **New** > **Apex Class**.
  2. For the class name, enter **StatusSyncManager** and then click **OK**.
  3. Replace the default code with the code found at `src/classes/StatusSyncManager.cls`.
  4. Save the file. 

### Update Genesys Cloud status ids

1. Get the status ids from your Genesys Cloud org by calling `https://api.{region}.com/api/v2/systempresences`. For more information, see [API Explorer](https://developer.genesys.cloud/devapps/api-explorer "Opens the API Explorer") in the Genesys Cloud Developer Center.
2. In the **StatusSyncManager** class, update the `CHANGE ME` text anywhere a new **GenesysTargetStatus** class is being created with a valid id from the results in the previous step. 
3. Save the file.

### Update Salesforce status ids

1. Get the status ids from your Salesforce org by running a query
  1. In the Developer Console, click the **Query Editor** tab in the middle of the console.
  2. Enter `SELECT DeveloperName, Id, MasterLabel FROM ServicePresenceStatus WHERE IsDeleted = false`.
  3. Click the **Execute** button.
  4. In the **StatusSyncManager** class, update the `CHANGE ME` text anywhere a new **SalesforceTargetStatus** class is being created with a valid id from the results in the previous step.
  5. Save the file.

### Configure the Genesys Cloud for Salesforce Service Cloud Voice package

1. Go to the **Genesys Cloud for SCV Settings** component in Salesforce.
2. Select the desired contact center.
3. Under Enhanced Status Syncing, select the Extension Class of **StatusSyncManager**.
4. Save the settings.
5. Refresh the page for the new settings to take affect.

### Test your solution

1. Log into Salesforce.
2. Log into Genesys Cloud in Salesforce in the Omni-Channel widget.
3. In another tab, log into Genesys Cloud.
4. Change your status in the Omni-Channel widget to one that you mapped in the **StatusSyncManager** class.
5. In the Genesys Cloud tab observe the status change accordingly.
6. Change your status in the Genesys Cloud tab to one that you mapped in the **StatusSyncManager** class.
7. In the Omni-Channel widget observe the status change accordingly.

## Additional resources

TODO: Need permanent link address
* [About Genesys Cloud for Salesforce Service Cloud Voice](https://help.mypurecloud.com/?p=65221 "Opens the About Genesys Cloud for Salesforce Service Cloud Voice article") in the Genesys Cloud Resource Center
* [Getting Started with Apex](https://developer.salesforce.com/docs/atlas.en-us.apexcode.meta/apexcode/apex_intro_get_started.htm "Opens the Getting Started with Apex page") in the Salesforce documentation.
* [Create Presence Statuses](https://help.salesforce.com/s/articleView?id=sf.service_presence_create_presence_status.htm&type=5 "Opens the Create Presence Statuses page") in the Salesforce documentation.
* The [enhanced-status-syncing-with-salesforce-service-cloud-voice](https://github.com/GenesysCloudBlueprints/enhanced-status-syncing-with-salesforce-service-cloud-voice "Opens the enhanced-status-syncing-with-salesforce-service-cloud-voice repository in GitHub") repository in GitHub
