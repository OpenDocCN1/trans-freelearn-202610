If we review the fields of the prebuilt **Receipt** document type, we can see that the fields match up with those from our current state understanding (Figure 8.19):

|  |  |
|---|---|---|
| **Taxonomy Design** |  |  |
| **Fields from Current State understanding** | **Fields from prebuilt Document Type** |  |
| Vendor Name | Receipt Name |  |
| Receipt Date | Receipt Date |  |
| Tax Amount | Tax Amount |  |
| Total Amount | Total Value |  |

Figure 8.19 – Taxonomy Design

This mapping of the fields from our current state understanding against the fields of the prebuilt document type ensures that we don't have to edit the taxonomy at all. If there were additional fields that we wanted to capture, we could easily add the fields to the **Receipt** document type or create our very own document type. However, in this case, since we have a mapping, we can leverage the prebuilt document type and continue to the next section of the Document Understanding framework, **digitization**.

# Setting up the digitizer

The next step of the *Document Understanding framework* is digitization. Digitization is the process of taking input data and scanning it into machine-readable text. Once the data is digitized, we can classify and extract the relevant data fields to create actionable steps for automation.