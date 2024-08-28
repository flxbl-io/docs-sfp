# Org Shapes

### 1. Expired Org Shape

\
**Error Message**: "No org shape exists for the specified sourceOrg. Create an org shape and try again."

**Symptom**: When attempting to use an Org Shape, an error is received stating that no org shape exists for the specified sourceOrg, despite the Org Shape being previously activated and the ID matching.\
\
**Solution:**   Recreate the org shape

### 2. Unable to create scratch org pools when Org Shape is used

**Error Message**: "Your password cannot be reset at this time. Please contact your organization's administrator for more information."

Symptom: When creating a pool of scratch orgs using a shape org, sfp is unable to provision it with the above error message

**Solution**:

The issue is related to the \`minimum 1 day password lifetime\` setting. If this setting is enabled, it may cause the above error. To resolve this, you can disable this setting in the scratch org definition file. Here is an example of how to do this:&#x20;

```
"securitySettings": {
            "passwordPolicies": {
                "minimumPasswordLifetime": false
            }
        }
```

After making this change, the error should no longer occur. If you still encounter issues, please create an issue in the repository for further assistance.
