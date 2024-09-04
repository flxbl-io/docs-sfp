# Delete Pools

To keep the pools up to date, a nightly scheduled job can be utilised which can delete all the scratch orgs in each pool. In the case of developer pools (pools with source tracking and used by developers as opposed to CI pools), care must be taken to delete only the `unassigned` pools by omitting the `--allscratchorgs` flag.\
\


{% hint style="warning" %}
Be careful when running delete command against developer pools with `-allscratchorg` flag. It will delete the scratch orgs that are used by developer and can result in potential loss of work
{% endhint %}

### Deleting Orphaned Scratch orgs

sfpowerscripts during prepare attempts to create multiple scratch orgs in parallel, while respecting the timeout as provided in Pool configuration. At times when the Salesforce infrastructure is stretched such as during release windows or during maintenance windows, it could be often seen some of the scratch orgs requested results in being timed out and are discarded by the rest of the pool activities. However, some of these scratch orgs are in fact created by Salesforce without respecting the timeout parameter, and counted against the active scratch org limits.

These scratch orgs titled **orphaned scratch orgs** in sfpowerscripts lingo can be reclaimed by issuing the pool: delete command with --orphans flag. This will delete these scratch orgs and hence allow to recover your active scratch org limits\


```
// Example command & output demonstrating delete of orphaned scratch orgs
sfp pool:delete -o -v flxbl    
```

\
