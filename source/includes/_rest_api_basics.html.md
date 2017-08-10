## What is a REST API?

A REST API is a way to programatically transfer information over the web using a predefined schema. Appboy has created many different endpoints with specific requirements that will perform various actions and/or return various data. API access is done using HTTPS web requests to your company's REST API endpoint. Typically this is `https://api.appboy.com`, but your Success Manager will provide an alternative endpoint URL if necessary.

<aside class="notice">
Customers using Appboy's EU database should use <code>https://rest.api.appboy.eu/</code>. For more information on REST APIs endpoints for customers using Appboy's EU database see <a href="">FAQs</a>.
</aside>

## API Definitions

Below is some terminology that you may see in the Appboy REST API documentation and what it means.

### Endpoints

Appboy manages a number of different instances for our Dashboard and REST Endpoints. When your account is provisioned you will login to one of the corresponding URLs below. Use the correct REST Endpoint based on which instance you are provisioned to. If you are unsure, open a support ticket or use the table below to match the URL of the dashboard you use to the correct REST Endpoint.

Instance  | Dashboard URL             | REST Endpoint
----------|---------------------------|---------------------------------
Appboy 01 | `dashboard.appboy.com`    | `https://api.appboy.com`
Appboy 02 | `dashboard-02.appboy.com` | `https://rest-02.iad.appboy.com`
Appboy EU | `dashboard.appboy.eu`     | `https://rest.api.appboy.eu`

### Company Secret Explanation

The `company_secret` was formerly included with all API requests but has been deprecated as of October 2014. This field will be ignored for all future API requests to ensure backwards compatibility.

### App Group Identifier Explanation

The `app_group_id` indicates the app title with which the data in this request is associated. It can be found in the [Developer Console]() section of the Appboy dashboard.

###  App Identifier Explanation

For Custom Events and Revenue, you may want to specify a particular variant of the App in which the event occurred. For example, if you have event data on your servers that you know came from the Android version of your app, you might want to indicate that so that it's reflected on the dashboard. In that case, you will provide the appropriate app identifier. It can be found in the [Developer Console]() section of the Appboy dashboard.

###  External User ID Explanation

The `external_id` serves as a unique user identifier for whom you are submitting data. This identifier should be the same as the one you set in the Appboy mobile SDK in order to avoid creating multiple profiles for the same user.

###  Appboy User ID Explanation

The `appboy_id` serves as a unique user identifier that is set by Appboy. This identifier can be used to delete users through the REST API in addition to external_ids.

For more information see:

- [Setting User IDs - iOS](9)
- [Setting User IDs - Android](10)
- [Setting User IDs - Windows Phone 8](11)
- [Setting User IDs - Windows Universal](12)

##  API Limits

The Appboy API infrastructure is designed to handle high volumes of data across our customer base. We enforce API rate limits in order to ensure responsible use of the API.

Initial API rate limit | Value
--|--
Requests of any kind, legacy Free customers|100 per hour
Requests of any kind, all other customers|50,000 per hour
Requests to the Send endpoint specifying a Segment or Connected Audience|250 per minute
Users modified per User Track request|50 users

REST API rate limit increases are considered based on need for customers who are making use of the API batching capabilities. Please batch requests to our API endpoints:
- A single request to the User Track endpoint can contain Purchases, Custom Events and/or Custom Attribute updates for up to 50 users, specified by `external_id`
- A single request to the Messaging endpoints can reach any one of the following:
  - Up to 50 specific `external_ids`, each with individual message parameters
  - A segment of any size created in the Appboy dashboard, specified by its `segment_id`
  - An ad-hoc audience segment of any size, defined in the request as a [Connected Audience](7) object.

The response headers for any valid request include the current rate limit status:

Header Name             | Description
----------------------- | -----------------------
`X-RateLimit-Limit`     | The maximum number of requests that the consumer is permitted to make per hour.
`X-RateLimit-Remaining` | The number of requests remaining in the current rate limit window.
`X-RateLimit-Reset`     | The time at which the current rate limit window resets in UTC epoch seconds.

<aside class="success">
  If you have questions about API limits please contact your Customer Success Manager or please <a href="">email Support</a>.
</aside>

##  API IP Whitelisting

For additional security, you can specify a whitelist of IP addresses and subnets which are allowed to make REST API requests for a given App Group. To whitelist specific IP addresses or subnets, navigate to the Developer Console in the Appboy dashboard and modify the section shown below:
