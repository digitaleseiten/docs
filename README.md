## Submitting Leads to Digitaleseiten

### Step-by-step guide

Before you start make sure you have received a valid Cipid slug and API credentials from DigitaleSeiten Team, without a Cipid slug we can not identify your leads.


 

#### HOW-TO post leads via the API:
In order to post leads in our API first you need to provide a proper json lead data

```ruby

{
  "description": "a nice lead description", 
    "data":{
      "project_deadline": "Within the next 4 weeks", 
    "size": "30,00 m hight 13,00 m width"
  },
  "epid": "gartenbauorg", 
  "trade_slug": "gartenbau", 
  "order_type": "other", 
  "address_attributes": {
    "line_1": "schoenerhauser alle 6-7",
    "postal_code": "10119",
    "city": "berlin"
  },
  "contact_attributes": {
    "gender": "male",
    "first_name": "John",
    "last_name": "Cruz",
    "email": "john@example.com",
    "fix_phone": "+49123213223",
    "mobile_phone": "03023131"
  },
  "cipid":"SLUG YOU HAVE GOT FROM DS TEAM"
}
```

- `description` is the lead description

- `data`: includes the service deadline and other details (more keys and values can be added).

- `epid` is the vertical code which the lead is belong (see List of EPIDs for all codes).

- `address_attributes` (line_1) is the street address of the lead.

- `order_type` is the category of the lead e.g; TV-Technik. If you have more than one order_type please separate them by `/` or `;` You may need to translate your order_types to DigitaleSeiten order_types, contact them for more details.

- contact_attributes (fix_phone) is the phone number which the client is able to give feedback about the lead.

cipid is the name of the party which uses the API to post leads - basically name of your organization who tries to use this API.

NOTE: the lead which you send will be reviewed and if the content is not well provided then it will be marked as spam!



List of EPIDs:

```
"dachdeckercom",
"malerorg",
"fliesenlegernet",
"gartenbauorg",
"geruestbauorg",
"elektrikerorg",
"tischlerschreinerorg",
"klimatechnikernet",
"heizungsbaunet",
"sanitaerorg",
"autowerkstattde",
"bestatterorg",
"glasereiorg",
"schlossereinet",
"finanzberaternet",
"gutachterorg",
"optikersite",
"haarentfernungsite",
"fahrradreparaturorg",
"itserviceverzeichnisde",
"fusspflegesite",
"schaedlingsvernichtungde"

```

*List of trade_slugs:*
```
dachdecker
maler
fliesenleger
gartenbau
geruestbau
elektriker
tischlerschreiner
klimatechniker
heizungsbau
sanitaer
autowerkstatt
glaserei
schlosserei
finanzberater
gutachter
schaedlingsvernichtung
fensterbau
steuerberater24
bauunternehmen
```

Here is a basic curl command to post a lead:

```bash
curl -H "Content-Type: application/json" -X POST -d '{"description":"lead description/comment", "trade_slug": "maler", "data":{"project_deadline": "In den nächsten 4 Wochen", "project_surface": "150", "project_height": "220", "project_place": "Aussenwand", "order_type": "other"}, "order_type":"other","address_attributes":{"line_1":"schoenerhauser alle 6-7","postal_code":"10119","city":"berlin"},"epid":"malerorg","contact_attributes":{"gender":"male","first_name":"John6","last_name":"Cruz4","email":"john@example.com","fix_phone":"030234131","mobile_phone":"+49123213223"},"cipid":SLUG YOU HAVE GOT FROM DS TEAM"}' http://user:password@staging.leadlaundry.de/submarine
```

Successful response

a successful POST request will give you the below message:

```
 {"msg":"success"}%
 ```

Unsuccessful Response

an unsuccessful POST attempt will give you the below response:

```
HTTP Basic: Access denied.
```
or
```
 # usually happens when the epid or the trade_slug is not correct.
'{"msg":"Failed! -> Check the doc params are not valid! {}"}
```



Post a lead using Faraday in ruby:

```ruby
# above your class
# -*- coding: utf-8 -*-

API_URL = 'http://www.leadlaundry.de/submarine'
API_BASIC_AUTH_USER='Your Username'
API_BASIC_AUTH_PASS='Your Password'

 conn = Faraday.new(url: API_URL) do |faraday|
    faraday.request  :url_encoded
    faraday.adapter  Faraday.default_adapter
  end

  conn.basic_auth(API_BASIC_AUTH_USER, API_BASIC_AUTH_PASS)

  response = conn.post do |req|
    req.url "/submarine"
    req.headers['Content-Type'] = 'application/json'
    req.body = converted_lead.to_json
  end

  response

```
