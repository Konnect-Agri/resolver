## Consent Artifact
A consent artifact is a document which defines all the entities, parameters, access levels, data restrictions etc for a consent.

A detailed doc on consent artifact can be found [here](https://dla.gov.in/sites/default/files/pdf/MeitY-Consent-Tech-Framework%20v1.1.pdf)


The following json describes the structure of a consent artifact
```
{
    "signature": "",
    "created": "YYYY-MM-DDThh:mm:ssZn.n",
    "expires": "YYYY-MM-DDThh:mm:ssZn.n",
    "id": ""
    "revocable": false,
    "collector": {
        "id": "did:collector:123",
        "url": "https://sample-collector/api/v1/collect"
    },
    "consumer": {
        "id": "did:consumer:123",
        "url": "https://sample-consumer/api/v1/consume" //webhook on revoke, accept
    },
    "provider": {
        "id": "did:proider:123",
        "url": "https://sample-consumer/api/v1" //webhook
    },
    "user": {
        "id": "did:user:123", //FA ID, Accept or Reject => notify collector
    },
    "revoker": {
        "url": "https://sample-revoker/api/v1/revoke",
        "id": "did:user:123"
    },
    "purpose": "",
    "user_sign": "",
    "collector_sign": "",
    "frequency": {
        "ttl": 1200,
        "limit": 2
    },
    "total_queries_allowed": 1,
    "log": {
        "consent_use": {
            "url": "https://sample-log/api/v1/log"
        },
        "data_access": {
            "url": "https://sample-log/api/v1/log"
        }
    },
    "data": <Valid superset GraphQL query of consented data>",
    "proof": {
        "type": "RsaSignature2018",
        "created": "2017-06-18T21:19:10Z",
        "proofPurpose": "assertionMethod",
        "verificationMethod": "https://example.edu/issuers/565049#key-1",
        "jws": "eyJhbGciOiJSUzI1NiIsImI2NCI6ZmFsc2UsImNyaXQiOlsiYjY0Il19..TCYt5X
                sITJX1CxPCT8yAV-TVkIEq_PbChOMqsLfRoPsnsgw5WEuts01mq-pQy7UJiN5mgRxD-WUc
                X16dUEMGlv50aqzpqh4Qktb3rk-BuQy72IFLOqV0G_zS245-kronKb78cPN25DGlcTwLtj
                PAYuNzVBAh4vGHSrQyHUdBBPM"
    }
}
```


### Query of consent artifact
Query structure of consent artifact is same as [GraphQL Oct 2021 Release](https://spec.graphql.org/October2021/#sec-Overview), we recommend giving it a read since it's cirtical in understanding the consent artifact.


The query param inside a consent artifact defines what data is allowed to be accessed by the consent artifact
We can define this access for every query root to give very fine grained control of the data.

---

Example query to fetch attendances of people of all industries in district "abu" 

```
attendance(where: {industry: {district: {_eq: "abu"}}}) {
    date
    name
    created_at
    industry {
      district
    }
  }
```
