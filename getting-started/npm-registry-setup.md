# NPM Registry Setup

To keep it simple the following guide will setup using GitHub.

Packages are needed to store your artifacts which is a state in time of your metadata.  For the first example, we are focusing on 1 single package force-app and that will be pushed to the NPM Registry.

1. Create your PAT token for GitHub
   1. Settings > Developer Settings > Token > Classic ([Link](https://docs.github.com/en/authentication/keeping-your-account-and-data-secure/managing-your-personal-access-tokens#creating-a-fine-grained-personal-access-token))
2. Create a \~/.npmrc file in your local user

```
//npm.pkg.github.com/:_authToken=TOKEN
```

3. for the sfdx project, add this

```
@NAMESPACE:registry=https://npm.pkg.github.com
```

4.

References

* [Working with the npm registry](https://docs.github.com/en/packages/working-with-a-github-packages-registry/working-with-the-npm-registry)



Packages Create

<figure><img src="../.gitbook/assets/image (1) (1).png" alt=""><figcaption><p>GitHub Repository</p></figcaption></figure>

1. Packages > Publish your first package
2. NPM
3.

For other NPM registries like GitLabs or Azure Devops, documentation will be provided in the future.

