# Requests and Reports

Request & Reports page provides insight to the tasks running during fault execution, fault remediation and triggering of scheduled jobs. Mangle creates tasks that transition to one of the stages : NOT STARTED, IN\_PROGRESS, COMPLETED, FAILED.

## Processed Requests <a id="processed-requests"></a>

It provides details of the tasks executed by Mangle.

### Important fields of Mangle tasks <a id="important-fields-of-mangle-tasks"></a>

1. Task Name: Name of the task created for any fault execution, remediation or schedule.
2. Status: Will reflect one of the Stages : NOT STARTED, IN\_PROGRESS, COMPLETED, FAILED.
3. Endpoint Name: Name of the targeted endpoint during fault execution.
4. Task Type: Type of the task executed. For eg: INJECTION or REMEDIATION
5. Task Description: You can get more details about the fault, fault parameters, endpoint targeted, targeted component within an endpoint etc form this field.
6. Start Time: Task trigger time
7. End Time: Task end time

### Supported operations for Mangle tasks <a id="supported-operations-for-mangle-tasks"></a>

Click on ![](https://firebasestorage.googleapis.com/v0/b/gitbook-28427.appspot.com/o/assets%2F-LcVKiIEQZ_SDz8uqA0g%2F-LezQBnj-1YbJVjkYcQD%2F-LezQCgsZxWwXZMn6Zkb%2Fsupportedactionsbutton.png?generation=1557989744420325&alt=media) to understand what operations are supported for a specific task.

Primarily, the operations supported are Delete, Remediate Fault and Report.

Remediate Fault option will be enabled only if the the task type is INJECTION and status is set to COMPLETED.

Delete is not supported for tasks created through scheduled jobs.

### Refreshing the Mangle task data grid <a id="refreshing-the-mangle-task-data-grid"></a>

Click on refresh icon ![](https://firebasestorage.googleapis.com/v0/b/gitbook-28427.appspot.com/o/assets%2F-LcVKiIEQZ_SDz8uqA0g%2F-Lf4WT_PD61Djy1FB7UY%2F-Lf4b-jKYwAnMpjqMxzq%2FRefreshButton.png?alt=media&token=2fbd890e-4c05-47ff-a615-4751aa1d49d1) to sync Mangle task data grid with the current status.

## Scheduled Jobs <a id="scheduled-jobs"></a>

Scheduled Jobs data grid lists all the schedules available on Mangle.

### Important fields of schedules <a id="important-fields-of-schedules"></a>

1. ID: Contains id of the schedule.
2. Job Type: Type of the schedule. For eg: CRON, SIMPLE
3. Scheduled At: Recurrence and Time at which the schedule will be triggered. If job type is CRON, it shows a cron expression and if the job type is SIMPLE, it shows the epoch time in milliseconds.
4. Status: Status of the schedule. Will reflect one of the values: INITIALIZING, CANCELLED, SCHEDULED, FINISHED, PAUSED, SCHEDULE\_FAILED

### Triggers of each schedule <a id="triggers-of-each-schedule"></a>

Click on the ID link of each schedule to view all the triggers of that schedule.

### Supported operations for Mangle schedules <a id="supported-operations-for-mangle-schedules"></a>

Click on ![](https://firebasestorage.googleapis.com/v0/b/gitbook-28427.appspot.com/o/assets%2F-LcVKiIEQZ_SDz8uqA0g%2F-LezQBnj-1YbJVjkYcQD%2F-LezQCgsZxWwXZMn6Zkb%2Fsupportedactionsbutton.png?generation=1557989744420325&alt=media) to understand what operations are supported for a Scheduled Job.

Primarily, the operations supported are Cancel, Pause, Resume, Reports, Delete, and Delete Schedule Only.

### Refreshing the schedule data grid <a id="refreshing-the-schedule-data-grid"></a>

Click on refresh icon ![](https://firebasestorage.googleapis.com/v0/b/gitbook-28427.appspot.com/o/assets%2F-LcVKiIEQZ_SDz8uqA0g%2F-Lf4WT_PD61Djy1FB7UY%2F-Lf4b-jKYwAnMpjqMxzq%2FRefreshButton.png?alt=media&token=2fbd890e-4c05-47ff-a615-4751aa1d49d1) to sync Mangle schedule data grid with the current status.

## Logs <a id="logs"></a>

Click on the Logs link to open up a browser window displaying the current Mangle application log.

## Relevant API Reference <a id="relevant-api-reference"></a>

**For access to relevant API Swagger documentation:**

Please traverse to link ![](https://firebasestorage.googleapis.com/v0/b/gitbook-28427.appspot.com/o/assets%2F-LcVKiIEQZ_SDz8uqA0g%2F-Leac8tB-KodjFhsg2Zs%2F-LeacYoaMEsURLcmrZyl%2FHelp.png?alt=media&token=e28a8f10-207c-4ae0-a803-2949ee85c749) -----&gt; API Documentation from the Mangle UI or access _https:///mangle-services/swagger-ui.html\#_/_scheduler-controller_

  ![](https://firebasestorage.googleapis.com/v0/b/gitbook-28427.appspot.com/o/assets%2F-LcVKiIEQZ_SDz8uqA0g%2F-Lf4dQH_h04c3cxlC7B2%2F-Lf4emZp1NheDJCVHY9f%2FSchedulerController.png?alt=media&token=0bdba233-8ac2-4f82-9c08-687442cc1b54)

