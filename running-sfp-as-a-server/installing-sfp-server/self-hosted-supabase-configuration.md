---
description: >-
  The page details the configurations that are required in your self hosted
  supabase instance for sfp-server to work effectively
---

# Self Hosted Supabase Configuration

In your .env file for your supabase instance,  please ensure the following are authenticated

**Authentication Related**

The following callback URL's have to be configured on the supabse db for sfp cli and codev to authenticate through OAuth

```
//Configure Redirect URI 
GOTRUE_URI_ALLOW_LIST="io.flxbl.codev://auth/callback,http://localhost:54329/callback"
```
