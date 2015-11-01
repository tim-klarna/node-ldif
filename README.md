# node-ldif
#### Nodejs LDIF (LDAP Data Interchange Format) parser based on [RFC2849](https://github.com/tapmodo/node-ldif/tree/master/rfc)

Unless you are an LDAP aficionado you may not know about the LDIF format.
I was surprised to learn that no LDIF parsing library existed for node.

So I wrote one, with [peg.js](http://pegjs.org). It aims to be RFC-compliant.  
Now I'll never have to use that cursed perl script again!

## Installation

Install easily with **npm**!

    npm install ldif

## Usage

##### Parse an LDIF file
```javascript
var LDIF = require('ldif');
var fs = require('fs');
var input = fs.readFileSync('./rfc/example1.ldif','utf8');

console.log(LDIF.parse(input));
```

##### Current object output for [example1.ldif](https://github.com/tapmodo/node-ldif/blob/master/rfc/example1.ldif)

```json
{
  "version": 1,
  "type": "content",
  "entries": [
    {
      "dn": "cn=Barbara Jensen, ou=Product Development, dc=airius, dc=com",
      "attributes": [
        {
          "attribute": "objectclass",
          "value": "top"
        },
        {
          "attribute": "objectclass",
          "value": "person"
        },
        {
          "attribute": "objectclass",
          "value": "organizationalPerson"
        },
        {
          "attribute": "cn",
          "value": "Barbara Jensen"
        },
        {
          "attribute": "cn",
          "value": "Barbara J Jensen"
        },
        {
          "attribute": "cn",
          "value": "Babs Jensen"
        },
        {
          "attribute": "sn",
          "value": "Jensen"
        },
        {
          "attribute": "uid",
          "value": "bjensen"
        },
        {
          "attribute": "telephonenumber",
          "value": "+1 408 555 1212"
        },
        {
          "attribute": "description",
          "value": "A big sailing fan."
        }
      ]
    },
    {
      "dn": "cn=Bjorn Jensen, ou=Accounting, dc=airius, dc=com",
      "attributes": [
        {
          "attribute": "objectclass",
          "value": "top"
        },
        {
          "attribute": "objectclass",
          "value": "person"
        },
        {
          "attribute": "objectclass",
          "value": "organizationalPerson"
        },
        {
          "attribute": "cn",
          "value": "Bjorn Jensen"
        },
        {
          "attribute": "sn",
          "value": "Jensen"
        },
        {
          "attribute": "telephonenumber",
          "value": "+1 408 555 1212"
        }
      ]
    }
  ]
}
```

That's a lot. But it's most descriptive of the file format. You can
throw it into some lodash, or map and reduce it as you please.
Some tooling like that will be added to this package soon.

As you will see next, parsing an LDIF "changes" formatted file
results in differently structured output. For now you're going to
have to do your own experimentation, as some of the output below
has been removed for brevity

##### Parsing LDIF changes [example6.ldif](https://github.com/tapmodo/node-ldif/blob/master/rfc/example6.ldif)

```json
{
  "version": 1,
  "type": "changes",
  "changes": [
    {
      "dn": "cn=Fiona Jensen, ou=Marketing, dc=airius, dc=com",
      "control": null,
      "changes": {
        "type": "add",
        "attributes": [
          {
            "attribute": "objectclass",
            "value": "top"
          },
          {
            "attribute": "objectclass",
            "value": "person"
          },
          {
            "attribute": "cn",
            "value": "Fiona Jensen"
          },
          {
            "attribute": "sn",
            "value": "Jensen"
          }
        ]
      }
    },
    {
      "dn": "cn=Robert Jensen, ou=Marketing, dc=airius, dc=com",
      "control": null,
      "changes": {
        "type": "delete"
      }
    },
    {
      "dn": "cn=Paul Jensen, ou=Product Development, dc=airius, dc=com",
      "changes": {
        "type": "modrdn",
        "newrdn": "cn=Paula Jensen"
      }
    },
    {
      "dn": "cn=Paula Jensen, ou=Product Development, dc=airius, dc=com",
      "changes": {
        "type": "modify",
        "changes": [
          {
            "type": "add",
            "attribute": "postaladdress",
            "values": [
              "123 Anystreet $ Sunnyvale, CA $ 94086"
            ]
          },
          {
            "type": "delete",
            "attribute": "description"
          },
          {
            "type": "replace",
            "attribute": "telephonenumber",
            "values": [
              "+1 408 555 1234",
              "+1 408 555 5678"
            ]
          }
        ]
      }
    }
  ]
}
```

## Implementation Notes

  * An attempt has been made to closely model RFC2849  
    (If you find any problems, file an issue!)
  * Supports both "content" and "change" file schemas
  * Able to parse all LDIF examples from RFC spec
  * Currently only returns plain object structure  
    This will change! (see first "TODO")

### TODO

  * Cast parsed values as defined objects
  * Ability to output LDIF format e.g. toString()
  * Streaming read interface (coming soon)
  * More complete documentation
  * Test suite

