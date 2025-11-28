# Domains

sfp is a natural fit for organisations that utilize Salesforce in a large enterprise setting as its purpose built to deal with modular saleforce development. Often these organisations have teams dedicated to a particular business function, think of a team who works on features related to billing, while a team works on features related to service\\

<figure><img src="../.gitbook/assets/image (2) (1) (2).png" alt=""><figcaption><p>Packages in an org organised by domains</p></figcaption></figure>

The diagram illustrates the method of organizing packages into specific categories for various business units. These categories are referred to as **'domains'** within the context of sfp cli.\
\
Each domain can either contain further domains ( sub-domains) and each domain constitute of one or more packages.

**sfp** cli utilizes ['Release Config'](../configuring-a-project/release-config.md) to organise packages into domains. You can read more about creating a release config in the next section
