---
layout: post
title: Introduction to AWS Identity and Access Management (IAM)
subtitle: How to assign access to specific resources using IAM-managed policies (user and group management)
categories: Training Website 
tags: [AWS, IAM Users, IAM Groups]
---

## Introduction to AWS Identity and Access Management (IAM)

# Introduction
AWS Identity and Access Management (IAM) is a service that allows AWS customers to manage user access and permissions for their accounts, as Ill as available APIs/services within AWS. IAM can manage users and security credentials (such as API access keys), and allow users to access AWS resources.
In this lab, I will walk through the foundations of IAM. I'll focus on user and group management as well as how to assign access to specific resources using IAM-managed policies. I'll learn how to find the login URL where AWS users can log in to their account and explore this from a real-world use case perspective.

# Solution
Log in to the AWS Management Console using the credentials provided on the lab instructions page. Make sure I're using the us-east-1 region.

## Environment Walkthrough
# Explore the Users
 1.	Navigate to **IAM**.
 2.	From the left-hand menu, click **Users**.
 3.	Click **user-1**.
 4.	Select the **Permissions and Groups** tabs, where I'll see user-1 does not have any permissions assigned to it and does not belong to any groups.
 5.	Select the **Security credentials** tab, where I would see user access keys, SSH public keys, and HTTPS Git credentials for AWS CodeCommit.
 6.	Select the **Access Advisor** tab to see which services the user has accessed and when.
 7.	At the top, under **Summary**, observe the user’s ARN (Amazon Resource Name), path, and creation time.

# Explore the Groups
There are 3 groups I're going to focus on:
 - EC2-Admin: Provides permissions to view, start, and stop EC2 instances
 - EC2-Support: Provides read-only access to EC2
 - S3-Support: Provides read-only access to S3
**Note**: There are 2 different kinds of policies for these groups:
   - **Managed policies**: Policies shared among users and/or groups that are pre-built either by AWS or an administrator within the AWS account. When it's updated, the changes to this policy are immediately applied for all users and groups to which it's attached.
   - **Inline policies**: Policies assigned to just one user or group that are typically used in one-off situations.
 1.	In the left-hand menu, click **User groups**.
 2.	Click **EC2-Admin**.
 3.	Click **Permissions**, where I'll see EC2-Admin has an inline policy with a set of permissions associated with it.
 4.	Click the plus-sign icon next to the ec2-admin policy to view the policy and see the actions the group is alloId to take (and which resources the action can be taken on) or if it has read-only access.
 **Note**: From this policy, I have permission to view, start, and stop EC2 instances on all resources, view elastic load balancers, list metrics, get metric statistics, and describe metrics (which our CloudWatch metrics automatically configured with our EC2 instance). The same permissions apply to our Auto Scaling service.
 5.	Click **Cancel**.
 6.	In the left-hand menu, click **User groups**.
 7.	Click **EC2-Support**.
 8.	Click **Permissions**, where I'll see it has a managed policy created by AWS.
 9.	Click the plus-sign icon next to the AmazonEC2ReadOnlyAccess policy.
 **Note**: This group can describe EC2 instances, elastic load balancers, CloudWatch metrics, and our Auto Scaling configurations. It doesn't allow us to stop, start, or create EC2 instances. It's a read-only policy, meaning I can view what's happening inside EC2, but I can't make changes to the resource.
 10. Click **Cancel**.
 11. In the left-hand menu, click **User groups**.
 12. Click **S3-Support**.
 13. Click **Permissions**. Our S3-Support group is only alloId read-only access.
 14. Click the plus-sign icon next to the AmazonS3ReadOnlyAccess policy, where I'll see the Get and List actions that allow us to view the S3 bucket and the objects in it.
 15.	Click **Cancel**.

# Add the Users to the Proper Groups
 1.	In the left-hand menu, click **User groups**.
 2.	Click **S3-Support**.
 3.	In the **Users** section, click **Add users**.
 4.	Select user-1, and click **Add users**.
 5.	in the left-hand menu, click **User groups**.
 6.	Click **EC2-Support**.
 7.	In the **Users** section, click **Add users**.
 8.	Select user-2, and click **Add users**.
 9.	In the left-hand menu, click **User groups**.
 10. Click **EC2-Admin**.
 11. In the **Users** section, click **Add users**.
 12. Select user-3, and click **Add users**.
 13. In the left-hand menu, click **Users**.
 14. Click **user-3**.
 15. Under **Permissions**, expand the ec2-admin policy.
 16. Click **Edit policy**.
 17. Click the plus-sign icon next to the ec2-admin policy.
 18. Review the policy, but do not make any changes.

# Use the IAM Sign-In Link to Sign In as a User
 1.	Click **Dashboard** in the left-hand menu.
 2.	Copy the sign-in URL located on the right, under **AWS Account**.
 3.	In a new browser tab, navigate to the URL.

# user-1
 1.	Using the credentials provided in the lab overview, log in to the user-1 account.
 2.	Navigate to **S3**.
 3.	Click **Create bucket**.
 4.	Enter a globally unique bucket name.
 5.	Click **Create bucket**.
    - I should receive an "Access Denied" error, since this user cannot create buckets in S3.
 6.	Navigate to **EC2**.
    - I won't be able to see any instances.
 7.	Click on the username in the top-right corner of the page.
**Note**: Before clicking **Sign Out** I will need to copy the Account ID in the sign out box to log in to user-2 and user-3 as the ID is not persistent after logging out.
 8.	Click **Sign Out**.

# user-2
 1.	Using the credentials provided in the lab overview, log in to the user-2 account.
 2.	Navigate to **EC2** > **Instances (running)**.
    - This time, I should be able to view the running instances.
 3.	Select the running instance.
 4.	Click **Instance state** > **Stop instance** > **Stop**.
    - I should see an error message, since this user doesn't have permission to stop instances.
 5.	Navigate to **S3**.
    - I should see an error message that I don't have permission to list buckets.
 6.	Click on the username in the top-right corner of the page.
**Note**: Before clicking **Sign Out** I will need to copy the Account ID in the sign out box to log in to user-2 and user-3 as the ID is not persistent after logging out.
 7.	Click **Sign Out**.

# user-3
 1.	Using the credentials provided in the lab overview, log in to the user-3 account.
 2.	Navigate to **EC2 > Instances (running)**.
 3.	Select the running instance.
 4.	Click **Instance state > Stop instance > Stop**.
    - I should see it enters the **Stopping** state.
 5.	Refresh the table.
 6.	Click **Clear filters**.
 7.	Once the instance has stopped successfully, select it and click **Instance state > Start instance**.
 8.	Refresh the table to make sure it enters the **Running** state.

# Conclusion
Congratulations — That’s how I've completed this hands-on lab!
