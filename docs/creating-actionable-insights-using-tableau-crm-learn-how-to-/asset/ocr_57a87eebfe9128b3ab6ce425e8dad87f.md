```
"members": [],
"recordDisplayFields": [],
"salesforceActions": []
}
],
"measures": [
{
"conditionalFormatting": {},
"field": "Amount",
"format": {
"customFormat": "[\"$#,##0.00\",1]"
},
"label": "Initial Amount"
}
],
"organizations": [],
"showDetailsDefaultFields": []
}
```

For detailed insights and instructions regarding using XMD, please reference this developer guide:

https://developer.salesforce.com/docs/atlas.en-us.bi_dev_guide_xmd.meta/bi_dev_guide_xmd/bi_xmd_intro.htm

# You might not need to code if...

As Salesforce continues to develop TCRM, more advanced querying and customizations can be performed in the GUI, requiring less work to be performed using code. This aligns with the Salesforce mantra of clicks and not code.

The following are use cases where code was once required, but now may not be:

+   - Data flow, data prep, and recipes:

Code is not required for data flows (JSON) or recipes except for complex use cases.

For a detailed reference about dashboard JSON, see this developer guide:

https://developer.salesforce.com/docs/atlas.en-us.bi_dev_guide_json.meta/bi_dev_guide_json/bi_dbjson_intro.htm.