# Decision Process

## Summary

A new concept `dpv:DecisionProcess` is proposed to distinguish which (nested or otherwise) processes represent decision points from those that are logical groupings of information and do not represent processes where e.g. the user can give/refuse consent. 

The rule for interpreting processes becomes thus:

1. Only `DecisionProcess` instances should have consent statuses and events.
2. `DecisionProcess` instances inherit all fields and attributes from their parent process as well as any child/sub/nested process within the parent process which is NOT a decision process.
3. The `DecisionProcess` instances are useful for UI/UX tools to understand where to offer controls for the individual to make decisions.
4. A `DecisionProcess` MUST NOT contain another `DecisionProcess` either directly or indirectly (i.e. a subprocess which contains decision process)

## Rationale

A `Process` is a logical collection or grouping of concepts to indicate they function together within a business activity. Each process can be composed of further _sub-processes_ or _nested processes_ to clarify how specific operations take place. For example, to distinguish specific categories of data going to different recipients, or to indicate that different categories of data have separate storage locations and durations. To indicate which part of the process the individual has _control_ over i.e. where they can make decisions, the concept `DecisionProcess` is used. 

That is, a `DecisionProcess` is a `Process` where the individual (or another entity) can make a decision e.g. to give or withdraw consent. Therefore, only `DecisionProcess` instances should have a consent status associated with them as they represent decision points where the invidual has been offered a choice or has indicated a preference which is recorded as a decision using `ConsentStatus` e.g. `ConsentGiven` or `ConsentRefused`.

When a machine-agent or tool uses a consent record or a consent receipt, the `DecisionProcess` provide a hint for creating the necessary UI/UX and controls for the individual to make or change their choices. For example, each `DecisionProcess` instance translates to controls that the user can then use to make decisions - such as to give or withdraw their consent. 

`DecisionProcesses` also assist in separating processes which exist for logical clarity from those that represent decision points and will thus have or require a consent status. For consent records, a nested processes 'inherits' all attributes and fields from its parent process. This allows for an efficient representation of the record where 'common information' such as controller or personal data can be declared in the outer or parent process, and fields for which separate consent decisions can be made such as recipients or purposes can be specified individually in nested processes.

## Example

Consider the example where name and email are being stored for different periods of time - but for which the individual does not have a choice for individually accepting or refusing such storage. Simultaneously, the data is being shared with two recipients where the individual can choose whether to share the data with each individual recipient. If we model this information with only processes - as the example below shows - there is an ambiguity in the record as well as for the agent using the record due to the way processes are structured.

Nested processes inherit the attributes of the parent process - however in this case, the personal data and storage location are NOT present in the parent process, but are instead in a nested _sibling process_. The altnerative to this is to explicitly add the data and storage location to each process with the recipient - which would increase the size of the record as well as reduce the convenience of using it.

```json
{
    "dpv:hasProcess": [{
        // Other common info goes here ...
        "dpv:hasProcess": [
            { // this is NOT being inherited for the recipient process
                "@type": ["@dpv:Process"],
                "dpv:hasPersonalData": ["pd:Name"],
                "dpv:hasStorageCondition": [{
                    "@type": ["dpv:StorageLocation"],
                    "dpv:hasLocation": ["loc:IE"]
                }]
            },
            { // this is NOT being inherited for the recipient process
                "@type": ["@dpv:Process"],
                "dpv:hasPersonalData": ["pd:Email"],
                "dpv:hasStorageCondition": [{
                    "@type": ["dpv:StorageLocation"],
                    "dpv:hasLocation": ["loc:CH"]
                }]
            },
            { // this is where the individual can make a choice
                "@type": ["@dpv:Process"],
                "dpv:hasRecipient": ["company:Alpha"],
                "dpv:hasConsentStatus": [{
                    "@type": ["dpv:ConsentGiven"]
                }]
            },
            { // this is where the individual can make a choice
                "@type": ["@dpv:Process"],
                "dpv:hasRecipient": ["company:Beta"],
                "dpv:hasConsentStatus": [{
                    "@type": ["dpv:ConsentRefused"]
                }]
            }
        ]
    }]
}
```

To distinguish between which information should be inherited and which should not be, we create the new interpretation rule for processes as: "A nested process inherits all attributes and fields from its parent process and any sub- or nested process within the parent process which is NOT a decision process." This means that the name and storage location processes are not decision processes, and therefore they are inherited by the decision processes for recipients.

```json
{
    "dpv:hasProcess": [{
        // Other common info goes here ...
        "dpv:hasProcess": [
            { // NOT a decision process means 
              // this is part of the parent process
                "@type": ["@dpv:Process"],
                "dpv:hasPersonalData": ["pd:Name"],
                "dpv:hasStorageCondition": [{
                    "@type": ["dpv:StorageLocation"],
                    "dpv:hasLocation": ["loc:IE"]
                }]
            },
            { // NOT a decision process means 
              // this is part of the parent process
                "@type": ["@dpv:Process"],
                "dpv:hasPersonalData": ["pd:Email"],
                "dpv:hasStorageCondition": [{
                    "@type": ["dpv:StorageLocation"],
                    "dpv:hasLocation": ["loc:CH"]
                }]
            },
            { // Decision process - inherits data and storage location
                "@type": ["@dpv:DecisionProcess"],
                "dpv:hasRecipient": ["company:Alpha"],
                "dpv:hasConsentStatus": [{
                    "@type": ["dpv:ConsentGiven"]
                }]
            },
            { // Decision process - inherits data and storage location
                "@type": ["@dpv:DecisionProcess"],
                "dpv:hasRecipient": ["company:Beta"],
                "dpv:hasConsentStatus": [{
                    "@type": ["dpv:ConsentRefused"]
                }]
            }
        ]
    }]
}
```

This is semantically equivalent to the case where the data and storage location processes are duplicated in each recipient process:

```json
{
    "dpv:hasProcess": [{
        // Other common info goes here ...
        "dpv:hasProcess": [
            {
                "@type": ["@dpv:DecisionProcess"],
                "dpv:hasRecipient": ["company:Alpha"],
                "dpv:hasProcess": [ // DUPLICATED info
                    {
                        "@type": ["@dpv:Process"],
                        "dpv:hasPersonalData": ["pd:Name"],
                        "dpv:hasStorageCondition": [{
                            "@type": ["dpv:StorageLocation"],
                            "dpv:hasLocation": ["loc:IE"]
                        }]
                    },
                    {
                        "@type": ["@dpv:Process"],
                        "dpv:hasPersonalData": ["pd:Email"],
                        "dpv:hasStorageCondition": [{
                            "@type": ["dpv:StorageLocation"],
                            "dpv:hasLocation": ["loc:CH"]
                        }]
                    }
                ],
                "dpv:hasConsentStatus": [{
                    "@type": ["dpv:ConsentGiven"]
                }]
            },
            {
                "@type": ["@dpv:DecisionProcess"],
                "dpv:hasRecipient": ["company:Beta"],
                "dpv:hasProcess": [ // DUPLICATED info
                    {
                        "@type": ["@dpv:Process"],
                        "dpv:hasPersonalData": ["pd:Name"],
                        "dpv:hasStorageCondition": [{
                            "@type": ["dpv:StorageLocation"],
                            "dpv:hasLocation": ["loc:IE"]
                        }]
                    },
                    {
                        "@type": ["@dpv:Process"],
                        "dpv:hasPersonalData": ["pd:Email"],
                        "dpv:hasStorageCondition": [{
                            "@type": ["dpv:StorageLocation"],
                            "dpv:hasLocation": ["loc:CH"]
                        }]
                    }
                ],
                "dpv:hasConsentStatus": [{
                    "@type": ["dpv:ConsentRefused"]
                }]
            }
        ]
    }]
}
```

A `DecisionProcess` is also an indication to the UI/UX to understand where decisions can be made or changed. Therefore, any information within such a decision process represents a potential control for the individual to make decisions over. If the individual cannot make decisions for a given field or information, then it should not be in a decision process.

In the following example, the decision process implies that the purposes and recipients can be used as part of the controls for the individual to make or change their decision e.g. by providing necessary UI/UX controls to individually allow or deny each recipient to receive data for each specific purpose i.e. Alpha can be allowed to receive data for Fraud Prevention and Detection but not for marketing.

```json
{
    // Other common info goes here ...
    "dpv:hasProcess": [{
        "dpv:hasProcess": [
            {
                "@type": ["@dpv:DecisionProcess"],
                "dpv:hasPurpose": ["dpv:Marketing", "dpv:FraudPreventionDetection"],
                "dpv:hasRecipient": ["company:Alpha", "company:Beta"],
                "dpv:hasConsentStatus": [{
                    "@type": ["dpv:ConsentRefused"]
                }]
            }
        ]
    }]
}
```

## Overlapping Consent Statuses

To avoid ambiguity in what the consent covers, overlapping or nesting `DecisionProcesses` is prohibited. This means the following example is not a valid consent record as it is ambiguous where there is a consent given for Alpha to use the data for Marketing but consent is refused for Beta for the same process. The ambiguity arises as Alpha's consent given information is inherited within the decision process where consent is refused for Beta based on the inheritance rules for processes.

```json
{
    "dpv:hasProcess": [{
        "@type": ["@dpv:DecisionProcess"],
        // Other common info goes here ...
        "dpv:hasPurpose": ["dpv:Marketing"],
        "dpv:hasDataController": ["company:Alpha"],
        "dpv:hasProcess": [
            {
                "@type": ["@dpv:DecisionProcess"],
                
                "dpv:hasRecipient": ["company:Beta"],
                "dpv:hasConsentStatus": [{
                    "@type": ["dpv:ConsentRefused"]
                }]
            }
        ],
        "dpv:hasConsentStatus": [{ // Ambigious overlap with nested Consent Refused
            "@type": ["dpv:ConsentGiven"]
        }]
    }],
    
}
```


Instead, the following example is clear and is unambigious as to what was accepted and what was refused even though the actual information fields are the same (just in a different place).

```json
{
    "dpv:hasProcess": [{
        "@type": ["@dpv:DecisionProcess"],
        // Other common info goes here ...
        "dpv:hasPurpose": ["dpv:Marketing"],
        "dpv:hasProcess": [
            {
                "@type": ["@dpv:DecisionProcess"],
                "dpv:hasPurpose": ["dpv:Marketing"],
                "dpv:hasRecipient": ["company:Alpha"],
                "dpv:hasConsentStatus": [{
                    "@type": ["dpv:ConsentRefused"]
                }]
            },
            {
                "dpv:hasDataController": ["company:Alpha"],
                "dpv:hasConsentStatus": [{ // Ambigious overlap with nested Consent Refused
                    "@type": ["dpv:ConsentGiven"]
                }]
            }
        ]
    }],
    
}
```
