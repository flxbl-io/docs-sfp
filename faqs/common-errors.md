# Common Errors

#### No artifacts Built

When I generate the quick build, no artifacts are built.&#x20;

```
➜  flxbl-demo git:(main) sfp orchestrator:quickbuild -v DXScaleProd --branch main
------------------------------------------------------------------------------------------
sfp  -- Salesforce Package Manager -Version:30.3.5 -Release:January 24
------------------------------------------------------------------------------------------
command: quickbuild
Build Packages Only Changed: false
Config File Path: config/project-scratch-def.json
Artifact Directory: artifacts
------------------------------------------------------------------------------------------
Packages scheduled for build
                                
 Package   Reason to be built 
                                

Resolving dependencies

Validating Project Dependencies...



Building Packages




Generating Artifacts and Tags....
```

Resolutions:

````
Confirm that you have added the additional attributes in your sfdx-project.json file.  If you have not edited, then it will not generate.


{
  "packageDirectories": [
    {
      "path": "force-app",
      "package": "force-app",
      "versionNumber": "1.0.0.1",      
      "default": true
    }
  ],
  "name": "flxbl-demo",
  "namespace": "",
  "sfdcLoginUrl": "https://login.salesforce.com",
  "sourceApiVersion": "59.0"
}
```

````
