# Consent Record Specification

This specification provides a JSON and JSON-LD specification for the implementation of consent records as per the ISO/IEC TS 27560:2023 technical specification and the Implementation Guide using DPV.

The goal is to have compatibility between the JSON and JSON-LD records without disruption to the adopter or the requirements for tooling. With this in mind, the JSON implementation of the consent record is mostly similar to the JSON-LD implementation i.e. it uses the same keys and notations using `prefix:term` notations where `prefix` is an IRI namespace and `term` is a well-defined concept as stated by DPV.  The valid JSON-LD iteration of a valid JSON consent record should ideally only require the additional contexts and namespaces declared in order to parse it as valid RDF data.

The structure of the specification is as follows:

1. First, the specification lists the fields which are to be recorded in the consent record as defined in ISO/IEC TS 27560:2023, along with their relevance in implementing GDPR, and the relevant DPV concepts that are used to express information within the record.
2. Second, each field is expressed in terms of its information contents, JSON structure within the record, and how to consistently and unambigiously interpret the field.
3. Finally, the JSON-LD requirements are specified as additional information and constraints over a valid JSON record.

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
   
}
```

Each record MUST correspond to EXACTLY one consent record i.e. the record must not contain multiple consent records e.g. as a list of records. If such a list or collection of records must be communicated, then the nature of such lists is outside the scope of this specification.

## Metadata Fields

### Schema Version

The schema version MUST be present at `root` level with the key `dct:conformsTo`. Its value MUST be EXACLT `dpv-27560:record-2` (in JSON-LD, the prefix `dpv-27560` should correspond to the IRI `https://w3id.org/dpv/schema/dpv-27560#`). 

This field is used to indicate which schema should be used to interpret the rest of the data in the consent record. To create custom schemas, a separate namespace must be used i.e. only DPVCG defined schemas should start with the indicated prefix and namespace IRI.

### Record Identifier

The record identifier MUST be present at `root` level with the key `dpv:hasIdentifier`. Its value MUST be a single **String**. Recommended value is a valid UUID-4 generated unique identifier.

This field is used to create an unique reference and identifier for the record. The entity that creates the consent record is responsible for generating this identifier. Multiple identifiers can exist for the same record e.g. internal identifiers used by an entity. Such identifiers MUST NOT be indicated using the same key, but must use another separate key e.g. `dct:identifier` or similar. The intention of this is to enable a simple way to depict and interpret the unique identifier associated with a record. 

```json
{
    "dpv:hasIdentifier": "UUID4"
}
```

### PII Principal / Data Subject Identifier

The PII Principal / Data Subject identifier MUST be present at `root` level with the key `dpv:hasDataSubject`. Its value MUST be a single **Object** which MUST contain the key `dpv:hasIdentifier` and a **String** value.

The Data Subject associated with the consent record is the individual whose personal data is being referenced for processing within the record. A consent record MUST NOT be associated with more than one data subject. For multiple data subjects, separate consent records must be generated with their own unique identifiers.

```json
{
    "dpv:hasDataSubject": {
        "dpv:hasIdentifier": "XYZ"
    }
}
```

The Data Subject object MAY optionally contain the key `@type` with the value an **Array** whose items include `dpv:DataSubject` to assert that the that object is a Data Subject.

The Data Subject object MAY optionally contain other key-value pairs, including identifiers, e.g. to denote emails, names, timestamps, or other relevant information as necessary for the record. 

### Creation Date/Timestamp

The creation date MUST be present at `root` level using the key `dct:created` and the value a **String** with a timestamp expressed in ISO 639 format. 

There MUST NOT be more than one creation date. Further modifications to the record MAY be depicted using `dct:modified` and a timestamp string or a list of strings in ISO 639 format. 

Alternatively, modifications to the record can be represented as separate records, and the relationship between the _previous_ and _current_ record should be indicated using `dpv:isBefore` and `dpv:isAfter` as keys with the value as the identifier for the relevant record.

```json
{
    "dct:created": "2024-01-14T00:25:11.840Z"
}
```

### Creator

The creator of the record MUST be present at `root` level using the key `dct:creator` and the value a **String** with a value that matches one of the keys in the list present as the value of `dpv:hasEntity`.

There MUST NOT be more than one creator. Where multiple entities are deemed responsible for the information within the consent reocrd, ONLY the entity responsible for actually generating the record - whether it be the controller or processor or have another role - should be mentioned as the creator.

```json
{
    "dct:creator": "company:Alpha",
    "dpv:hasEntity": {
        "company:Alpha": {
            "data": "data"
        }
    }
}
```

### Language

The language used in the strings within the record MUST be indicated at `root` level using the key `dct:language` and and the value a **String** of exactly 2 characters based on language codes as defined in ISO 639. Note that the language code MUST contain only small case letters i.e. `en` and not `EN`.

The language field indicates the language used within strings for textual descriptions, and should match the language the notice used for consent.

```json
{
    "dct:language": "en"
}
```

## Processing Fields

There MUST be EXACTLY one `dpv:hasProcess` key present at root level whose value is an **Array** of **Objects** represent processes. Each Process Object MUST contain all mandatory fields as defined in the _Processing Fields_ section of this specification - EXCEPT when the object has the key `dpv:hasProcess` which represents a nested process. 

The interpretation of nested process is that the 'inner' process inherits the fields from 'outer' processes. Nested processes are useful to describe common elements in the 'outer' process without repeating them alongside the differing elements within the 'inner' process. For example, if the consent record is to reflect that the data subject has given consent for collecting data for purpose X to the data controller but not for sharing that data with a third party recipient for the same purpose, then the outer process will contain the fields for the data and purpose, while two inner processes will be first indicate that data collection by data controller has the state of 'consent given', and second to indicate that sharing data to third party recipient has the state of 'consent refused'.

Alternatives to this approach are to create two separate processes which contain all the mandatory fields and only differ in the recipients and whether the consent was given or refused. While such an approach would offer the most explicit and unambiguous representation of information, it quickly leads to the consent record being 'bloated' and 'repetitive' when used for practical scenarios. A better way to manage the consent is to think of the consent records as capturing the 'entire consent interaction' rather than representing each distinct 'consent' as a separate process within the record. This means when an user on a website is given choices to manage consent for several different scenarios, all of those choices are reflected in the same consent record instead of being spread across different records.

Where the individual has not given their consent i.e. they have refused to give consent either through inaction (e.g. not ticking a box) or through an action (e.g. clicking on a button) - this interaction is necessary to be recorded to ensure that the refusal is registered. This means that if the individual chooses 4 out of 5 options to give consent to, a record or a process within the record should not contain only the 4 options with a consent given status, but also MUST contain the option where consent was refused. To represent this distinctly and unambiguously, nested processes are useful and convenient as compared to the alternatives.

To ensure there is no ambiguity in the interpretation of processes and the consent statuses within them, the following rules MUST be followed:

1. If a Process does NOT have a nested process, then it MUST have a Consent Status `dpv:hasProcess`, then it MUST have `dpv:ConsentStatus`.
2. A Process MUST NOT have a Consent Status if there is a nested process within it i.e. if `dpv:hasProcess` is present, then `dpv:hasConsentStatus` MUST NOT be present.
3. As a corollary of the above two, there MUST NOT be a Consent Status within a Process and also within its nested Process i.e. a Process must NOT have both these paths: `dpv:hasConsentStatus` and `dpv:hasProcess -> dpv:hasConsentStatus` as this would lead to an ambiguous conflict between the 'outer' and 'inner' consent states.

### Purpose

There MUST be a key `dpv:hasPurpose` within the Process object whose value is an **Array** where each item in the array MUST be an **Object** that: 

1. MUST have the key `skos:broader` whose value is a **String** or an array of **String** where the string represents a concept from the DPV's Purpose taxonomy.
2. MUST have the key `skos:prefLabel` whose value is a **String** that provides a short description of label for the purpose
3. MAY have the key `skos:definition` whose value is a **String** that provides a definition or detailed description of the purpose
4. MAY have the key `@type` whose value is an **Array** of **String** which MUST contain `dpv:Purpose`

The purpose object represents a single instance of purpose regarding why the personal data needs to be processed. It should provide a sufficiently high-level description that is not technical or legal jargon and is comprehensible to the data subject - as is required within the notice for informed consent. The `skos:broader` value(s) indicate the 'categories' the purpose falls within, and provide assistance in its interpretation. The `skos:prefLabel` and `skos:definition` provide the human-readable descriptions of the purpose.

The purpose object MUST NOT contain other fields defined in the specification - such as Personal Data, Processing, Data Controller, or Recipient. This is to ensure consistent expression of purposes, and since the Process object combines these different fields together.

The interpretation of Purpose acts as the justification or end-goal for why the data is being processed. Therefore, if multiple purposes are presented within the same process - this MUST be interpreted as meaning ALL purposes are necessary or required for that process to function. 

If consent is or can be given separately to one or more purposes, then each purpose should be separately expressed in its individual process rather than combining all purposes within the same process - even if the consent state is the same for all of them. This is to reduce the unambiguity associated with combining all purposes within the same process as the individual can later decide to revoke or withdraw their consent for some of them. If each revocable purpose is expressed in its own process, then each such process will have its own consent state and thus be unambiguously distinct from other consent states associated with other purposes.

Where multiple purposes are present and contain common elements - such as personal data or processing operations, these can be expressed as a common 'outer' process with the specific purposes and its relevant fields expressed in 'inner' nested processes to reduce the duplication of information.

```json
{
    "dpv:hasProcess": {
        "dpv:hasPurpose": [
            {
                "@type": ["dpv:Purpose"],
                "skos:broader": "dpv:FraudPreventionDetection",
                "skos:prefLabel": "Preventing Fraudulent News in Emails",
            },
            {
                "@type": ["dpv:Purpose"],
                "skos:broader": "dpv:ServiceProvision",
                "skos:prefLabel": "Send news alerts as requested"
            }
        ]
    }
}
```

### Personal Data

There MUST be a key `dpv:hasPersonalData` within the Process object whose value is an **Array** where each item in the array MUST either be a **String** or an **Object** that: 

1. MUST have the key `@type` whose value is a list that MUST contain the item `dpv:PersonalData` or `dpv:SensitivePersonalData` or `dpv:SpecialCategoryPersonalData`
2. MUST have the key `skos:broader` whose value is a **String** or an **Array** of **String** where the string represents a concept from the DPV's Personal Data extension.
2. MAY have the key `skos:prefLabel` whose value is a **String** that provides a short description of label for the personal data
3. MAY have the key `rdf:value` whose value is a **String** that represents the exact value or data (e.g. the actual email being used)

The personal data field represents the personal data being processed in context of the consent record. The field also enables expressing whether the data is considered sensitive or special category (GDPR) - either by using the DPV's Personal Data Taxonomy where special categories are already defined, or by explicitly indicating it within the object's type. The use of `skos:prefLabel` provides a human-readable description of the data, and the use of `rdf:value` enables recording the actual data involved.

The personal data field MUST NOT contain other fields present within the processing fields section - such as data source or purpose or recipient.

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

### Processing Operations

There MUST be a key `dpv:hasProcessing` within the Process object whose value is an **Array** where each item in the array must either be a **String** or an **Object** that:

1. MUST have the key `skos:broader` whose value is an **Array** of **String** where each string represents a concept from DPV's Processing Taxonomy.
2. MAY have the key `@type` whose value is an **Array** of **String** that MUST contain `dpv:Processing`.
3. MAY contain `skos:prefLabel` whose value is a **String**.

The processing operations field (not to be confused with the _Process_ object or the _Processing Fields_ section) refers to the 'operations' performed on the personal data. Common values used for such operations are: `dpv:Collect`, `dpv:Store`, `dpv:Use`, `dpv:Share`, `dpv:Delete` - though the DPV's taxonomy offers a rich collection of explicit concepts that are aligned to these and also enable representing operations such as `dpv:Transfer`, `dpv:Combine`, `dpv:Profiling`, and `dpv:Anonymise` which have legally relevant implications.

The Processing operations field MUST NOT contain other keys present in the Processing Fields - such as Recipient, Personal Data, or Purpose.

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

There MUST be a key `dpv:hasDataController` within the Process object whose value is an **Array** where each item in the array must be a **String** that matches the key in the object that is the value of `dpv:hasEntity` at `root` level. That is, the data controller should be an entity defined in the record as the key in `dpv:hasEntity` object.

Where multiple data controllers are defined for the same process, it is to be interpreted that they play a Joint Data Controller role. 


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

There MUST be a key `dpv:hasDataSource` within the Process object whose value is an **Array** where each item in the array must either be a **String** or an **Object** that MUST have the key `@type` whose value is a list that MUST contain at least one of these strings: `dpv:DataSubjectDataSource`, `dpv:DataControllerDataSource`, `dpv:ThirdPartySource` and MAY contain `dpv:PublicDataSource` or `dpv:NonPublicDataSource`.

The data source indicates where the data being processed will be sourced from. This field is necessary even when data is not being collected e.g. data already collected and stored is being used. In such cases, the data source would probably be the Data Controller who is storing the data.

The data source field can optionally or additionally indicate whether the data was publicly available - in which case the entity that published the data should also be indicated e.g. the data subject.

If there are multiple data controllers or third parties associated with the consent record, then the data source field can utilise the key `dpv:hasEntity` to disambiguate which specific entity was responsible for the sourcing of data.

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

There MUST be a key `dpv:hasStorageCondition` within the Process object whose value is an **Array** where each item in the array MUST be an **Object**. At least one such object MUST have the key `@type` whose value is an **Array** and which contains `dpv:StorageLocation` and `dpv:StorageDuration`. Both these values MAY be present within the same object or be present in different objects. The object type MAY also have values `dpv:StorageDeletion` and `dpv:StorageRestoration`.

For the object whose type includes `dpv:StorageLocation`, the object MUST contain the key `dpv:hasLocation` whose value is a **String** or an **Object** with key `skos:broader` whose value is a **String** - where the strings represent a concept from the DPV's Location taxonomy based on ISO 3166 location codes.

For the object whose type includes `dpv:StorageDuration` or `dpv:StorageDeletion` or `dpv:StorageRestoration`, the object MUST contain the key `@type` whose value is a **String** and MUST be one of these: `dpv:TemporalDuration`, `dpv:UntilTimeDuration`, `dpv:FixedOccurencesDuration`, `dpv:UntilEventDuration`. And the object MUST contain the key `rdf:value` whose value is a string containing the value describing the relevant duration - which must use ISO 639 for describing temporal durations and timestamps, positive non-zero integers for fixed occurrences, or a string description of the event. If the storage location does not correspond to a geo-physical specific location (e.g. on device), then the relevant `dpv:LocationLocality` taxonomy should be used as the value of `rdf:value` e.g. `dpv:WithinDevice`.

The storage condition field enables representing information associated with restrictions or limitations on the storage period or location, and optionally the period after which data will be deleted or the duration for restoring the data (e.g. from backups). If storage duration is defined, then its duration should be interpreted as the period of time, or a specific date, or a specific event, or the number of occurrences - after which data will be marked for deletion. If the data is NOT immediately deleted after the storage duration, then the storage deletion field MUST be used to indicate the temporal period or condition determining the actual deletion.

For storage locations, the values indicate all the locations where the data will be stored - either as multiple copies or in parts. 

The storage condition fields MUST NOT contain fields or information such as whether the data is encrypted (processing operation field), who receives the stored data (recipient field), or what data is being stored (personal data field).

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

### Processing Conditions

There MAY be a key `dpv:hasProcessingCondition` within the Process object whose value is an **Array** where each item in the array MUST be an **Object**. The objects MUST have a key `@type` whose value is an **Array** and which contains one or more of: `dpv:ProcessingLocation` or `dpv:ProcessingDuration`. Both these values MAY be present within the same object or be present in different objects.

For the object whose type includes `dpv:ProcessingLocation`, the object MUST contain the key `dpv:hasLocation` whose value is a **String** or an **Object** with key `skos:broader` whose value is a **String** - where the strings represent a concept from the DPV's Location taxonomy based on ISO 3166 location codes.

For the object whose type includes `dpv:ProcessingDuration` or `dpv:StorageDeletion` or `dpv:StorageRestoration`, the object MUST contain the key `@type` whose value is a **String** and MUST be one of these: `dpv:TemporalDuration`, `dpv:UntilTimeDuration`, `dpv:FixedOccurencesDuration`, `dpv:UntilEventDuration`. And the object MUST contain the key `rdf:value` whose value is a string containing the value describing the relevant duration - which must use ISO 639 for describing temporal durations and timestamps, positive non-zero integers for fixed occurrences, or a string description of the event. If the storage location does not correspond to a geo-physical specific location (e.g. on device), then the relevant `dpv:LocationLocality` taxonomy should be used as the value of `rdf:value` e.g. `dpv:WithinDevice`.

The processing condition field enables representing information associated with restrictions or limitations on the processing period or location. Even though storing data is considered a form of processing, this field MUST NOT be used to indicate storage conditions. This field is ideal for indicating restrictions on geo-physical transfers of data e.g. outside jurisdictions.

For processing locations, the values indicate all the locations where the data will be processed - either as whole or in parts. 

The processing condition fields MUST NOT contain fields or information such as whether the data is encrypted (processing operation field), who receives the processed data (recipient field), or what data is being processed (personal data field).

```json
{
    "dpv:hasProcess": {
        "dpv:hasProcessingCondition": [
            {
                "@type": ["dpv:ProcessingLocation"],
                "dpv:hasLocation": "loc:NO-03"
            },
            {
                "@type": ["dpv:ProcessingDuration"],
                "dpv:hasDuration": {
                    "@type": "dpv:TemporalDuration",
                    "rdf:value": "P6M"
                }
            }
        ]
    }
}
```

### Recipients

There MUST be a key `dpv:hasRecipient` within the Process object whose value is an **Array** of **String** where each string corresponds to a key present in the value of `dpv:hasEntity` at the `root` level. That is, each recipient must be a declared entity within the consent record.

If there are no recipients, then the value of this field must be an empty list.

If the Data Controller is also a recipient of the personal data, it MUST be listed within the recipients array. An absence of the Data Controller or any entity in the recipients array MUST be interpreted as that entity is NOT receiving personal data.

If recipients are not known ahead of time, but it is intended to send data to recipients based on their role, then a category describing the role of the recipient MUST be used instead of the identity/identifier of the entity. Such groups are referenced using an identifier which MUST be a key within the `dpv:hasEntity` object at `root` level. The group object MUST contain the key `rdfs:subClassOf` to denote that this is a grouping or category and not a specific instance of an entity. It MUST contain a field `skos:prefLabel` which provides a label describing the entity group. It MAY contain a field `skos:definition` which provides a short description or definition of the group of entities and the role they will play within the processing.

If only some recipients are known, then they MUST be listed within the recipients array in addition to the group identifier as described above. To indicate that en entity belongs to the group, the entity object within the `dpv:hasEntity` object at `root` level MUST contain include the identifier to the group (i.e. they key used to reference that group within the `dpv:hasEntity` object) within the array value of `@type` key.

```json
{
    "dpv:hasProcess": {
        "dpv:hasRecipient": [
            "company:Alpha",
            "company:Beta",
            "group:PostProvider"
        ]
    },
    "dpv:hasEntity": {
        "company:Alpha": {
            "@type": ["dpv:Organisation"],
            "dpv:hasName": "Alpha Inc."
        },
        "company:Beta": {
            "@type": ["dpv:Organisation", "group:PostProvider"],
            "dpv:hasName": "Beta AG"
        },
        "group:PostProvider": {
            "@type": ["dpv:Entity"],
            "rdfs:subClassOf": ["dpv:ServiceProvider"],
            "skos:prefLabel": "Postal Service Provider",
            "skos:definition": "A company that will be contracted to deliver post"
        }
    }
}
```

```json
{
    "dpv:hasProcess": {
        "dpv:hasRecipient": []            
    }
}
```

### Legal Basis

Within the Process object, there MUST be a key `dpv:hasLegalBasis` whose value is an **Array** of **String** or **Object** values. If the value is of type String, then it MUST be a legal basis defined within the DPV legal basis taxonomy or a legal basis defined in an extension (e.g. EU-GDPR). If the value is of type Object, then there MAY be a key `@type` whose value is `dpv:LegalBasis`, and there MUST be a key `skos:broader` whose value is an Array, and at least one of the values in that array MUST be a legal basis defined within the DPV legal basis taxonomy or a legal basis defined in an extension (e.g. EU-GDPR). In the object, there key `skos:prefLabel` MAY be present whose value is a string and provides a human readable title for the legal basis. There also MAY be a key `skos:definition` which provides a human readable description of the legal basis. Depending on the legal basis, there may be additional

```json
{
    "dpv:hasLegalBasis": ["dpv:Consent"]
}
```
```json
{
    "dpv:hasLegalBasis": ["dpv:Consent", "eu-gdpr:A-6-1a-non-explicit-consent"]
}
```

### Jurisdiction and Laws

### Rights

### Consent Change & Withdrawal

### Codes of Conduct

### Impact Assessment

## Consent Fields

### Indicating Consent State within Process

Within the Process object, there MUST be a key `dpv:hasConsentStatus` unless there is a key `dpv:hasProcess` present i.e. a Process must have a consent state associated with it unless it has further nested (sub-) processes defined within it - in which case the nested processes will have the consent state associated within it.

When interpreting the consent state for processes, the interpretation is as follows: Each process 'inherits' the information fields from its 'parent' process i.e. if an 'outer' process has a Purpose field, then that purpose is also applied to all inner processes present within the outer process. The inverse of this is not true.

The value of the key `dpv:hasConsentStatus` MUST be an **Array** whose values are an **Object** with the following:

1. There MUST be a key `@type` whose value is an **Array** of **String**, where at least one string MUST be a valid consent status from the DPV taxonomies. 
2. There MAY be a consent type (e.g. Explicitly Expressed Consent) also present in the values array of `@type`. If such a field is NOT present, then the interpretation is that the type of action expressed by the consent status (e.g. consent given) is according to the type of consent expressed in the legal basis (e.g. if consent status only states that it is given, and expressed consent is indicated as the legal basis, then the given consent is implied to be of type expressed consent).
3. There MUST be a key `dpv:isIndicatedAtTime` whose value is a ISO 8601 timestamp and which indicates when the associated action was performed. E.g. the timestamp indicates when consent was given or withdrawn.
4. There MUST be a key `dpv:isExercisedAt` whose value provides sufficient information (e.g. as a string) for where/how the action was performed (e.g. settings in dashboard, or consent dialogue on website)
5. There MAY be a key `dpv:isIndicatedBy` whose value is a **String** which MUST be a key within the object value of `dpv:hasEntity` key at `root` level.
6. There MAY be a key `dpv:hasLocation` which indicates the geo-physical or virtual location the action was performed at, e.g. the URL of a website.
7. There MAY be a field `dpv:hasDuration` whose value MUST be an **Object** which: contains a key `@type` whose value is an **Array** of strings where at least one of the following MUST be present: `dpv:TemporalDuration`, `dpv:UntilTimeDuration`, `dpv:TemporalDuration`, `dpv:UntilEventDuration`, `dpv:FixedOccurencesDuration`, or `dpv:EndlessDuration`; the object MUST contain a key `rdf:value` which provides the relevant information e.g. a timestamp or the period. For `dpv:EndlessDuration`, this key is NOT required.


```json
{
    "dpv:hasProcess": [{
        "@type": "dpv:Process",
        "dpv:hasConsentStatus": [
            {
              "@type": ["dpv:ConsentGiven", "dpv:ExpressedConsent"],
              "dpv:isIndicatedAtTime": "2023-11-05T16:08:50",
              "dpv:hasDuration": {
                "@type": ["dpv:TemporalDuration"],
                "rdf:value": "P6M"
              },
              "dpv:hasNotice": {
                "@type": ["dpv:ConsentNotice"],
                "dpv:hasIdentifier": "UUID-4",
                "dpv:hasLocation": "http://example.com/"
              },
              "dpv:isExercisedAt": "https://example.com/manage-consent"
            }
        ]
    }]
}
```

### Consent Notice

The Notice associated with consent MUST be present in the following form:

1. at `root` level with the key `dpv:hasNotice` with an **Array** or list where each item is an **Object** which contains the information described later in this section; OR 
2. 

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
