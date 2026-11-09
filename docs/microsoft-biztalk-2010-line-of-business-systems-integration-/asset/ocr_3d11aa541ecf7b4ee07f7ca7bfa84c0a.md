# IDOC Deep Dive

It is now time to take a closer look into the structure of IDOCs to better understand some of the segments that a BizTalk Developer must understand.

In each IDOC schema, you can expect the following elements:

+   - EDI_DC40
+   - SAP table(s), that is, E2CONF5
+   - guid

The EDI_DC40 node is also referred to as the Header node, or record. Within this node we will discover several fields that we need to populate including:

| Element Name | Purpose |
|---|---|
| IDOCTYP | The basic structure of the IDOC. |
| CIMTYP | The structure of the custom extension. If an SAP basic structure is extended, a name must be given for the extension. |
| MESTYP | A logical representation of the message. The name should provide a business contextual meaning. |
| SNDPOR | Sending System Port |
| SNDPRN | Sending System/Partner |
| SNDPRT | Sender Partner Type |
| RCVPRT | Receiver Partner Type |
| RCVPRN | Receiving System/Partner |
| IDOCTYP | The basic structure of the IDOC |

Depending upon the IDOC there may be one or more SAP tables that are represented within the IDOC. In the case of the CONF32 IDOC, there is only one called E2CONF5. Within this segment, or node, there are a number of fields that we need to populate.