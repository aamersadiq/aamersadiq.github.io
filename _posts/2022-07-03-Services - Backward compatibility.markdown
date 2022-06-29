---
title:  "Services - Backward compatibility"
date:   2022-06-28 9:15:43
categories: [services]
tags: [services]	
---
<h3>Introduction</h3>
<p>
This page outlines how to develop REST API services to keep it backward compatible with the older version.  An API is backward compatible between releases if the clients are able to work with a new version of the API seamlessly. It allows clients to continue using the existing REST API and migrate their applications to the newer API version when they are ready.
</p>
<h3>Versioning</h3>
<p>
Versioning an API allows us to support different functionality for the same resource. 

For example, consider a patients API for managing patients (http://www.store.com/customers). The first iteration has an endpoint that creates a customer with first name, last name and date of birth. Six months later, it is decided that every new patience now must include a phone number. What should we do with the existing API?

There are essentially two options:

<ol>
<li>Update the user API to require a phone number with every request.</li>
<li>Simultaneously support the old and new user APIs.</li>
</ol>
With option 1, any request that doesn’t include the new parameter is rejected as a bad request. This is easy to implement, but it also breaks existing API clients.

With option 2, new API is implemented and  original API is also updated to provide some reasonable default for the phone number (also check Backwards compatible DB migration if phone is stored in database). This is definitely more work, but it doesn’t break any existing API clients.

There are few options to version API. Following options are considered.
</p>
<h4>URL</h4>
<p>
This is most common way and can be achieved using either the path:

POST http://www.store.com/v2/customers
Or by using query parameters:
POST http://www.store.com/customers?v=2

URLs are convenient because they’re a required part of every request, so consumers have to deal with it. Most frameworks log URLs with every request, so it’s easy to track which consumers are using which versions.
</p>
<h4>Headers</h4>
<p>
Versioning can be achieved with a custom header name that services understand:

API-Version: 2

Or use the Accept header to include custom extensions:

Accept: application/vnd.store.v2+json

Using headers for versioning is more in line with RESTful practices as URL should represent the resource, not some version of it. Additionally, headers are already great at passing what is essentially metadata between clients and servers, so adding in version seems like a good fit.

On the other hand, headers are cumbersome to work with in some frameworks, more difficult to test, and not feasible to log for every request. Some internet proxies may remove unknown headers, meaning we’d lose our custom header before it reaches the service.
</p>
<h3>How long old versions should be supported?</h3>
<p>
When new version of API is created, old version should be market as obsolete. Once all the clients are migrated to use new version, support for the old version can be dropped. It is good practice to check the logs for no activity on old version of API before deleting it.
</p>

