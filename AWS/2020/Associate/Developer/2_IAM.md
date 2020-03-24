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

### To create a user:
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

7. Click on create a group and give our new group a name e.g. SysAdmins
8. Add the policies you want to the group

Remember, policies define permissions that will be granted to the group, you'll see that alongside some of the policy names are "Type". Some of these are AWS managed for certain roles they expected to exist such as "Job Functions" or sometimes they're focused around services in which case they'll be "AWS Managed"

If you click on an individual role or policy's dropdown tab, you can see a JSON representation of the policy which outlines things like, what actions it allows, what effects it uses and what resources it affects

9. Press review which will create our group and add the user to the group then display a page that outline the summary of what we're about to create
10. Press create user

**IMPORTANT:** When you create a user with programmatic access, on the created page for the user it will show you the *Access Key ID* and *Secret Access Key* **ONCE**. If you lose these you will have to delete the user and re-create it therefore its reccomended to press the *"Download .csv"* button above the summary to get a file copy of this output

For standard console access, we would simply log into the AWS Web Console as we did before but with our newly created username/password combination

### To create a group and add existing users:

#### Creating a group
1. Click groups on the left hand side
2. Click "Create New Group"
3. Give your group a unique name within your account
4. Attach policies to give the group permissions to perform actions on resources
5. Press "Create Group"

**Remember:** When assigning permissions to users and groups, always adhere to the principle of least privalege, if a user doesn't need permission to perform their role you should always remove them

#### Assigning a user to a group
1. Go to the users tab on the left hand side
2. Select the user from the list that we would like to add to the group
3. Go to the groups tab on the name
4. Click the "Add user to groups" button
5. Pick the group/s that we would like to add this user to
6. Click "Add to groups"

#### Removing a user from a group
1. Go to the users tab on the left hand side
2. Select the user from the list that we would like to remove from the group
3. Go to the groups tab on the name
4. Press the *X* next to the group name that you wish to remove them from
5. On the confirmation prompt select "Remove from group"

#### Adding permissions directly to a user (NOT RECCOMMENDED)
1. Go to the users tab on the left hand side
2. Select the user from the list that we would like to modify permissions for
3. Go to the Permissions tab
4. Select "Add Permissions"
5. Select "Attach existing policies directly" on the grant permissions page
6. Select the policies to add directly and click "Next" to go to the review page
7. Click "Add Permissions"

### Setting an IAM Password Policy
This is one of the security recommendations that will show on the security status window when you visit the IAM splash page in the console, this is an *account wide baseline standard for password security in IAM* meaning that people will no longer be able to create poor passwords like "password" or "password123"

Click on the "Manage Password Policy" button on the reccomendation to go to the password policy page, this page then contains various restrictions that we can apply that put minimum standard on a users password such as:
* Minimum length
* Requires numbers/uppercase/lowercase/symbols
* Allow them to change their own password
* Force a password rotation policy
* Stop users reusing passwords
* Force admins to reset a password after it expires

After you've decided on what rules you'd like to apply to your password policy click "Apply Password Policy" to save it

### Creating Roles
IAM roles are a way to grant permissions to **entities** that you trust *(Users are for people, roles are for things)*. For example: Application code running that need to access and AWS service, IAM users in another account, AWS service accessing other AWS services, Federated users

**IAM Roles issue keys that are very short duration, they are a more secure way to grant access than private keys**

1. Go to the role tab on the left hand side
2. Click "Create Role"
3. Select the AWS service we're creating a role for
4. Choose the AWS service that the role will use
5. Press "Next"
6. Attach policies to the role as we did with users and groups
7. Click "Next"
8. Give our role a name, and optionally a description as well that describes its purpose
9. Click "Create Role"

## IAM Summary

Identity Access Management consists of the following:
* Users - (People)
* Groups - (Sets of people that allow us to manage policies collectively)
* Roles - (Entities like code we've written that we need to allow access to services)
* Policies - (The documents that define what actions can be taken with what services)

Users inside a group implicitly inherit that groups permissions, when they're removed from the group they lose the implicit permmissions

Groups are always a preferable alternative to applying policies to users directly, this is because it can be easy to forget to remove certain permissions from users directly when they're done which leads to users having adhoc permissions to things they most likely don't need anymore

**IAM is global not regional** so unlike EC2 where we create a virtual machine in a region, an IAM user created in Japan is valid in London for example

The **Root account** is the account you log in as when you first sign up with AWS, you should avoid using this and instead create sub-users to do work instead as this root account has *complete admin access*

When you create a new user is has **no permissions to anything** and you must assign them permissions either directly or by adding them to a relevant group

When users sign up for programmatic access they get:
* An Access Key ID
* A Secret Access Key

These are shown **once** so its reccommended to save them as a file when they're shown, these cannot be used to log into the AWS Web Console and instead are used for API access, if you want to log into the Web Console use the Username/Password combination instead

It's highly reccomended to set up MFA(Multi-Factor Authentication) on your root account as its a high value target

You can create and customize password rotation policies and its reccomended that you have atleast a basic one in place