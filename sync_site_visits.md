API End Point


URL - `https://lead.anarock.com/api/v0/:channel/third-party/create-site-visit`

Test URL - `https://marketing.anarock.com/api/v0/test_channel/third-party/create-site-visit`

METHOD - `POST`

PARAMS 

```js
{
  "project_id": 726,
  "integration_key": "516501274ba4288a",
  "country_code": "+91",
  "phone": "9819619866",
  "name": "Test Name",
  "email": "test@test.com",
  "sourcing_agent_email_ids": ["agent1@anarock.com", "agent2@anarock.com"],
  "closing_agent_email_ids": ["agent3@anarock.com", "agent4@anarock.com"],
  "min_budget": 1000000,
  "max_budget": 12110000,
  "interested_property_types": ["1 BHK", "2 BHK", "3 BHK"],
  "purpose_of_purchase": "Investment", // "End Use"
  "pincode": 400051,
  "occupation": "Business",
  "possession_in": "1 year",  // "1 year", "2 year", "Ready to move in"
  "gender": "male", // "male", "female", "others"
  "source": "google", // google, facebook, radio, 
  "cp_data": {
    "name": "CP Name",
    "country_code: "+91",
    "phone": "9879876434",
    "firm_name": "CP Firm Name"
  },
  "visit_date": 1597342829 // epoch timestamsp in seconds
}
```

Response 
