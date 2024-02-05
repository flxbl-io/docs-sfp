# Build Artifacts

```
sfp orchestrator:quickbuild -v DevHub --branch main
```

Explain this output



```
// Some code

------------------------------------------------------------------------------------------
sfp  -- Salesforce Package Manager -Version:31.0.0 -Release:January 24
------------------------------------------------------------------------------------------
command: build
Build Packages Only Changed: false
Config File Path: config/project-scratch-def.json
Artifact Directory: artifacts
------------------------------------------------------------------------------------------
Packages scheduled for build
                                                      
 Package     Reason to be built                     
 force-app   Activated as part of all package build 
                                                      

Resolving dependencies

Validating Project Dependencies...



Building Packages


Packages currently processed:{1} + force-app
Awaiting Dependencies to be resolved:{0} + 
Package creation initiated for force-app

force-app package created in 00:00:00.002
-- Package Details:--
  Package Type           :  source                                   
  Package Version Number :  1.0.0.1                                  
  Metadata Count         :  2                                        
  Apex In Package        :  No                                       
  Profiles In Package    :  No                                       
  Source Version         :  a4ad7c9e244b585b81012578ba26f5f2979a7e66 





Generating Artifacts and Tags....
Artifact Generation Completed for source to /Users/vuha/sfp-demo/artifacts/force-app_sfpowerscripts_artifact_1.0.0-1.zip
------------------------------------------------------------------------------------------
1 packages created in 00:00:00.297 minutes with 0 errors
------------------------------------------------------------------------------------------
```



<figure><img src="../.gitbook/assets/image (1).png" alt=""><figcaption></figcaption></figure>
