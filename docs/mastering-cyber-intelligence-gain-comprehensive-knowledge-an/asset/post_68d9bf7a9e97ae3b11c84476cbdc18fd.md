
- **Exploit targets**: *Exploit targets* relate to system **vulnerabilities** that threat actors can exploit. The latter use defined TTPs to compromise systems. Vulnerabilities in software and hardware are the leading causes of breaches. *Chapter 11, Usable Security: Threat Intelligence as Part of the Process*, looks at some basic best practices to secure software and hardware. STIX defines the structure of exploit targets using industry best practices such as vulnerability and exploit databases.
- **Course of action**: The *course of action* relates to what the security team does after a cyber incident. Analysts take necessary measures to prevent or stop cyber attacks as they are discovered. The course of action must define the steps required to remediate the event. STIX provides a standard structure to present a course of action, and organizations can share how they acted against a threat with the community. STIX's *course of action* consists of (1) the relevant **Stage** in threat management, which determines what is done – remedy or response, (2) **Type** of action, (3) action **description**, (4) action **objective**, (5) action **impact**, (6) action **cost**, and (7) the **efficacy**.

STIX presents the standard that follows certain principles to ensure that cyber threats are explicitly presented so that when they are shared, CTI analysts have enough data to push for protective security actions. The preceding descriptions of the STIX elements are based on STIX v1. In the next section, we will briefly look at STIX v2.

## Understanding the STIX v2 standard

STIX is an evolving standard. The standard was publicly introduced in 2017 and since then has moved from STIX 2.0 to STIX 2.1. The difference between the two versions comes from newly added objects, concepts, and more (https://bit.ly/3cE02uB). In this section, we look at the global STIX v2.

The STIX v2 standard uses JSON to present threat components and introduces two categories of objects: **STIX Domain Objects (SDOs)** and **STIX Relationship Objects (SROs)**. All threat event aspects are presented using STIX objects and their relationships. In STIX v2, threat information can be presented using the native approach (JSON) or graphically by joining 2 or more SDOs using SROs. There are 16 STIX domain objects that represent a threat in STIX v2. These elements include the 8 aspects of STIX v1 plus the following:

- **Identity**: Groups, organizations, or individuals with their respective classes (for example, individuals, target industries, and operating regions).
- **Intrusion set**: This refers to malicious behaviors and resources characterizing a specific threat actor. STIX tries to group all attributes of threat actors into a single set of behavior and resources.
- **Malware:** STIX defines a standard way to represent malware information to facilitate sharing within the threat intelligence community.
- **Malware analysis:** STIX helps model malware information resulting from malware analysis or reverse engineering.
- **Tool:** This refers to the resources used by threat actors to orchestrate attacks.
- **Location:** This refers to the geographic location of a threat represented as a country, continent, physical address, or latitude and longitude coordinates. STIX helps the CTI model attack location.
- **Note:** This SDO is used to contextualize a threat and provide information not available in any STIX objects. The additional information should be modeled as text.
- **Opinion:** It is related to the assessment of STIX objects created by any other entity. Opinions are subjective and should be addressed with care. For example, a CTI analyst can tag an indicator as false positive if the campaign object linked to the indicator is not reliable or disputed.

STIX also adds a structure for reporting threat incidents.

> **Domain Object Changes in STIX v2**
> Some of the domain objects from STIX v1 have been extended with name modifications in STIX v2. For example, the TTP object in v1 has been extended to **attack-pattern**, which includes all methods used by threat actors. The **exploit target** domain has been extended to **vulnerability** in STIX v2, which is much broader.

SROs define interactions between SDOs. An SRO interaction can be a *relationship* or a *sighting*. When an SRO is of the relationship type, a `relationship_type` object is defined and listed by property name and target. When an SRO is of the sighting type, it means that a specific object has been observed. The following code example illustrates a STIX v2 JSON file showing a simple indicator object. The three blocks of code display three types of malware information that can be in a single STIX file:

```
[
{
    "type": "indicator",
    "id": "indicator--ID",
    "created_by_ref": "identity Reference ",
    "created": "Date of creation",
    "modified": "Date of indicator modification",
    "labels": [
        "indicator label"
    ],
    "name": "Cobra Venom Malware",
    "description": "indicator description",
    "pattern": "[ file pattern ]",
    "valid_from": "Date from which the indicator became valid"
},
```

The preceding code represents an element of the indicator type. The indicator is linked to a specific piece of malware. The following code displays an element of the relationship type. This relationship links the indicator in the preceding code with the malware in the last code example:

```
{
    "type": "relationship/sighted",
    "id": "relationship--ID",
    "created_by_ref": "Identity--reference",
    "created": "Date of creation",
    "modified": "Date of modification",
    "relationship_type": "indicates",
    "source_ref": "indicator--ID",
    "target_ref": "malware--ID"
},
```

The following is the last piece of code that displays the STIX element of the malware type (Cobra Venom), which is linked to the indicator created in the first code snippet:

```
{
    "type": "malware",
    "id": "malware--ID",
    "created": "Date of Creation",
    "modified": "Date of modification",
    "created_by_ref": "identity--reference",
    "name": "Cobra Venom"
}
]
```

The JSON code snippet shows two objects of the malware and indicator types with their respective attributes. The indicator used is indicator label. The relationship type is either relationship or sighting. It uses indicator--ID as the source reference and malware--ID as the target reference to show that it is linking the two objects (indicator and malware). The following example represents a file hash linked to the Poison Ivy malware (accessible through https://oasis-open.github.io/cti-documentation/stix/examples.html). It shows a CTI analyst how to model an indicator linked to an object. The code is divided into three blocks, each describing an object type:

```json
{
  "type": "bundle",
  "id": "bundle--2a25c3c8-5d88-4ae9-862a-cc3396442317",
  "objects": [
    {
      "type": "indicator",
      "spec_version": "2.1",
      "id": "indicator--a932fcc6-e032-476c-826f-cb970a5a1ade",
      "created": "2014-02-20T09:16:08.989Z",
      "modified": "2014-02-20T09:16:08.989Z",
      "name": "File hash for Poison Ivy variant",
      "description": "This file hash indicates that a sample of Poison Ivy is present.",
      "indicator_types": [
        "malicious-activity"
      ],
      "pattern": "[file:hashes.'SHA-256' = 'ef537f25c895bfa782526529a9b63d97aa631564d5d789c2b765448c8635fb6c']",
      "pattern_type": "stix",
      "valid_from": "2014-02-20T09:00:00Z"
    },
    {
```

In the preceding code example, the first STIX object element is described. The first element is of the indicator type, which describes the indicator of compromise (IOC) being shared. More information is provided under indicator_type, which is a malware activity. This indicator is used to signal the presence of the Poison Ivy malware identified by the hash key – an SHA256 key shown in the pattern parameter.

The following code extends the previous one by adding the next STIX object element type – malware :

```
...
        "type": "malware",
        "spec_version": "2.1",
        "id": "malware--fdd60b30-b67c-41e3-b0b9-f01faf20d111",
        "created": "2014-02-20T09:16:08.989Z",
        "modified": "2014-02-20T09:16:08.989Z",
        "name": "Poison Ivy",
        "malware_types": [
            "remote-access-trojan"
        ],
        "is_family": false
    },
```

In the preceding figure, we see the malware type and name. The following example shows the last piece of the STIX object, the relationship object. In the relationship object, we identify the relationship_type parameter, which is indicates. The complete STIX model can be interpreted as follows: the indicator (SHA256 hash value) indicates the presence of remote access trojan malware with the name Poison Ivy. The relationship is applied between the first two STIX objects as follows:

```
{
        "type": "relationship",
        "spec_version": "2.1",
        "id": "relationship--29dcdf68-1b0c-4e16-94ed-bcc7a9572f69",
        "created": "2020-02-29T18:09:12.808Z",
        "modified": "2020-02-29T18:09:12.808Z",
        "relationship_type": "indicates",
        "source_ref": "indicator--a932fcc6-e032-476c-826f-cb970a5a1ade",
