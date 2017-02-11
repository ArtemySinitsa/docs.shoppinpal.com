#ACL Setup
Once you have proper roles and role resolvers added into your project the next step comes to setup proper [access controls (ACL)](https://loopback.io/doc/en/lb2/Controlling-data-access.html) for REST apis. 

You can setup right ACL for each built-in method or custom [remote methods](http://loopback.io/doc/en/lb3/Remote-methods.html) through javascript or you can add ACL rules in `model.json` file.


#####Example
For example, lets setup some ACL rules in organization model for a custom method names `addProductsInBulk` through `organization.json` file

```
...
....
"acls": [
        {
            "accessType": "EXECUTE",
            "principalType": "ROLE",
            "principalId": "adminForOrg",
            "permission": "ALLOW",
            "property": "addProductsInBulk"
        },
        {
            "accessType": "EXECUTE",
            "principalType": "ROLE",
            "principalId": "userForOrg",
            "permission": "ALLOW",
            "property": "addProductsInBulk"
        },
    ]
...
...
```