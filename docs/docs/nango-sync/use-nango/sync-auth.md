import Tabs from '@theme/Tabs';
import TabItem from '@theme/TabItem';

# Request Authentication

## OAuth

Nango leverages our sister project [Nango](https://github.com/NangoHQ/nango) to handle authentication with OAuth APIs. 

Nango + Nango Sync let you benefit from: 
- a full OAuth 2 and OAuth 1.0a dance implementation
- a frontend SDK that makes it easy trigger new OAuth flows from your web app
- a backend SDK & REST API that make it easy to get always-fresh access tokens for your API calls
- a CLI that makes it easy to manage your OAuth provider configs, setup different environments and debug OAuth issues
- a built-in way to authenticate Nango Syncs, using access tokens maintained fresh by Nango

### Getting started with OAuth

To get stared with OAuth, follow the instructions in the [Nango docs](quickstart.md).

Once your have successfully created a Provider Configuration and Connection, specify the `nango_provider_config_key` and `nango_connection_id` parameters in your [Sync config options](sync-all-options.md) (cf. example below).

Finally, specify where the access token should go in the request using the `${nangoAccessToken}` notation (cf. example below). It will automatically be replaced with a fresh access token before each API requests made by your Sync.

### Example: syncing Hubspot contacts with OAuth

<Tabs groupId="programming-language">
  <TabItem value="node" label="Node SDK">

```ts
import {Nango, NangoHttpMethod} from '@nangohq/node-client'

let config = {
    //==================
    // OAuth access token insertion
    //==================
    headers: { authorization: "Bearer ${nangoAccessToken}" }, // Templating is used to insert the token where you specify.

    //==================
    // Nango configuration
    //==================
    nango_connection_id: '1',                                 // Connection ID configured with Nango.
    nango_provider_config_key: 'hubspot',                     // Provider configuration configured with Nango.

    //==================
    // Other Sync parameters
    //==================
    method: NangoHttpMethod.POST,
    paging_cursor_request_path: 'after',                       
    paging_cursor_metadata_response_path: 'paging.next.after', 
    response_path: 'results',                                  
    unique_key: 'id',      
};

// Add the Sync
new Nango().sync('https://api.hubapi.com/crm/v3/objects/contacts/search', config);
```
  </TabItem>
  <TabItem value="curl" label="REST API (curl)">

  ```json
  curl --request POST \
--url http://localhost:3003/v1/syncs \
 --header "Content-type: application/json" \
 --data '{
    "url": "https://api.hubapi.com/crm/v3/objects/contacts/search",
    "headers": { "Authorization": "Bearer ${nangoAccessToken}"},
    "nango_connection_id": "1",
    "nango_provider_config_key": "hubspot",
    "method": "POST",
    "paging_cursor_request_path": "after",
    "paging_cursor_metadata_response_path": "paging.next.after",
    "response_path": "results",
    "unique_key": "id"                                  
  }'

#==================
# OAuth access token insertion
#==================
#
# Templating is used to insert the token where you specify.
#
#==================
# Nango configuration
#==================
#
# - nango_connection_id: Connection ID configured with Nango.
# - nango_provider_config_key: Provider configuration configured with Nango.
#

  ```
  </TabItem>
</Tabs>

## Other authentication methods

For other authentication methods that do not require frequent refreshes of credentials, you can simply specify the authentication information in the `headers`, `body` and `query_params` parameters in the [Sync config options](sync-all-options.md).

## Problems with your Sync? We are here to help!

If you need help or run into issues, please reach out! We are online and responsive all day on the [Slack Community](https://nango.dev/slack).