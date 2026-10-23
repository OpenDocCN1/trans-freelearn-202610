At this stage, we have validated that the endpoint is functional. We now need to describe our plugin for OpenAI.

# Manifest

The manifest describes what your plugin is going to achieve and gives a direction to the model that tells when to trigger the plugin depending on the prompt that is submitted by the user. Here is the JSON content:

```
{
"schema_version": "v1",
"name_for_human": "PacktGPT_Plugin",
"name_for_model": "PacktGPT_Plugin",
"description_for_human": "An Assistant, you know, for searching in the Elastic documentation",
"description_for_model": "Get most recent elastic documentation post 2021 release, anything after release 7.15",
"auth": {
"type": "none"
},
"api": {
"type": "openapi",
"url": "PLUGIN_HOSTNAME/openapi.yaml",
"is_user_authenticated": false
},
"logo_url": "PLUGIN_HOSTNAME/logo.png",
"contact_email": "info@info.co",
"legal_info_url": "http://www.example.com/legal"
}
```

All the preceding fields help describe in detail the plugin we are publishing in ChatGPT, such as the name of the plugin or the URL to get the description of the API, or even the email to contact the developer. The most notable one is description_for_model, as this is the one the ChatGPT framework uses to guide the model in terms of what is expected from it.

The plugin description helps ChatGPT to understand what the plugin is providing and to set the behavior of the model. We now have to describe what the exposed APIs are in the plugin.

# OpenAPI definition

The OpenAPI definition is a standard representation of the API exposed by the plugin, including the paths, HTTP verb, and relative parameters, as you can see in the following snippet:

```
openapi: 3.0.1
info:
title: PacktGPT_Plugin
```