# Identity Access Management

## IAM 101

IAM is all about setting up users (to both the AWS website console and programatically), and granting those users varying levels of permissions through your account - **Understanding IAM is vital as its a large part of managing AWS**

IAM gives you:
* Centralized control over your account
* Shared access to the AWS account
* Granular permission control (Differing levels of access to different users and groups of users)
* Allows for Identity Federation through known providers such as Linked in, Facebook, Google etc.
* Supports Multifactor or 2 Factor Authentication
* Can provide *temporary* access to either users or devices for time restricted tasks
* Supports password rotation policies
* Integrates with lots of different AWS services
* Supports PCI DSS Compliance

Core entities involved with IAM are:
* Users - These are the end users e.g. People logging into the console and programatically
* Groups - These are *collections* of users that have a common set of permissions for example - *Marketing Team*. This means that instead of creating and adding permissions to each member of the team, we can instead create a group and add the user to the group as well as the inverse for revoking permissions easily
* Roles - These are a mix between users and groups, you create them and assign permissions to resources to them e.g. *"You can perform read actions against my S3 bucket"* then that role can be **assumed** by other AWS services such as S3 to allow them to perform the action specified on the role
* Policy - These are documents that define one or more permissions - **These can either be attached to a group or role**

Policies are not a one to one relationship, *its possible for a user, group and role to all share the same policy*

## IAM Lab

Note: *When they launch new AWS services, they don't do it in every region at the same time, so you may find that some services are unavailable yet in certain regions*. If you do want to see/try the latest services typically **eu-east-1 (N. Virginia)** gets them first

Identity Access Management comes under "Security, Identity and Compliance" in the AWS Console, when you first access the IAM console you'll see at the top its got a "IAM Users Sign-in link" in future if you want to access the AWS console directly you can use this link as you can see inside the link *it has your AWS account number in it*

There is a Security Status splash that shows below what IAM resources you have in your account, this provides some warnings about best practices you should adhere to for your AWS account and typically has things in it like:
* Delete your root access keys
* Activate MFA on your root account - (Don't just use username/password on your root account, as your root account has access to *everything*)
* Create individual IAM users - (Create atleast one other user and log in as that user instead, we don't want everyone in the organisation logging in as an all powerful root user that can do anything - Use this when you absolutely need to)
* Use groups to assign permissions - (We don't want to assign permissions directly to users through policies, because we can forget to take away permissions when they're no longer needed, using groups is a better way of managing this - Consider how you would group up your organisation)
* Apply an IAM password policy

**It should be your priority to get all green ticks across these because it limits the security expose you have on your whole AWS account**

When setting up MFA for your root account, choose a virtual device and make sure you've got a compatible application downloaded on your phone ready e.g. Google Authenticator

To create a user:
1. Click on users on the left hand side
2. Click add user
3. Enter a username
4. Select the AWS access type

There are two types of AWS access for IAM accounts:
* Programmatic access - Provides you an access key id and secret access key, you supply these to the AWS CLI or other command line utilities that provide you the ability to access your AWS account programatically
* AWS Console Management Access - Provides the ability for the user to sign into the AWS Console Website

Using the principle of least privalege, instead of providing total access all the time *think about what type of workload the user will perform, do they need console access? If they're an end user do they need programmatic access?*

5. Select if you want to autogenerate a password or set your own
6. Toggle if you want to force a password reset or not (typically a good idea if creating users for other people)

Then we'll be asked to set permissions for the user that we've created, there are three main ways we can add permissions to a user:
* Add a user to a group - For example if we add this user to the admins group they implicitly inherit all the permissions set on that group until they're removed from it
* Copy permissions from an existing user - If we have a template user we want to copy from *try and avoid doing this, as once copied we lose the link between the users meaning that revoked permissions on one user stay on the other* this can be risky when it comes to revoking and managing permissions
* Attach existing policies directly - Similar to the copy permissions but instead setting the permissions by hand on the user, this will also drift in future

Of the three **Add user to a group is the most preferable** as groups allow you to better control which access users have and makes it easier to revoke permissions by removing them from the group *(We can also just skip and don't add any permissions for now going straight through to review)*





## IAM Summary

### AWS This Week
