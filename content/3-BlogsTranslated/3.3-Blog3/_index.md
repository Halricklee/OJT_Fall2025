---
title: "Blog 3"
date: "2025-10-04"
weight: 3
chapter: false
pre: " <b> 3.3. </b> "
---
## AWS for SAP
## Automating SAP HANA DB HA Patch using SSM and nZDT

by Guilherme Sesterheim and Mohit Biyani | on 22 AUG 2025 | in Amazon EC2, SAP on AWS, Technical How-to | Permalink Share

---

### Introduction

Keeping your SAP HANA database up to date with the latest patches is crucial for maintaining security, performance, and reliability. However, traditional database patching often requires significant downtime, impacting business operations. While previous AWS guidance covered various automation approaches, this post introduces a new solution that achieves near-zero downtime for High Availability SAP HANA databases using native AWS services.

Using AWS Systems Manager and AWS CloudFormation, we’ll show you how to automate SAP HANA database patching for both Red Hat Enterprise Linux (RHEL) and SUSE Linux Enterprise Server (SLES) environments.

### Benefits of this approach

Using this AWS-native tooling approach improves SAP HANA database maintenance by integrating critical operational and security features into a unified solution. AWS Systems Manager serves as your central command center, providing real-time monitoring, detailed logging, and automated health validations throughout the update process. The solution intelligently coordinates updates between primary and secondary nodes while maintaining rollback capabilities for operational assurance. By eliminating the need for third-party tools, you not only reduce licensing costs but also benefit from native AWS service integration, including encrypted communications and comprehensive audit trails. This consolidated approach, managed through a single AWS Systems Manager console, delivers enterprise-scale database maintenance with built-in security and compliance controls.

### Pre-requisites

The following pre-requisites must be met prior to using this code for updating your HANA Database (HDB) in HA setup:

* A pre-configured SAP HANA 2.0+ database environment running in high availability mode on your Amazon EC2 instances. While we won’t cover the initial setup in this blog, we encourage the use of AWS Launch Wizard to automate the deployment and configuration of your SAP workloads such as SAP HANA, or leverage the SAP on AWS documentation if you need assistance with the configuration.
* Verify that your SAP HANA database EC2 instances are managed by AWS Systems Manager. This is essential as the automation solution leverages Systems Manager’s capabilities for seamless management and operations.
* If you’re leveraging a central or shared services account for automation (recommended AWS best practice), ensure you have the appropriate cross-account permissions configured before proceeding, refer the link for more information.
* AWS Systems Manager automation syncs Amazon S3 media files to the /tmp directory on your EC2 instance. Before running the automation, make sure you have enough storage space available in this temporary directory. The amount of space needed depends on the size of the files for the update you’re performing.
* Add a tag to your SAP HANA database EC2 instances with the key-value pair “Hostname: <hostname>”. You’ll need this hostname value when you run the solution in a later step.

### Architecture Diagram

The architecture diagram (in Image 1) illustrates the solution using AWS Systems Manager Automation Documents, an Amazon S3 bucket containing SAP HANA patch installation media, and essential parameters stored in AWS Secrets Manager. The SAP HANA workloads run on Amazon EC2 instances within your AWS account.

> Image 1 – HANA DB HA Cluster [Diagram illustrating the solution architecture]

### Preparing for the execution

1.  Setup a user account in your SAP HANA SYSTEMDB with sufficient privileges to perform database updates. This user will be referenced in the automation process, so it’s essential to verify the account has all necessary authorizations before proceeding with the upgrade. We strongly recommend following the principle of least privilege when configuring your SAP HANA database user permissions. For more details see, Create a Lesser-Privileged Database User for Update.

2.  Make sure your SAP HANA database instances have the necessary permissions to access the Amazon S3 bucket containing your SAP HANA patch installation media. For detailed instructions on creating and attaching IAM policies to EC2 instances, see working with IAM Roles in the AWS IAM User Guide. Snippet 1 is a sample policy which demonstrates how to grant a specific IAM role within your account the necessary permissions to download files from your S3 bucket. Make sure you change the bucket and role names (highlighted in yellow) according to your environment.

    ```json
    {
        "Version": "2012-10-17",
        "Statement": [
            {
                "Sid": "AddPerm",
                "Effect": "Allow",
                "Principal": {
                    "AWS": "arn:aws:iam::{account_id}:role/service-role/{ec2_role}"
                },
                "Action": [
                    "s3:GetObject",
                    "s3:GetObjectVersion",
                    "s3:ListBucket"
                ],
                "Resource": [
                    "arn:aws:s3:::{bucket_name}/*",
                    "arn:aws:s3:::{bucket_name}"
                ]
            }
        ]
    }
    ```

    > Snippet 1 – S3 Bucket Policy

3.  The automation relies on AWS Secrets Manager to retrieve the SAP HANA database credentials. AWS Secrets Manager allows you to share secrets across same/different AWS accounts. This functionality lets you centralize secret management in a single location. For additional details refer to How do I share AWS Secrets Manager secrets between AWS accounts. Make sure you have created the required secrets (sapadm user password and SYSTEM user password) in AWS Secrets Manager and configured appropriate permissions for your target AWS account to access these secrets.

4.  To enable secure cross-account access to sensitive information, the solution uses AWS Secrets Manager with AWS KMS encryption. The encrypted secrets are protected by a KMS key that must be accessible to all participating AWS accounts. For detailed guidance on configuring cross-account secrets access, please refer to the documentation.

### How to run the code

Follow the steps below to use the code contained in this GitHub repository to get your HANA Databases updated.

1.  Create the secrets required on AWS Secrets Manager on the same account you have your HANA databases. For your reference, we’ve included a sample secret for how to configure on image 2. This example demonstrates the expected format and key-value pairs needed for the automation to work correctly. Please ensure your secrets follow a similar structure while using your actual credentials.

    > Image 2 – Example secret for DB credentials 

2.  Establish an S3 bucket that will serve as the storage location for your update files.

3.  Use the CloudFormation creating a stack guide to deploy the solution. In the repository, under folder cloudformation, refer below 2 files:

    * hana_db_patch_ha_rhel – to be used for databases configured for HA in a RHEL system. This will achieve the nZDT concept explained in the Introduction.
    * hana_db_patch_ha_suse – to be used for databases configured for HA in a SUSE system. This will achieve the nZDT concept explained in the Introduction.

4.  Go to Systems Manager > Documents > select tab “Owned by me” > search for “patch” and open your applicable doc:

    > Image 3 – SSM Documents available to use [Screenshot of SSM Documents console]

5.  Select “Execute automation” on the top right corner:

    > Image 4 – SSM Execution Steps [Screenshot of the "Execute automation" button]

6.  Fill in the required input parameters:

    > Image 5 – SSM Documents Input Parameters [Screenshot of the SSM document input parameters]

7.  Scroll down and select “Execute”.

8.  Post successful completion you see a completion message as Image 6.

    > Image 6 – SSM Automation Output [Screenshot of a successful automation completion message]

### Flow of Execution

Below you find a flow chart with all the steps, and their details, performed by this automation.

> Flow Chart 1 – SSM Execution Sequence [Diagram showing the sequential flow of automation steps]

The steps shown in the ‘Flow Chart 1’ execute sequentially. If any step fails, the entire process stops immediately.

If you encounter an error, please consult the Troubleshooting section of this blog to diagnose and resolve the issue before continuing.

### Troubleshoot

To monitor & help diagnose automation issues, AWS Systems Manager maintains detailed execution logs in both EC2 instances and CloudWatch Logs. These logs capture the step-by-step progress of your automation.

#### Monitoring Automation Status:

To check the status of your Systems Manager automations:

1.  Open the AWS Systems Manager console.
2.  In the left navigation pane, choose Automation
3.  Choose Configure preferences > Executions
4.  View your automation statuses in the Automation executions section

#### Reviewing Execution Details:

The AWS Management Console lets you examine each automation execution in detail. You can:

* Navigate through individual automation steps
* Review the results of each step
* Identify any failures that occurred during the automation process

#### Troubleshooting with Logs:

There are two ways to access automation logs:

* EC2 Instance Logs
    * Path: /var/lib/amazon/ssm/{instance-id}/document/orchestration/{automation_step_execution_id}/awsrunShellScript/0.awsrunShellScript – contains detailed execution logs from the EC2 instance
    * Path: /tmp/hana/patch – contains the files used for the patch procedure
    * Path: /tmp/update – contains the credentials for running the update command
* CloudWatch Logs Integration
    * You can configure Systems Manager to send automation outputs to Amazon CloudWatch Logs
    * For setup instructions, see [Configuring Amazon CloudWatch Logs for Run Command]

### Cost Consideration

AWS Systems Manager Automation used in this HANA DB patching solution follows a pay-as-you-go model. You’re charged based on the number and duration of automation steps executed, with a generous free tier that includes:

* 100,000 automation steps per month,
* 5,000 seconds of automation execution time per month

If you’re using AWS Organizations, this free tier usage is shared across all accounts in your Consolidated Billing family. If you are running other workloads on the same account that are already using your entire free-tier, the costs associated with running this tool will stay below USD $10.

For detailed information about cost calculation, please refer AWS Systems Manger Pricing.

### Conclusion

As demonstrated in this blog post, automating the patching updates for HANA DBs can be easily achieved using AWS native tools. When you are implementing this in your environment, make sure to run first in a non-productive account before going to real business, as some minor OS versions may cause the solution to behave differently from environment to environment.

By using this solution you can standardize how the update process happens across different SAP landscapes and environments and have a single source of truth for the process.

Are you interested in learning more or maybe you would like a better understanding of how you can extend this solution for your project?

For more information, contact us at sap-on-aws@amazon.com.