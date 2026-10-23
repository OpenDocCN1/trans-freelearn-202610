# DUCk: Documents Under Control!

Current Document: /literature/shakespeare/Venus and Adonis.htm

Class: Work of Literature

| Property Name | Value | Type | Notes | Settabillity | Required |
|---|---|---|---|---|---|
| Owner | cn=poweruser,cn=people,dc=whozit,dc | string | The security owner of the object.; max 4000; | R/W |  |
| Compound Document State | 0 | integer | State of document, is it a standard document or a compound document.; | R/W |  |
| Document Title | Venus and Adonis | string | Document Title; max 255; | R/W |  |

The page will only display properties that can actually be updated, and current values will be supplied as defaults. As for the previous page for viewing details, the Notes column is a composite of the PropertyDescription.DescriptiveText property and a simple rendering of the property validation criteria (for example, minimum and maximum values for an integer property).

There are many aspects of full property editing that are not implemented in the DUCK prototype. These include: the use of choice lists to guide value selection, object valued properties, multi-valued properties, and binary (blob) properties. The processing of updated values from the UI is done via fairly simple parsers that don't have the user friendliness that would be needed for a production application (for example, we use methods Integer.parseInt() and Date.parse()). There will be a link for updating content in addition to the edit link. The content update will cause a document checkout, and the edit page will be for the properties of the reservation document. There will be a content upload link, and there will be the opportunity to commit the change (by doing a document checkin) or cancel.

> For the reason we described previously (in the description of adding a new document) the DUCK prototype does not implement content uploads.