# Limiting Validation by Domain

Validation processes often aim to synchronize the provided organization by installing packages that differ from those already installed. This task can become particularly time-consuming for large projects with hundreds of packages.

To streamline the validation process and focus it on specific domains, employing release config based modes is highly recommended. This approach limits the scope of validation, enhancing efficiency and reducing time.

```
 sfp validate org        -u ci 
                         -v devhub \
                         --diffcheck \
                         --mode=thorough-release-config \
                         --releaseconfig=<path-to-release-config>
```



In the above example the changes are only validated against the packages as mentioned in the provided release config



