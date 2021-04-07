# Adding Endpoints

## Supported Endpoints <a id="supported-endpoints"></a>

Endpoint in Mangle refers to an infrastructure component that will be the primary target for your chaos engineering experiments.

For **version 1.0**, Mangle supported four types of endpoints:

1. Kubernetes
2. Docker
3. VMware vCenter
4. Remote Machine

From **version 2.0**, apart from the four endpoints listed above, support has been extended to the following new endpoint:

1. AWS \(Amazon Web Services\)

### Kubernetes Endpoint <a id="kubernetes-endpoint"></a>

Mangle supports K8s clusters as endpoints or targets for injection. It needs a kubeconfig file to connect to the cluster and run the supported faults. If a kubeconfig file is not provided, Mangle assumes that it is running on a K8s cluster and targets the same cluster for fault injection.

**Steps to follow:**

1. Login as an user with read and write privileges to Mangle.
2. Navigate to Endpoint tab ---&gt; Kubernetes Cluster.
3. Click on ![](https://firebasestorage.googleapis.com/v0/b/gitbook-28427.appspot.com/o/assets%2F-LcVKiIEQZ_SDz8uqA0g%2F-Leqa65y0ZygdSkgOJJ7%2F-Leqi7_EWioURSm2AO-4%2FK8sClusterButton.png?alt=media&token=a5a336c5-60a6-485f-aa63-14164dd8a603).
4. Enter a name, credential \(kubeconfig file\), namespace \(mandatory...Please specify "default" if you are unsure of the namespace else provide the actual name\), tags \(refers to additional tags that should be send to the enabled metric provider to uniquely identify faults against that endpoint\) and click on **Test Connection**.
5. If **Test Connection** succeeds click on **Submit**.
6. A success message is displayed and the table for Endpoints will be updated with the new entry.
7. Click on ![](https://firebasestorage.googleapis.com/v0/b/gitbook-28427.appspot.com/o/assets%2F-LcVKiIEQZ_SDz8uqA0g%2F-Leac8tB-KodjFhsg2Zs%2F-Lead2bNkR2AZju-PTuh%2FSupportedActionsButton.png?alt=media&token=b99be285-99bb-42ed-b6b9-9100d4664a4c) against a table entry to see the supported operations.

### Docker Endpoint <a id="docker-endpoint"></a>

Mangle supports docker hosts as endpoints or targets for injection. It needs the IP/Hostname, port details and certificate details \(if TLS is enabled for the docker host with --tlsverify option specified\) to connect to the docker host and run the supported faults.

**Steps to follow:**

1. Login as an user with read and write privileges to Mangle.
2. Navigate to Endpoint tab.
3. Click on ![](https://firebasestorage.googleapis.com/v0/b/gitbook-28427.appspot.com/o/assets%2F-LcVKiIEQZ_SDz8uqA0g%2F-Leqa65y0ZygdSkgOJJ7%2F-LeqlLq1mrO3tf-BdgJR%2FDockerButton.png?alt=media&token=01024b51-173f-45aa-998f-4f164345c5ef).
4. Enter a name, IP/Hostname, port details, tags \(refers to additional tags that should be send to the enabled metric provider to uniquely identify faults against that endpoint\), certificate details \(if TLS is enabled for the docker host\)and click on **Test Connection**.
5. If **Test Connection** succeeds click on **Submit**.
6. A success message is displayed and the table for Endpoints will be updated with the new entry.
7. Click on ![](https://firebasestorage.googleapis.com/v0/b/gitbook-28427.appspot.com/o/assets%2F-LcVKiIEQZ_SDz8uqA0g%2F-Leac8tB-KodjFhsg2Zs%2F-Lead2bNkR2AZju-PTuh%2FSupportedActionsButton.png?alt=media&token=b99be285-99bb-42ed-b6b9-9100d4664a4c) against a table entry to see the supported operations.

### VMware vCenter Endpoint <a id="vmware-vcenter-endpoint"></a>

Mangle supports VMware vCenter as endpoints or targets for injection. It needs the IP/Hostname, credentials and a vCenter adapter URL to connect to the vCenter instance and run the supported faults.

**Steps to follow:**

1. Login as an user with read and write privileges to Mangle.
2. Navigate to Endpoint tab.
3. Click on ![](https://firebasestorage.googleapis.com/v0/b/gitbook-28427.appspot.com/o/assets%2F-LcVKiIEQZ_SDz8uqA0g%2F-Leqm67oTkhybsf0L8B5%2F-LeqnR5o36JWymcJ_4bS%2FVcenterButton.png?alt=media&token=374ebbff-5a3f-467c-a66d-78277e9c93e0).
4. Enter a name, IP/Hostname, credentials, vCenter Adapter URL \(format: "_https://:_"where the IP/hostname is the docker host where the adapter container runs appended with the port used\), username, password, tags \(refers to additional tags that should be send to the enabled metric provider to uniquely identify faults against that endpoint\) and click on **Test Connection**.
5. If **Test Connection** succeeds click on **Submit**.
6. A success message is displayed and the table for Endpoints will be updated with the new entry.
7. Click on ![](https://firebasestorage.googleapis.com/v0/b/gitbook-28427.appspot.com/o/assets%2F-LcVKiIEQZ_SDz8uqA0g%2F-Leac8tB-KodjFhsg2Zs%2F-Lead2bNkR2AZju-PTuh%2FSupportedActionsButton.png?alt=media&token=b99be285-99bb-42ed-b6b9-9100d4664a4c) against a table entry to see the supported operations.

When the vCenter adapter is deployed on the same machine on which Mangle is running, vCenter adapter IP used for adding vCenter endpoint can be either

* A internal docker container IP OR
* A docker host IP

**To find out the internal docker container IP for mangle-vc-adapter run**

`docker inspect --format '{{.NetworkSettings.IPAddress}}' *mangle-vc-adapter`

### Remote Machine Endpoint <a id="remote-machine-endpoint"></a>

Mangle supports any remote machine with ssh enabled as endpoints or targets for injection. It needs the IP/Hostname, credentials \(either password or private key\), ssh details, OS type and tags to connect to the remote machine and run the supported faults.

**Steps to follow:**

1. Login as an user with read and write privileges to Mangle.
2. Navigate to Endpoint tab.
3. Click on ![](https://firebasestorage.googleapis.com/v0/b/gitbook-28427.appspot.com/o/assets%2F-LcVKiIEQZ_SDz8uqA0g%2F-LeqqtTNNlITlGxt1daB%2F-LeqsZGMk-f12vQMDPU1%2FRemoteMachineButton.png?alt=media&token=cf4573ca-a7dd-4066-a83b-54b3a8729c60).
4. Enter a name, IP/Hostname, credentials \(either password or private key\), ssh port, ssh timeout, OS type, tags \(refers to additional tags that should be send to the enabled metric provider to uniquely identify faults against that endpoint\) and click on **Test Connection**.
5. If **Test Connection** succeeds click on **Submit**.
6. A success message is displayed and the table for Endpoints will be updated with the new entry.
7. Click on ![](https://firebasestorage.googleapis.com/v0/b/gitbook-28427.appspot.com/o/assets%2F-LcVKiIEQZ_SDz8uqA0g%2F-Leac8tB-KodjFhsg2Zs%2F-Lead2bNkR2AZju-PTuh%2FSupportedActionsButton.png?alt=media&token=b99be285-99bb-42ed-b6b9-9100d4664a4c) against a table entry to see the supported operations.

### AWS \(Amazon Web Services\) <a id="aws-amazon-web-services"></a>

Mangle supports AWS as endpoint or target for injection. It needs the Region, credentials \(Access key ID and Secret key\) and tags to connect to AWS and run the supported faults. Currently the only supported service is EC2. However, there are plans to extend this to other important services in AWS.

**Steps to follow:**

1. Login as an user with read and write privileges to Mangle.
2. Navigate to Endpoint tab.
3. Click on ![](https://firebasestorage.googleapis.com/v0/b/gitbook-28427.appspot.com/o/assets%2F-LcVKiIEQZ_SDz8uqA0g%2F-LqjmLkd7e9HNT6y74Ej%2F-LqjysvOi0XbuoNnaKDb%2FAWS.png?alt=media&token=aaffb80a-cb8c-4cae-82f0-5640b021d7fe).
4. Enter a name, Region, credentials \(Access key ID and Secret key\), tags \(refers to additional tags that should be send to the enabled metric provider to uniquely identify faults against that endpoint\) and click on **Test Connection**.
5. If **Test Connection** succeeds click on **Submit**.
6. A success message is displayed and the table for Endpoints will be updated with the new entry.
7. Click on ![](https://firebasestorage.googleapis.com/v0/b/gitbook-28427.appspot.com/o/assets%2F-LcVKiIEQZ_SDz8uqA0g%2F-Leac8tB-KodjFhsg2Zs%2F-Lead2bNkR2AZju-PTuh%2FSupportedActionsButton.png?alt=media&token=b99be285-99bb-42ed-b6b9-9100d4664a4c) against a table entry to see the supported operations.

## Relevant API Reference <a id="relevant-api-reference"></a>

**For access to Swagger documentation:**

Please traverse to link ![](https://firebasestorage.googleapis.com/v0/b/gitbook-28427.appspot.com/o/assets%2F-LcVKiIEQZ_SDz8uqA0g%2F-Leac8tB-KodjFhsg2Zs%2F-LeacYoaMEsURLcmrZyl%2FHelp.png?alt=media&token=e28a8f10-207c-4ae0-a803-2949ee85c749) -----&gt; API Documentation from the Mangle UI or access _https:///mangle-services/swagger-ui.html\#_/_endpoint-controller_

  ![](https://firebasestorage.googleapis.com/v0/b/gitbook-28427.appspot.com/o/assets%2F-LcVKiIEQZ_SDz8uqA0g%2F-Lf4J8nIngzw1Yrwg0f0%2F-Lf4MWbZt2a1LP9vjChy%2FEndpointController.png?alt=media&token=b1b9b451-acc0-43d8-a710-36564afe7ee7)

