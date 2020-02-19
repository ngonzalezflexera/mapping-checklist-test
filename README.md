# mapping-checklist-test
Testing prerrequisites to check before integrating the code in dev/staging/production

*Manual test before integration*

Generate a token that would allow you to to send request to the software/unknown processor.

**To check that the the software-installer is working**

kubectl port-forward trs-mapping-software-installer-service-XXXXX-XXXXX 8000:8000

Send an item that is know to the 4 endpoints:

```
{
  "source_id": "9a15d4d5-ad63-4678-8d64-bc82bad89396",
  "source_type": "SCCM",
  "title": "Microsoft Money",
  "manufacturer": "Microsoft",
  "version": "14",
  "org_id": 100,
  "instance_id": [
    "42174a7e-b358-4ea5-b01c-e7ff9a584138"
  ]
}
```

The result for the endpoints should be:

MapItem:

```
{
  "items": {
    "field": [
      {
        "technopedia_id": "495bc46a-f2a9-4b99-a1d1-e1cd9cbdafee",
        "class": "software",
        "type": "product"
      },
      {
        "technopedia_id": "3a17ebc3-a1cd-4cfd-9c23-83dd53a3b16e",
        "class": "software",
        "type": "release"
      },
      {
        "technopedia_id": "709088ed-5f7c-46c7-b247-877ecb42462e",
        "class": "software",
        "type": "version"
      }
    ]
  }
}
```

For MapItems:
```
{
  "items": {
    "field": [
      {
        "items": {
          "field": [
            {
              "technopedia_id": "495bc46a-f2a9-4b99-a1d1-e1cd9cbdafee",
              "class": "software",
              "type": "product"
            },
            {
              "technopedia_id": "3a17ebc3-a1cd-4cfd-9c23-83dd53a3b16e",
              "class": "software",
              "type": "release"
            },
            {
              "technopedia_id": "709088ed-5f7c-46c7-b247-877ecb42462e",
              "class": "software",
              "type": "version"
            }
          ]
        }
      }
    ]
  }
}
```
For Mappable
```
{
  "mappable": "Mappable",
  "updated_at": "2009-08-13 14:26:39"
}
```
For MappableStream
```
{
  "mappable": "Mappable",
  "updated_at": "2009-08-13 14:26:39",
  "original_payload": {
    "instance_id": [
      "577293bd-b775-4280-a1c8-163909250c92"
    ],
    "title": "Microsoft Money",
    "manufacturer": "Microsoft",
    "version": "14",
    "source_type": "Hello",
    "source_id": "076c14ec-90f6-46de-9107-98640bf11c87",
    "org_id": 100
  },
  "error": ""
}
```
Check that no errors were logged in the logs.

**To check that the unknown-processor is working**

kubectl port-forward trs-mapping-software-installer-service-XXXXX-XXXXX 8000:8000
Send to the mappableEndpoint the next items:
```

{
  "title": "Microsoft Money1",
  "manufacturer": "Microsoft",
  "version": "14",
  "source_type": "Hello",
  "source_id": "076c14ec-90f6-46de-9107-98640bf11c87",
  "org_id": 100,
  "instance_id": [
    "377293bd-b775-4280-a1c8-163909250c92"
  ]
}
{
  "title": "Microsoft Money1",
  "manufacturer": "Microsoft",
  "version": "14",
  "source_type": "Hello",
  "source_id": "076c14ec-90f6-46de-9107-98640bf11c87",
  "org_id": 100,
  "instance_id": [
    "277293bd-b775-4280-a1c8-163909250c92"
  ]
}
{
  "title": "Microsoft Money1",
  "manufacturer": "Microsoft",
  "version": "14",
  "source_type": "Hello",
  "source_id": "076c14ec-90f6-46de-9107-98640bf11c87",
  "org_id": 100,
  "instance_id": [
    "177293bd-b775-4280-a1c8-163909250c92"
  ]
}
{
  "title": "Microsoft Money2",
  "manufacturer": "Microsoft",
  "version": "14",
  "source_type": "Hello",
  "source_id": "076c14ec-90f6-46de-9107-98640bf11c87",
  "org_id": 100,
  "instance_id": [
    "577293bd-b775-4280-a1c8-163909250c92"
  ]
}
```
kubectl port-forward trs-unknown-processor-service-XXXXX-XXXXX 8000:8000

Hit the endpoint with the same org_id used in the example (ord_id:100)

The kinesis stream attached to the process should report 2 messages been sent.

Check the errors, no logs should be there
