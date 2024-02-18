# Configuring installation behaviour of an artifact

sfp provides various features to alter the installation behaviour of an artifact. These behaviours have to be applied  as additonal property of a package during build time. The following section details each of  the parameters that is available.

{% hint style="info" %}
sfp is built on the concept of immutable artifacts, hence any properties to control the installation aspects of an artifact  need to be applied during the build command.  Installation behaviour of an artifact cannot be controlled dynamically. If you need to alter or add a new behaviour, please build a new version of the artifact
{% endhint %}
