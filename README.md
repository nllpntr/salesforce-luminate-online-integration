# Salesforce -> Luminate Online Integration
This is an implementation of Blackbaud Luminate Online APIs in Salesforce, for synchronizing Constituent fields between LCRM and Luminate Online more reliably.

LuminateAPI.apcx is a batchable class that performs callouts to the SRConsAPI endpoint in Luminate Online. Currently, it only supports the "update" method.

CLIContactUpdate.apxt is a before update trigger on Contact, which causes LuminateAPI to synchronize whatever fields have been set up in the Custom Luminate Mappings tab.

Custom Luminate Mapping records define the link between a Luminate Online database name and the Salesforce API name. The LuminateAPI.synchronizeContacts() method takes Trigger.newMap and Trigger.oldMap to test for changes to whatever fields have been mapped, and pushes the API callout into the Apex batch queue.

# IMPORTANT
Since I'm still a bit of a noob, and I haven't included custom settings and object/field definitions on git-hub yet, you'll need to install this package from &lbrace;package url coming soon&rbrace;. It includes Custom Settings and an object definition that is necessary to configure field mappings between systems.

# Post-Installation
Required configuration:
 1. Go to Settings->Develop->Custom Settings and click Manage next to "Custom Luminate Integration Settings"
 2. Click New
 3. In the Name field, enter 'LuminateOpenAPISettings' exactly.
 4. Enter the SRCons API URL according to your organizations configuration, in the format https://clusterN.convio.net/<orgname>/site/SRConsAPI
 5. Enter the api_key, login and password for API Admin access, available from the Open API Configuration tab in Luminate Online
 6. Once this is saved, this package can be used.
 
The package is now ready to communicate with the Constituent API. 

Field Mapping Setup:
 1. Click the Custom Luminate Field Mappings Tab
 2. Create a new record
 3. Enter a descriptive name
 4. In Online Field Name, enter the field name from your Database Configuration tab in Luminate Online. Use the "Column Name" value, i.e. custom_number1, or membership_expiration_date
 5. The Salesforce API Name field should match the API name for the corresponding contact field in LCRM. It must include prefixes and suffixes, i.e. cv__membership_expiration_date__c
 6. Upon save, this package will now post changes to the LCRM field directly to the Constituent API any time a record is created or edited.
