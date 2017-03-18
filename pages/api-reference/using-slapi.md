---
title: Using the SoftLayer API
keywords: 
last_updated: November 18, 2016
tags: 
summary: 
sidebar: crs_sidebar
permalink: using-slapi
folder: api-reference
---

{% include note.html content="This section is under construction. Please contact us on [dW Answers](https://developer.ibm.com/answers/smartspace/public-cloud-object-storage/) or [StackOverflow](http://stackoverflow.com/questions/tagged/object-storage+ibm)  with questions." %}

More information on using the SoftLayer API to create or delete credentials, check capacity usage, retrieve a UUID, and other account functions can be found at the [SoftLayer Development Network](http://sldn.softlayer.com/reference/services/SoftLayer_Network_Storage_Hub_Cleversafe_Account).

## Example script to retrieve an account ID

```python
import SoftLayer
import pprint as pp

# set known ids as variables
customer_account_id = {account_id}
my_cos_account_name = "{account_name}"
username = "{username}"
api_key = "{api_key}"

# create a softlayer client
client = SoftLayer.create_client_from_env(username=username,
                                          api_key=api_key)

# get the customer account information
account_info = client['SoftLayer_Account'].getObject(
    id=customer_account_id,
    mask='hubNetworkStorage.billingItem.id')

# get the list of accounts
cos_accounts = account_info.get("hubNetworkStorage")

# iterate over the list to find the storage account name
my_storage_account = (
    item for item
    in cos_accounts
    if item["username"] == my_cos_account_name).next()

# get the "id" for the storage account
my_cos_account_id = my_storage_account["id"]

# fetch the credentials
credentials = client[
    'SoftLayer_Network_Storage_Hub_Cleversafe_Account'].getCredentials(
    id=my_cos_account_id)

print "Account %s has an ID of %s," % (my_cos_account_name, my_cos_account_id)
print "and can be accessed with the following credentials:"
pp.pprint(credentials)
```

