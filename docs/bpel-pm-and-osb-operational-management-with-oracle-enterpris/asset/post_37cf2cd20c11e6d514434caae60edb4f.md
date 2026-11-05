
5. Note the steps underlying the procedure:

![](img/7529bbd615e859608c21b965162348b2_99_0.png)

## Scheduling BPEL suitcase deployment

The primary task for the administrator is to schedule the out-of-the-box deployment procedure to run once, or to a schedule.

- 1. Continuing from the previous step, click on the **Schedule Deployment** button.
- 2. In the **Source Selection** step:
  - Click on **Add** to select the source suitcase bundles.

> These bundles are stored in the Grid Control Software Library before actual deployment.

## BPEL Deployment Automation

- From the existing suitcases, you can select one or more to deploy. For example, select **ParallelFlow**, **UnitedLoan**, and **AmericanLoan**.
- Click on **Select** to continue.

![](img/7529bbd615e859608c21b965162348b2_100_0.png)

- Use the **Move Up** and the **Move Down** buttons to order the deployment.

> Order of deployment is essential in a BPEL environment, where partner links are typically deployed before the BPEL Process that invokes them.

- Click on **Next** to continue.

3. In the **Target Selection** step, follow these steps as shown in the next screenshot:

- Click on **Add** to select one or more targets from the list of managed BPEL targets; for example, select **gcsoa.us.oracle.com_bpel**.
- Click on **Select** to continue.

- Select **default** from the **BPEL Domain** dropdown for each BPEL target.
- Click on **Next** to proceed, as shown in the following screenshot:

![](img/7529bbd615e859608c21b965162348b2_101_0.png)

4. In the **Credentials** step, follow these steps as shown in the next screenshot:

- Provide the credentials for the host under **Application Server Credentials**; that is, **Username** as **oracle** and **Password** as *welcome1*.
- Click on **Next** to proceed.
- Under **BPEL Process Manager Credentials**, provide domain level access; that is, **Username** as **oc4jadmin** and **Password** as *welcome1*.

These credentials for host and BPEL Process Manager can be stored in the Grid Control "Preferred Credentials" page and reused.

![](img/7529bbd615e859608c21b965162348b2_102_0.png)

Click on Next.

5. In the Schedule step, follow these steps as shown in the next screenshot:

- Click on One Time (Later) and select a Start Date (for example, June 23, 2008) and Start Time can be anything from a couple of minutes later.
- Enter BPEL Process Provisioning for the Instance Name field.
- Click on Next to proceed, as shown in the next screenshot:

Deployment can be scheduled immediately or for a future time, typically during a customer maintenance window. The Grid Control job system takes care of the job scheduling and management.

![](img/7529bbd615e859608c21b965162348b2_103_0.png)

- 6. In the Review step:
  - Review all the information before submitting a deployment job.

Click on **Finish** when you're ready, as shown in the following screenshot:

![](img/7529bbd615e859608c21b965162348b2_104_0.png)

## Viewing the status of the scheduled deployment

Grid Control provides means to observe the overall deployment status and individual step status from its jobs page.

- 1. Continuing from the previous step, find the **BPEL Process Provisioning** job from the **Procedure Completion Status** tab.
- 2. Click on the **BPEL Process Provisioning** job to view the details, as shown in the following screenshot:

![](img/7529bbd615e859608c21b965162348b2_105_0.png)

The job status page shows the details of the scheduled job, including elapsed time and error messages if any.

- 3. Use the **Refresh** button on the top right to see the progress.
- 4. Wait for the job to finish successfully.
- 5. Note the **Status** and **Completed Date** values.

## Viewing the deployed processes

Once the deployment has completed successfully, you can verify the new process on the BPEL console:

- 1. Navigate to the BPEL target home page.
- 2. Click on the **Processes** tab.
- 3. Click on the **Refresh** button on the top right.
- 4. Note the newly deployed processes in the list.

## Summary

Deployment is a critical task for operational administrators. With the Grid Control Software Library and deployment procedure capabilities, administrators can manage BPEL suitcases, and schedule and automate deployment.

In this chapter, we discussed how to utilize the out-of-the-box deployment functionality with Grid Control. The administrator can view the deployment procedures, pick the right one, schedule the deploy, and observe the deploy taking place. This is a powerful automation framework that can be utilized for deploying repeatedly and on multiple environments at the same time. There are other variations of these deployments that might be useful, such as running a custom script after the end of the deployment.

With Grid Control release 10gR5, the deployment plan (introduced with BPEL 10.1.3.4) methodology is also supported. In other words, a deployment plan file with details of partner links for test, stage, and production environments can be uploaded to the Software Library. This file can be used while scheduling deployment and the partner link changes will automatically be done. It is also common for customers to create a copy of the out-of-box deployment procedure, and add a few custom steps with home-grown scripts that need to be executed before or after deployment.

The next few chapters will walkthrough hands-on exercises to manage configurations for the BPEL environment.

## 8 BPEL Configuration Management

Managing the configurations of software and hardware assets in an enterprise is important for several reasons. "Knowing what you have" enables IT managers to make decisions about future investments and retiring old assets, standardize existing assets, manage change across the enterprise, and troubleshoot performance problems related to configurations.

Most configuration management solutions today are compliant with the best practices laid out by the **IT Infrastructure Library (ITIL)** framework. All the assets are stored in a central repository called the **Configuration Management Database (CMDB)**. Configuration parameters such as `init.ora` parameters for a database, or `startup` parameters for a WebLogic application server, or port numbers for an HTTP server are stored in the CMDB. Any changes to these parameters are also tracked. It then becomes easy to audit the changes for a specific asset, compare assets to each other (for example database1 and database2), compare different versions of the same asset, save a reference configuration to the CMDB, and use that to standardize across the enterprise. This chapter talks about managing configurations for a BPEL environment. Specifically this chapter talks about:

- Challenges
- Solution
- Step-by-step exercises:
  - Navigating to BPEL target configuration management:
    - Viewing the last collected configuration
    - Saving a configuration snapshot to the repository
- Navigating to Oracle Application Server configuration management:
  - Viewing the last collected configuration
  - Saving a configuration snapshot to the repository
- Making changes to the BPEL environment
- Making changes to the Oracle Application Server environment
- Comparing the current BPEL configuration with a saved baseline
- Comparing BPEL process versions for the same process
- Comparing the current Oracle Application Server configuration with a saved baseline
- Viewing application server target comparison results with a saved baseline

## Challenges

It is difficult to track the configurations of all IT systems. Frequently, IT departments resort to Excel spreadsheets to store critical IT system information. This makes an IT department slow in making changes, measuring impact, and identifying performance problems due to incorrect configuration, and increases the costs of managing a data center. With a BPEL environment, it is difficult to baseline a working configuration and compare it with non-working ones. There is no good way to compare successive BPEL Process versions before rolling them out into production.

## Solution

With Grid Control, key configuration metrics for BPEL PM server and processes are collected and stored in the Grid Control configuration management database. This is the same database repository that houses the monitoring information. Administrators can view the historic configuration changes across the BPEL environment. They can also baseline a working configuration by saving it in the repository. A BPEL PM server and domain's configuration parameters can be compared with other server and domains. Finally, BPEL Process versions can also be compared to observe the changes across successive deployments.
