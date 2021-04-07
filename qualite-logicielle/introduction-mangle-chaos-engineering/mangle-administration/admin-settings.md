# Admin Settings

## Managing Authentication <a id="managing-authentication"></a>

### Adding additional Authentication sources <a id="adding-additional-authentication-sources"></a>

Mangle supports using Active Directory as an additional authentication source.

**Steps to follow:**

1. Login as an admin user to Mangle.
2. Navigate to ![](https://firebasestorage.googleapis.com/v0/b/gitbook-28427.appspot.com/o/assets%2F-LcVKiIEQZ_SDz8uqA0g%2F-Leac8tB-KodjFhsg2Zs%2F-LeacRGnZgE8i-v4kGwo%2FSettings.png?alt=media&token=8d4c0478-f36e-4369-a3bf-068f716763fc) -----&gt; Auth Management -----&gt; Auth Source .
3. Click on ![](https://firebasestorage.googleapis.com/v0/b/gitbook-28427.appspot.com/o/assets%2F-LcVKiIEQZ_SDz8uqA0g%2F-Leac8tB-KodjFhsg2Zs%2F-LeacUUjSyzEtnTn8YES%2FAuthSourceButton.png?alt=media&token=293d3f41-b50e-4f8b-94a0-5a8414741f42).
4. Enter URL, Domain and click on **Submit**.
5. A success message is displayed and the table for Auth sources will be updated with the new entry.
6. Click on ![](https://firebasestorage.googleapis.com/v0/b/gitbook-28427.appspot.com/o/assets%2F-LcVKiIEQZ_SDz8uqA0g%2F-Leac8tB-KodjFhsg2Zs%2F-Lead2bNkR2AZju-PTuh%2FSupportedActionsButton.png?alt=media&token=b99be285-99bb-42ed-b6b9-9100d4664a4c) against a table entry to see the supported operations.

**Relevant API List**

**For access to Swagger documentation, please traverse to link** ![](https://firebasestorage.googleapis.com/v0/b/gitbook-28427.appspot.com/o/assets%2F-LcVKiIEQZ_SDz8uqA0g%2F-Leac8tB-KodjFhsg2Zs%2F-LeacYoaMEsURLcmrZyl%2FHelp.png?alt=media&token=e28a8f10-207c-4ae0-a803-2949ee85c749) -----&gt; API Documentation from the Mangle UI or access _https:///mangle-services/swagger-ui.html\#/auth-provider-controller_

  ![](https://firebasestorage.googleapis.com/v0/b/gitbook-28427.appspot.com/o/assets%2F-LcVKiIEQZ_SDz8uqA0g%2F-Leac8tB-KodjFhsg2Zs%2F-LeacdL05vm61B9I3m71%2FAuth%20Provider%20Controller.png?alt=media&token=598009ac-66ea-44be-ab24-cb635ea0431a)

### Adding/Importing Users <a id="adding-importing-users"></a>

Mangle supports adding new local user or importing users from Active Directory sources added as additional authentication sources.

**Steps to follow:**

1. Login as an admin user to Mangle.
2. Navigate to ![](https://firebasestorage.googleapis.com/v0/b/gitbook-28427.appspot.com/o/assets%2F-LcVKiIEQZ_SDz8uqA0g%2F-Leac8tB-KodjFhsg2Zs%2F-LeacRGnZgE8i-v4kGwo%2FSettings.png?alt=media&token=8d4c0478-f36e-4369-a3bf-068f716763fc) -----&gt; Auth Management -----&gt; Users .
3. Click on ![](https://firebasestorage.googleapis.com/v0/b/gitbook-28427.appspot.com/o/assets%2F-LcVKiIEQZ_SDz8uqA0g%2F-Leac8tB-KodjFhsg2Zs%2F-LeacNaxtg1iNbB1FcFT%2FAddUserButton.png?alt=media&token=aa913a92-fd20-401e-8e0c-4b3634701a03).
4. Enter User Name, Auth Source, Password if the Auth Source selected is "mangle.local", an appropriate role and click on **Submit**.
5. A success message is displayed and the table for Users will be updated with the new entry.
6. Click on ![](https://firebasestorage.googleapis.com/v0/b/gitbook-28427.appspot.com/o/assets%2F-LcVKiIEQZ_SDz8uqA0g%2F-Leac8tB-KodjFhsg2Zs%2F-Lead2bNkR2AZju-PTuh%2FSupportedActionsButton.png?alt=media&token=b99be285-99bb-42ed-b6b9-9100d4664a4c) against a table entry to see the supported operations.

**Relevant API List**

**For access to Swagger documentation, please traverse to link** ![](https://firebasestorage.googleapis.com/v0/b/gitbook-28427.appspot.com/o/assets%2F-LcVKiIEQZ_SDz8uqA0g%2F-Leac8tB-KodjFhsg2Zs%2F-LeacYoaMEsURLcmrZyl%2FHelp.png?alt=media&token=e28a8f10-207c-4ae0-a803-2949ee85c749) -----&gt; API Documentation from the Mangle UI or access _https:///mangle-services/swagger-ui.html\#/user-management-controller_

![](https://firebasestorage.googleapis.com/v0/b/gitbook-28427.appspot.com/o/assets%2F-LcVKiIEQZ_SDz8uqA0g%2F-Leac8tB-KodjFhsg2Zs%2F-Leacs0IvVLAu7Lugxyd%2FUserManagementController.png?alt=media&token=6dc7772c-030a-4cf4-81cd-2a18e72f4bb1)

### Default and Custom Roles <a id="default-and-custom-roles"></a>

Mangle has the following default Roles and Privileges.

Edit and Delete operations are supported only for custom roles. It is forbidden for default roles.

Mangle supports creation of custom roles from the default privileges that are available.

**Steps to follow:**

1. Login as an admin user to Mangle.
2. Navigate to ![](https://firebasestorage.googleapis.com/v0/b/gitbook-28427.appspot.com/o/assets%2F-LcVKiIEQZ_SDz8uqA0g%2F-Leac8tB-KodjFhsg2Zs%2F-LeacRGnZgE8i-v4kGwo%2FSettings.png?alt=media&token=8d4c0478-f36e-4369-a3bf-068f716763fc) -----&gt; Auth Management -----&gt; Roles.
3. Click on ![](https://firebasestorage.googleapis.com/v0/b/gitbook-28427.appspot.com/o/assets%2F-LcVKiIEQZ_SDz8uqA0g%2F-Leac8tB-KodjFhsg2Zs%2F-Lead-oNq_aUxqb48NTE%2FCustomRoleButton.png?alt=media&token=2030c4bf-ddf9-489e-b55f-0e0881602759).
4. Enter Role Name, Privileges and click on **Submit**.
5. A success message is displayed and the table for Roles will be updated with the new entry.
6. Click on ![](https://firebasestorage.googleapis.com/v0/b/gitbook-28427.appspot.com/o/assets%2F-LcVKiIEQZ_SDz8uqA0g%2F-Leac8tB-KodjFhsg2Zs%2F-Lead2bNkR2AZju-PTuh%2FSupportedActionsButton.png?alt=media&token=b99be285-99bb-42ed-b6b9-9100d4664a4c) against a table entry to see the supported operations.

**Relevant API List**

**For access to Swagger documentation, please traverse to link** ![](https://firebasestorage.googleapis.com/v0/b/gitbook-28427.appspot.com/o/assets%2F-LcVKiIEQZ_SDz8uqA0g%2F-Leac8tB-KodjFhsg2Zs%2F-LeacYoaMEsURLcmrZyl%2FHelp.png?alt=media&token=e28a8f10-207c-4ae0-a803-2949ee85c749) -----&gt; API Documentation from the Mangle UI or access _https:///mangle-services/swagger-ui.html\#/role-controller_

![](https://firebasestorage.googleapis.com/v0/b/gitbook-28427.appspot.com/o/assets%2F-LcVKiIEQZ_SDz8uqA0g%2F-Leac8tB-KodjFhsg2Zs%2F-LeadGao13GAaSPPu63x%2FRoleController.png?alt=media&token=eec168ac-311f-4cf6-baeb-93d5a1e7cad5)

## Loggers <a id="loggers"></a>

### Log Levels <a id="log-levels"></a>

Mangle supports modifying log levels for the application.

**Steps to follow:**

1. Login as an admin user to Mangle.
2. Navigate to ![](https://firebasestorage.googleapis.com/v0/b/gitbook-28427.appspot.com/o/assets%2F-LcVKiIEQZ_SDz8uqA0g%2F-Leac8tB-KodjFhsg2Zs%2F-LeacRGnZgE8i-v4kGwo%2FSettings.png?alt=media&token=8d4c0478-f36e-4369-a3bf-068f716763fc) -----&gt; Loggers -----&gt; Log Levels .
3. Click on ![](https://firebasestorage.googleapis.com/v0/b/gitbook-28427.appspot.com/o/assets%2F-LcVKiIEQZ_SDz8uqA0g%2F-Lepw3Cs0ncVRHDzl2qz%2F-LepxSe1P-Be6pMvVAzD%2FLoggerButton.png?alt=media&token=897c22d6-4541-4442-bdf3-406febd61f67).
4. Enter Logger name, Configured Level, Effective Level and click on **Submit**.
5. A success message is displayed and the table for Log levels will be updated with the new entry.
6. Click on ![](https://firebasestorage.googleapis.com/v0/b/gitbook-28427.appspot.com/o/assets%2F-LcVKiIEQZ_SDz8uqA0g%2F-Leac8tB-KodjFhsg2Zs%2F-Lead2bNkR2AZju-PTuh%2FSupportedActionsButton.png?alt=media&token=b99be285-99bb-42ed-b6b9-9100d4664a4c) against a table entry to see the supported operations.

**Relevant API List**

**For access to Swagger documentation, please traverse to link** ![](https://firebasestorage.googleapis.com/v0/b/gitbook-28427.appspot.com/o/assets%2F-LcVKiIEQZ_SDz8uqA0g%2F-Leac8tB-KodjFhsg2Zs%2F-LeacYoaMEsURLcmrZyl%2FHelp.png?alt=media&token=e28a8f10-207c-4ae0-a803-2949ee85c749) -----&gt; API Documentation from the Mangle UI or access _https:///mangle-services/swagger-ui.html\#/operation-handler_

  ![](https://firebasestorage.googleapis.com/v0/b/gitbook-28427.appspot.com/o/assets%2F-LcVKiIEQZ_SDz8uqA0g%2F-Lepw3Cs0ncVRHDzl2qz%2F-LepzFuglA8kcfdqhCnW%2FOperationHandlerController.png?alt=media&token=17e6cfc6-f69e-436e-833e-1bf0cb077cc1)​

## Integrations <a id="integrations"></a>

### Metric Providers <a id="metric-providers"></a>

Mangle supports addition of either Wavefront or Datadog as metric providers. This enables the information about fault injection and remediation to be published to these tools as events thus making it easier to monitor them.

**Steps to follow:**

1. Login as an admin user to Mangle.
2. Navigate to ![](https://firebasestorage.googleapis.com/v0/b/gitbook-28427.appspot.com/o/assets%2F-LcVKiIEQZ_SDz8uqA0g%2F-Leac8tB-KodjFhsg2Zs%2F-LeacRGnZgE8i-v4kGwo%2FSettings.png?alt=media&token=8d4c0478-f36e-4369-a3bf-068f716763fc) -----&gt; Integrations -----&gt; Metric Providers .
3. Click on ![](https://firebasestorage.googleapis.com/v0/b/gitbook-28427.appspot.com/o/assets%2F-LcVKiIEQZ_SDz8uqA0g%2F-Lepw3Cs0ncVRHDzl2qz%2F-Leq2UFQ2fnvUel-5KVt%2FMonitoringToolButton.png?alt=media&token=b952ff39-0bfd-48b9-af28-266f70e99f74).
4. Choose Wavefront or Datadog, provide credentials and click on **Submit**.
5. A success message is displayed and the table for Monitoring tools will be updated with the new entry.
6. Click on ![](https://firebasestorage.googleapis.com/v0/b/gitbook-28427.appspot.com/o/assets%2F-LcVKiIEQZ_SDz8uqA0g%2F-Leac8tB-KodjFhsg2Zs%2F-Lead2bNkR2AZju-PTuh%2FSupportedActionsButton.png?alt=media&token=b99be285-99bb-42ed-b6b9-9100d4664a4c) against a table entry to see the supported operations.

On adding a metric provider, Mangle will send events automatically to the enabled provider for every fault injected and remediated. If the requirement is to monitor Mangle as an application by looking at its metrics, then click on the ![](https://firebasestorage.googleapis.com/v0/b/gitbook-28427.appspot.com/o/assets%2F-LcVKiIEQZ_SDz8uqA0g%2F-Lf-5fUp_jyQo15bj9bB%2F-Lf-73wFvO2pK0JlNoba%2FSendMetricsButton.png?alt=media&token=8216c034-3f61-41e2-b9bf-de57d1dc6310) button to enable sending of Mangle application metrics to the corresponding metric provider.

**Relevant API List**

**For access to Swagger documentation, please traverse to link** ![](https://firebasestorage.googleapis.com/v0/b/gitbook-28427.appspot.com/o/assets%2F-LcVKiIEQZ_SDz8uqA0g%2F-Leac8tB-KodjFhsg2Zs%2F-LeacYoaMEsURLcmrZyl%2FHelp.png?alt=media&token=e28a8f10-207c-4ae0-a803-2949ee85c749) -----&gt; API Documentation from the Mangle UI or access _https:///mangle-services/swagger-ui.html\#_/operation-handler

  ![](https://firebasestorage.googleapis.com/v0/b/gitbook-28427.appspot.com/o/assets%2F-LcVKiIEQZ_SDz8uqA0g%2F-Lepw3Cs0ncVRHDzl2qz%2F-LepzFuglA8kcfdqhCnW%2FOperationHandlerController.png?alt=media&token=17e6cfc6-f69e-436e-833e-1bf0cb077cc1)​

