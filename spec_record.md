# Consent Record Specification

This specification describes the JSON schema for the consent record in a manner that follows the JSON-LD and RDF conventions (i.e. using `prefix:term` notations). For compatibility with JSON-LD (and RDF), the relevant contexts or namespaces must be added as described at the end of this document.

## List of Fields

| **27560 term**          | **required?** | **GDPR clauses**                                                                                                                                                                                     | **required?** | **DPV concept**                                                                                                                         | **relation**                                                                                                                                  |
| ----------------------- | ------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ------------- | --------------------------------------------------------------------------------------------------------------------------------------- | --------------------------------------------------------------------------------------------------------------------------------------------- |
| Metadata Fields         |               |                                                                                                                                                                                                      |               |                                                                                                                                         |                                                                                                                                               |
| schema version          | Mandatory     | N/A                                                                                                                                                                                                  | N/A           | None                                                                                                                                    | dct:conformsTo                                                                                                                                |
| record id               | Mandatory     | N/A                                                                                                                                                                                                  | N/A           | None                                                                                                                                    | dct:identifier, dpv:hasIdentifier                                                                                                             |
| receipt id              | Mandatory     | N/A                                                                                                                                                                                                  | N/A           | None                                                                                                                                    | dct:identifier, dpv:hasIdentifier                                                                                                             |
| pii principal id        | Mandatory     | N/A                                                                                                                                                                                                  | N/A           | dpv:DataSubject                                                                                                                         | dpv:hasDataSubject, dct:identifier                                                                                                            |
| Processing fields       |               |                                                                                                                                                                                                      |               |                                                                                                                                         |                                                                                                                                               |
| privacy notice          | Mandatory     | 13, 14 information to be provided                                                                                                                                                                    | Mandatory     | dpv:PrivacyNotice, dpv:ConsentNotice                                                                                                    | dpv:hasNotice                                                                                                                                 |
| language                | Mandatory     | N/A                                                                                                                                                                                                  | N/A           | None                                                                                                                                    | dct:language                                                                                                                                  |
| purpose                 | Mandatory     | 6-1a purpose                                                                                                                                                                                         | Mandatory     | dpv:Purpose                                                                                                                             | dpv:hasPurpose                                                                                                                                |
| purpose type            | Optional      | N/A                                                                                                                                                                                                  | N/A           | dpv:Purpose categories                                                                                                                  | dpv:hasPurpose                                                                                                                                |
| lawful basis            | Optional      | 6 legal basis                                                                                                                                                                                        | Mandatory     | dpv:LegalBasis                                                                                                                          | dpv:hasLegalBasis                                                                                                                             |
| pii information         | Mandatory     | N/A                                                                                                                                                                                                  | N/A           | dpv:PersonalData                                                                                                                        | dpv:hasPersonalData                                                                                                                           |
| pii controllers         | Mandatory     | N/A                                                                                                                                                                                                  | N/A           | dpv:DataController                                                                                                                      | dpv:hasDataController                                                                                                                         |
| collection method       | Optional      | 13-1 collected from data subject; 14-2f source                                                                                                                                                       | Mandatory     | dpv:DataSource, dpv:Collect                                                                                                             | dpv:hasDataSource, dpv:hasProcessing                                                                                                          |
| processing method       | Optional      | 4-2 processing; 30-2b processing categories; 13-2f, 14-2g, 151-h automated decision making, logic, consequences; 35-1b large scale processing of special categories; 35-1c large scale of processing | Mandatory     | dpv:Processing, dpv:AutomationOfProcessing, dpv:Profiling, dpv:AlgorithmicLogic, dpv:Consequence; dpv:DataVolume, dpv:ScaleOfProcessing | dpv:hasProcessing, dpv:hasAutomation, dpv:hasAlgorithmicLogic, dpv:hasConsequence, dpv:hasDataVolume, dpv:hasScale, dpv:hasContext            |
| storage locations       | Mandatory     | 15-2 third country                                                                                                                                                                                   | Mandatory     | dpv:StorageLocation, dpv:Jurisdiction                                                                                                   | dpv:hasStorageCondition, dpv:hasLocation, dpv:hasJurisdiction                                                                                 |
| retention period        | Mandatory     | 13-2a, 14-2a, 15-1d storage period or criteria; 30-1f erasure time limit                                                                                                                             | Mandatory     | dpv:StorageCondition, dpv:StorageDuration, dpv:StorageDeletion                                                                          | dpv:hasStorageCondition, dpv:hasDuration                                                                                                      |
| processing location     | Optional      | 13-1f, 14-1f, 15-2 transfer to third country                                                                                                                                                         | Mandatory     | dpv:Location, dpv:Jurisdiction                                                                                                          | dpv:hasLocation, dpv:hasJurisdiction                                                                                                          |
| geographic restrictions | Optional      | 13-1f, 14-1f, 15-2, 30-1e, 30-2c, 44, 45, 46, 47, 48, 49-1a transfer to third country                                                                                                                | Mandatory     | dpv:Location, dpv:Jurisdiction, dpv:Rule                                                                                                | dpv:hasLocation, dpv:hasJurisdiction, dpv:hasRule                                                                                             |
| service                 | Optional      | N/A                                                                                                                                                                                                  | N/A           | dpv:Service                                                                                                                             | dpv:hasService                                                                                                                                |
| jurisdiction            | Mandatory     | N/A                                                                                                                                                                                                  | N/A           | dpv:Jurisdiction                                                                                                                        | dpv:hasJurisdiction                                                                                                                           |
| recipient third parties | Mandatory     | 4-9 recipient; 4-10 third party                                                                                                                                                                      | Mandatory     | dpv:Recipient, dpv:ThirdParty                                                                                                           | dpv:hasRecipient, dpv:hasThirdParty                                                                                                           |
| withdrawal method       | Mandatory     | 7-3, 13-2c, 14-2d withdrawing consent                                                                                                                                                                | Mandatory     | dpv:WithdrawConsent                                                                                                                     | dpv:hasConsentControl                                                                                                                         |
| privacy rights          | Optional      | 13-22 rights                                                                                                                                                                                         | Mandatory     | dpv:DataSubjectRight, dpv:RightExerciseNotice                                                                                           | dpv:hasRight, dpv:hasNotice                                                                                                                   |
| codes of conduct        | Optional      | 24-3, 32-3, 35-8, 40 codes of conduct; 42 certification                                                                                                                                              | Optional      | dpv:CodeOfConduct, dpv:Certification, dpv:Seal                                                                                          | dpv:hasOrganisationalMeasure                                                                                                                  |
| impact assessment       | Optional      | 35 DPIA                                                                                                                                                                                              | Optional      | dpv:ImpactAssessment, dpv:DPIA                                                                                                          | dpv:hasImpactAssessment                                                                                                                       |
| authority party         | Optional      | 13-2d, 14-2e, 15-1f complaint to authority; 51 supervisory authority; 56 lead authority.                                                                                                             | Mandatory     | dpv:Authority                                                                                                                           | dpv:hasAuthority                                                                                                                              |
| personal data fields    |               |                                                                                                                                                                                                      |               |                                                                                                                                         |                                                                                                                                               |
| pii type                | Mandatory     | 4-1, 14-1d, 15-1b personal data categories                                                                                                                                                           | Mandatory     | dpv:PersonalData                                                                                                                        | dpv:hasPersonalData                                                                                                                           |
| pii attribute id        | Optional      | N/A                                                                                                                                                                                                  | N/A           | dct:identifier                                                                                                                          |                                                                                                                                               |
| pii optional            | Optional      | 13-2e, 35 necessity of personal data                                                                                                                                                                 | Mandatory     | dpv:Necessity                                                                                                                           | dpv:hasNecessity                                                                                                                              |
| sensitive pii category  | Optional      | N/A                                                                                                                                                                                                  | N/A           | dpv:SensitivePersonalData, dpv:SensitivityLevel                                                                                         | dpv:hasPersonalData, dpv:hasSensitivityLevel                                                                                                  |
| special pii category    | Optional      | 9 special categories of data                                                                                                                                                                         | Mandatory     | dpv:SpecialCategoryPersonalData                                                                                                         | dpv:hasPersonalData                                                                                                                           |
| party fields            |               |                                                                                                                                                                                                      |               |                                                                                                                                         |                                                                                                                                               |
| party id                | Mandatory     | N/A                                                                                                                                                                                                  | N/A           | dpv:LegalEntity                                                                                                                         | dct:identifier, dpv:hasIdentifier                                                                                                             |
| party address           | Mandatory     | 13-1a, 14-1a identity of controller                                                                                                                                                                  | Mandatory     | None                                                                                                                                    | dpv:hasAddress                                                                                                                                |
| party email             | Optional      | N/A                                                                                                                                                                                                  | N/A           | None                                                                                                                                    | dpv:hasContact                                                                                                                                |
| party url               | Optional      | N/A                                                                                                                                                                                                  | N/A           | None                                                                                                                                    | dpv:hasContact                                                                                                                                |
| party phone             | Optional      | N/A                                                                                                                                                                                                  | N/A           | None                                                                                                                                    | dpv:hasContact                                                                                                                                |
| party name              | Mandatory     | 13-1a, 14-1a identity of controller                                                                                                                                                                  | Mandatory     | None                                                                                                                                    | dpv:hasName                                                                                                                                   |
| party role              | Optional      | 4-1 data subject; 4-7 controller; 4-8 processor; 4-9 recipient; 4-10 third party; 4-17 representative; 4-21 supervisory authority                                                                    | Mandatory     | dpv:DataSubject, dpv:DataController, dpv:DataProcessor, dpv:Recipient, dpv:ThirdParty, dpv:Representative; dpv:Authority                | dpv:hasDataSubject, dpv:hasDataController, dpv:hasDataProcessor, dpv:hasRecipient, dpv:hasThirdParty, dpv:hasRepresentative; dpv:hasAuthority |
| party contact           | Mandatory     | 13-1a, 14-1a contact details of controller                                                                                                                                                           | Mandatory     | None                                                                                                                                    | dpv:hasContact                                                                                                                                |
| party type              | Mandatory     | 4-9 recipient, data importer and data  exporter (external concepts)                                                                                                                                  | Mandatory     | dpv:Recipient, dpv:DataImporter, dpv:DataExporter, dpv:DataSource                                                                       | dpv:hasRecipient, dpv:hasDataImporter, dpv:hasDataExporter, dpv:hasDataSource                                                                 |
| event fields            |               |                                                                                                                                                                                                      |               |                                                                                                                                         |                                                                                                                                               |
| event time              | Mandatory     | 7-1 demonstrate consent                                                                                                                                                                              | Mandatory     | None                                                                                                                                    | dct:date                                                                                                                                      |
| validity duration       | Mandatory     | N/A (external guidance requires validity)                                                                                                                                                            | Mandatory     | None                                                                                                                                    | dct:valid                                                                                                                                     |
| entity id               | Mandatory     | N/A (assumed roles, e.g. data subject, controller)                                                                                                                                                   | Mandatory     | dpv:LegalEntity                                                                                                                         | dpv:isImplementedByEntity                                                                                                                     |
| event type              | Mandatory     | 4-11 (regular or expressed) consent; 9-2a explicit consent                                                                                                                                           | Mandatory     | dpv:Consent types                                                                                                                       | dpv:hasLegalBasis                                                                                                                             |
| event state             | Mandatory     | 4-11, 9-2a consent given; 7-3 withdrawal of consent; 13, 14 notice for consent; (from derivations) lapsed validity, invalidation authorities, termination by controller                              | Mandatory     | dpv:ConsentStatus                                                                                                                       | dpv:hasConsentStatus                                                                                                                          |

## Structure of Record

The record MUST be a valid JSON object. The _root_ of the record MUST be an object i.e. the record must start and end as:

```json
{
  ... 
}
```

Each record MUST correspond to EXACTLY one consent record i.e. the record must not contain multiple consent records e.g. as a list of records.

## Metadata Fields

### Schema Version

The schema version MUST be present at `root` level with the key `dct:conformsTo`. Its value MUST be EXACLT `dpv-27560:record-2` (in JSON-LD, the prefix `dpv-27560` should correspond to the IRI `https://w3id.org/dpv/schema/dpv-27560#`)

### Record Identifier

The record identifier MUST be present at `root` level with the key `dpv:hasIdentifier`. Its value MUST be a single **String**

```json
{
    "dpv:hasIdentifier": "UUID4"
}
```

### PII Principal / Data Subject Identifier

The PII Principal / Data Subject identifier MUST be present at `root` level with the key `dpv:hasDataSubject`. Its value MUST be a single **Object** which MUST contain the key `dpv:hasIdentifier` and a **String** value.

```json
{
    "dpv:hasDataSubject": {
        "dpv:hasIdentifier": "XYZ"
    }
}
```

The Data Subject object MAY optionally contain the key `@type` with the value an **Array** whose items include `dpv:DataSubject` to assert that the that object is a Data Subject

The Data Subject object MAY optionally contain other key-value pairs, including identifiers, e.g. to denote emails, names, timestamps, or other relevant information as necessary for the record

### Creation Date/Timestamp

The creation date MUST be present at `root` level using the key `dct:created` and the value a **String** with a timestamp expressed in ISO 639 format.

```json
{
    "dct:created": "2024-01-14T00:25:11.840Z"
}
```

### Creator

The creator of the record MUST be present at `root` level using the key `dct:creator` and the value a **String** with a value that matches one of the keys in the list present as the value of `dpv:hasEntity`

```json
{
    "dct:creator": "company:Alpha",
    "dpv:hasEntity": {
        "company:Alpha": {
            ...
        }
    }
}
```

### Language

The language used in the strings within the record MUST be indicated at `root` level using the key `dct:language` and and the value a **String** of exactly 2 characters based on language codes as defined in ISO 639

```json
{
    "dct:language": "EN"
}
```

## Processing Fields

There MUST be EXACTLY one Process indicated at root level using the key `dpv:hasProcess` whose value is an **Array**. Each item in the array must contain the following fields, unless there is a key `dpv:hasProcess` within the object - in which case the fields absent from the 'outer' or 'parent' process MUST be present in the 'inner' or 'child' process. 

The interpretation of nested process is that the 'inner' process inherits the fields from 'outer' processes. Nested processes are useful to describe common elements in the 'outer' process without repeating them alongside the differing elements within the 'inner' process. For example, if the consent record is to reflect that the data subject has given consent for collecting data for purpose X to the data controller but not for sharing that data with a third party recipient for the same purpose, then the outer process will contain the fields for the data and purpose, while two inner processes will be first indicate that data collection by data controller has the state of 'consent given', and second to indicate that sharing data to third party recipient has the state of 'consent refused'.

### Purpose

There MUST be a key `dpv:hasPurpose` within the Process object whose value is an **Array**. Each item in the array must either be a **String** or an object that MUST have the key `@type` whose value is a list that MUST contain the item `dpv:Purpose`.

```json
{
    "dpv:hasProcess": {
        "dpv:hasPurpose": [
            "dpv:Marketing",
            {
                "@type": ["dpv:Purpose"],
                "skos:broader": "dpv:FraudPreventionDetection",
                "skos:prefLabel": "Preventing Fraud in Emails"
            }
        ]
    }
}
```

### Personal Data

There MUST be a key `dpv:hasPersonalData` within the Process object whose value is an **Array**. Each item in the array must either be a **String** or an object that MUST have the key `@type` whose value is a list that MUST contain the item `dpv:PersonalData` or `dpv:SensitivePersonalData` or `dpv:SpecialCategoryPersonalData`.

```json
{
    "dpv:hasProcess": {
        "dpv:hasPersonalData": [
            "pd:Email",
            {
                "@type": ["dpv:SpecialCategoryPersonalData"],
                "skos:broader": ["pd:MedicalHealth"],
                "skos:prefLabel": "Medical Records"
            }
        ]
    }
}
```

### Processing

There MUST be a key `dpv:hasProcessing` within the Process object whose value is an **Array**. Each item in the array must either be a **String** or an object that MUST have the key `@type` whose value is a list that MUST contain the item `dpv:Processing`.

```json
{
    "dpv:hasProcess": {
        "dpv:hasProcessing": [
            "dpv:Collect",
            {
                "@type": ["dpv:Processing"],
                "skos:broader": ["dpv:Store", "dpv:Transform"],
                "skos:prefLabel": "Store data in a structured manner"
            }
        ]
    }
}
```

### Data Controller

There MUST be a key `dpv:hasDataController` within the Process object whose value is an **Array**. Each item in the array must be a **String** that matches the key in the object that is the value of `dpv:hasEntity` at `root` level.

```json
{
    "dpv:hasProcess": {
        "dpv:hasDataController": ["company:SignatuAS"]
    },
    "dpv:hasEntity": {
        "company:SignatuAS": {
            "dpv:hasName": "Signatu AS"
        }
    }
}
```

### Data Source

There MUST be a key `dpv:hasDataSource` within the Process object whose value is an **Array**. Each item in the array must either be a **String** or an object that MUST have the key `@type` whose value is a list that MUST contain at least one of these strings: `dpv:DataSubjectDataSource`, `dpv:DataControllerDataSource`, `dpv:ThirdPartySource` and MAY contain `dpv:PublicDataSource` or `dpv:NonPublicDataSource`.

```json
{
    "dpv:hasProcess": {
        "dpv:hasDataSubject": [
            "dpv:DataControllerDataSource",
            {
                "@type": ["dpv:DataSubjectDataSource", "dpv:PublicDataSource"],
                "skos:prefLabel": "Data publicly published by subject on website X"
            }
        ]
    }
}
```

### Storage Condition

There MUST be a key `dpv:hasStorageCondition` within the Process object whose value is an **Array**. Each item in the array must either be an object. In the array, there MUST be at least one object which has the key `@type` whose value is a list and which MUST contain an object with key `@type` with a value of type list containing the strings `dpv:StorageLocation` and `dpv:StorageDuration`. These MAY be present within the same object or be present in different objects. 

For the object whose type includes `dpv:StorageLocation`, the object MUST contain the key `dpv:hasLocation`. 

For the object whose type includes `dpv:StorageDuration`, the object MUST contain the key `@type` whose value is a **String** and MUST be one of these: `dpv:TemporalDuration`, `dpv:UntilTimeDuration`, `dpv:FixedOccurencesDuration`, `dpv:UntilEventDuration`. And the object MUST contain the key `rdf:value` whose value is a string containing the value describing the relevant storage duration - which must use ISO 639 for describing temporal durations and timestamps, positive non-zero integers for fixed occurences, or a string description of the event.

```json
{
    "dpv:hasProcess": {
        "dpv:hasStorageCondition": [
            {
                "@type": ["dpv:StorageLocation"],
                "dpv:hasLocation": "loc:NO-03"
            },
            {
                "@type": ["dpv:StorageDuration"],
                "dpv:hasDuration": {
                    "@type": "dpv:TemporalDuration",
                    "rdf:value": "P6M"
                }
            }
        ]
    }
}
```

### Geographic Restriction

### Recipients

### Legal Basis

### Jurisdiction and Laws

### Rights

### Processing Condition

### Consent Change & Withdrawal

### Codes of Conduct

### Impact Assessment

## Consent Fields

### Indicating Consent State within Process

Within the Process object, there MUST be a key `dpv:hasConsentStatus` unless there is a key `dpv:hasProcess` present i.e. a Process must have a consent state associated with it unless it has further nested (sub-) processes defined within it - in which case the nested processes will have the consent state associated within it.

When interpreting the consent state for processes, the interpretation is as follows: Each process 'inherits' the information fields from its 'parent' process i.e. if an 'outer' process has a Purpose field

### Consent States

### Consent Notice

> **TODO** put this inside consent status so each consent state is associated with a notice where relevant

. The Notice associated with consent MUST be present at `root` level with the key `dpv:hasNotice` with an **Array** or list where each item is an **Object** which contains the following information:

. The Notice object MUST contain an identifier field with the key `dpv:hasIdentifier` whose value is a **String**.

. The Notice object MAY optionally contain the key `dct:date` with a value of type **String** which denotes the date of notice provision using ISO 8601

. The Notice object MAY optionally contain the key `dct:coverage` with a value of type **String** which denotes the duration or period of notice validity using ISO 8601

. The Notice object MAY optionally contain the key `@type` with a value `dpv:ConsentNotice` 

### Privacy Notice

## Entities Fields

### Entity Name

### Entity Role

### Entity Address

### Entity Contacts

### Entity Website
