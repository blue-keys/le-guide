# Custom Faults

Task-Extension: An example is available as `HelloManglePluginTaskHelper` at package com.vmware.mangle.test.plugin.helpers of mangle-plugin-skeleton. This task Helper is an implementation of `AbstractRemoteCommandExecutionTaskHelper`. The implementation of `AbstractRemoteCommandExecutionTaskHelper` is only expected to provide the implementation for below methods:

​

**`public Task init(T faultSpec) throws MangleException;`**

Should provide the Implementation to initialize the Task Helper for executing the Fault. And the commands required for injection/remediation of the Fault are expected to be provided here. More details on the model for providing the Command Information is explained later.

​

**`public Task init(T taskData, String injectedTaskId) throws MangleException;`**

Should provide the Implementation to initialize the Task Helper for executing the Fault, if the existing Task id also provided. This method will be used for executing the Remediation on a Task if the Remediation is available. This initialization is not used for task rerun or the Re-trigger.

​

**`public void executeTask(Task task) throws MangleException;`**

Provide the Implementation for execution steps required in addition to Implementation available in `AbstractRemoteCommandExecutionTaskHelper`. Plugin developer can use this interface to invoke his own implementation of Helpers for supporting his Fault across multiple endpoints supported in mangle.

​

**`protected ICommandExecutor getExecutor(Task task) throws MangleException;`**

Provide the Implementation for defining the Executor required for the Fault Execution. Mangle provide a default implementation of a executor for each Supported Endpoint. The Plugin user is free to use his own executor as long as he is implementing the resource as per the interface `ICommandExecutor` available at package com.vmware.mangle.utils;

​

**`protected void checkTaskSpecificPrerequisites(Task task) throws MangleException;`**

Provide the Implementation if the Fault being developed expect the test machine to be satisfying a condition for the execution. This step is separated from the Fault execution as Mangle wants to make sure the Fault execution or Remediation will not leave the user environment in a irrecoverable state due to execution of them in a non-perquisite satisfying machine.

​

**`protected void prepareEndpoint(Task task, List listOfFaultInjectionScripts) throws MangleException;`** Provide the Implementation if the Fault execution needs certain changes to the Test Machine before execution. Examples are Copying a binary file required to execute a fault. This step is optional for user as the predefined implementation already copies the files returned by `listFaultInjectionScripts()` to the remote machine.

​

**`public String getDescription(Task task);`**

Provide Implementation to generate description for Fault based on user inputs to help him to identify the task in future through the description. A generic implementation is already available at TaskDescriptionUtils.getDescription\(task\).

​

**`public List listFaultInjectionScripts(Task task);`**

Provide a implementation that return details of the support scrips to be copied to test machine required for executing the fault getting implemented.

​

Task-Extension Deep Dive: An example is available as `HelloManglePluginTaskHelper` at package com.vmware.mangle.plugin.tasks.impl of mangle-plugin-skeleton. This task Helper is an implementation of `AbstractRemoteCommandExecutionTaskHelper`. The implementation of `AbstractRemoteCommandExecutionTaskHelper` is only expected to provide the implementation for below methods:

​

**`public Task init(T faultSpec) throws MangleException ;`**

Should provide the Implementation to initialize the Task Helper for executing the Fault. And the commands required for injection/remediation of the Fault are expected to be provided here. More details on the model for providing the Command Information is explained later.

​

**`public Task init(T taskData, String injectedTaskId) throws MangleException;`**

Should provide the Implementation to initialize the Task Helper for executing the Fault, if the existing Task id also provided. This method will be used for executing the Remediation on a Task if the Remediation is available. This initialization is not used for task rerun or the Re-trigger.

​

**`public void executeTask(Task task) throws MangleException;`**

Provide the Implementation for execution steps required in addition to Implementation available in `AbstractRemoteCommandExecutionTaskHelper`. Plugin developer can use this interface to invoke his own implementation of Helpers for supporting his Fault across multiple endpoints supported in mangle.

​

**`protected ICommandExecutor getExecutor(Task task) throws MangleException;`**

Provide the Implementation for defining the Executor required for the Fault Execution. Mangle provide a default implementation of a executor for each Supported Endpoint. The Plugin user should use appropriate executor as per the endpoint provided as the target. Below is the Mapping of Executors to their Endpoint Types.

1. REMOTE\_MACHINE – SSHUtils
2. DOCKER - DockerCommandUtils
3. AWS - AWSCommandExecutor
4. K8s - KubernetesCommandLineClient
5. vCENTER - VCenterCommandExecutor

EndpointClientFactory class of mangle-task-framework can be used for initializing the appropriate Executor for Injecting the Fault as per user request.

All these executors expect the user to provide a command to be executed on the target machine with associated meta data to mark if it is executed successfully.

​

**`protected void checkTaskSpecificPrerequisites(Task task) throws MangleException;`**

Provide the Implementation if the Fault being developed expect the test machine to be satisfying a condition for the execution. This step is separated from the Fault execution as Mangle wants to make sure the Fault execution or Remediation will not leave the user environment in a irrecoverable state due to execution of them in a non-perquisite satisfying machine.

​

**`protected void prepareEndpoint(Task task, List listOfFaultInjectionScripts) throws MangleException;`** Provide the Implementation if the Fault execution needs certain changes to the Test Machine before execution. Examples are Copying a binary file required to execute a fault. This step is optional for user as the predefined implementation already copies the files returned by `listFaultInjectionScripts()` to the remote machine.

​

**`public String getDescription(Task task);`**

Provide Implementation to generate description for Fault based on user inputs to help him to identify the task in future through the description. A generic implementation is already available at `TaskDescriptionUtils.getDescription(task)`.

​

**`public List listFaultInjectionScripts(Task task);`**

Provide an implementation that return details of the support scrips to be copied to test machine required for executing the fault getting implemented. The support files can be any file required to be placed in the target in order to execute the developed fault. All the out of the box executors is capable of copying files to the corresponding targeted endpoint and the process completes automatically by default implementation of the `AbstractRemoteCommandExecutionTaskHelper`, provide that the names of the files are returned through `listFaultInjectionScripts()` implementation.

​

**`private List getInjectionCommandInfoList(T faultSpec) {}`**

Provide the commands to be executed for the Fault to be Injected. The commands should be provided as List. The fields and descriptions for the CommandInfo Fields.

1. `private String command;` String value of the actual command with references to members in pool of variables will be available to executor during command execution. The types and the accessing mechanism are explained in below section.
2. `private boolean ignoreExitValueCheck;` Boolean value to find if a command execution result should consider the return value of the command execution. Can be given false where there can be possibility that the command execution need not be resulted in only success return value, but it will be based on the command output.
3. `private List expectedCommandOutputList;` List of patterns to be provided to validate a command execution output to consider if the execution is success. The relation among the patterns verification is defaulted to logical ‘or’.
4. `private int noOfRetries;` Retries to be attempted by the executor before marking the command execution as a Failure.
5. `private int retryInterval;` Interval in seconds between any two attempts of a command execution incase of execution failures and opted for retry attempts.
6. `private int timeout;` Timeout interval in milliseconds to consider a command execution failure if the response was not received by the executor from the target.
7. `private Map knownFailureMap;` Mapping of Patterns to be looked for in the command execution output, to provide easier troubleshooting messages to user by masking stack traces in the result.
8. `private List commandOutputProcessingInfoList;` Explained in detailed below.

   ​

   `public class CommandOutputProcessingInfo`

   Fields are

   1. `private String regExpression;`

      Regular Expression Pattern to be used to collect an crucial information from current command’s execution to make it available throughout the Fault execution.

   2. `private String extractedPropertyName;`

      Name should be given to the collected information using the pattern given as regExpression

   ​

   **Types of Variables and Their Usage:**

   The information provided by the user or collected during the runtime of Fault are made available to command executor as below types of Variables.

   1. `TaskTroubleShootingInfo` of the Task holds the extracted information from the command execution Output.
   2. args field of `CommandExecutionFaultSpec` available as `taskData` in Task holds the data received from the user as args.
   3. `$FI_ADD_INFO_FieldName` can be used to refer to variables from `TaskTroubleShootingInfo`
   4. `$FI_ARG_Fieldname` can be used to refer to variables from args.
   5. `$FI_STACK` can be used to refer to the output of the previous command.

   ​

9. `private List getRemediationCommandInfoList(T faultSpec) {}`

   Provide the commands to for remediating the fault already Injected. The semantics of `CommandInfo` is same as it described in the previous section. The args and `TaskTroubleShootingInfo` collected during the injection will be available during the execution of remediation as well. Hence the dependency data from injection task can be passed to remediation by using the References in the commands.

