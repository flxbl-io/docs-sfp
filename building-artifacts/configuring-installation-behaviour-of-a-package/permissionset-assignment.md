# PermissionSet Assignment

<table><thead><tr><th width="229">Attribute</th><th>Type</th><th>Description</th><th>Package Types Applicable</th></tr></thead><tbody><tr><td>assignPermSetsPreDeployment</td><td>array</td><td>Apply permsets before installing an artifact to the deployment user</td><td><p></p><ul><li>unlocked </li><li>org-dependent unlocked</li><li>source</li><li>diff</li></ul><p></p></td></tr><tr><td>assignPermSetsPostDeployment</td><td>array</td><td>Apply permsets after installing an artifact to the deployment user</td><td><p></p><ul><li>unlocked </li><li>org-dependent unlocked</li><li>source</li><li>diff</li></ul></td></tr></tbody></table>



Occasionally you might need to assign a permission set for the deployment user to successfully install the package, run apex tests or  functional tests, sfp provides you an easy mechanism to assign permission set  either before installation or after installation of an artifact.  \
\
**assignPermSetsPreDeployment** assumes the permission sets are already deployed on the target org and proceed to assign these permission sets to the user

<pre><code> {
      "path": "src/health-cloud",
      "package": "health-cloud",
      "versionName": "health-cloud-1.0",
      "versionNumber": "1.0.0.0",
      "<a data-footnote-ref href="#user-content-fn-1">assignPermSetsPreDeployment</a>": [
        "HealthCloudFoundation",
        "HealthCloudSocialDeterminants",
        "HealthCloudAppointmentManagement",
        "HealthCloudVideoCalls",
        "HealthCloudUtilizationManagement",
        "HealthCloudMemberServices",
        "HealthCloudAdmin",
        "HealthCloudApi",
        "HealthCloudLimited",
        "HealthCloudPermissionSetLicense",
        "HealthCloudStandard",
        "HealthcareAssociatedInfectionDiseaseGroupAccess"
      ]
    },
</code></pre>

[\
](https://open.gitbook.com/\~space/2r1H8wvoufdI0GKojSUo/sfpowerscripts/types-of-packaging/data-packages)**assignPermSetsPostDeployment**  can be used to assign permission sets that are introduced by the artifact to the target org for any aspects of testing or any other automation usage



[^1]: use assignPermSetsPreDeployment for assigning permission sets before installing an artifact
