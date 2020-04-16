# AWS Certified Solutions Architect

## Identity Access Management (IAM)

*What is IAM?* IAM lets you manage users and their level of access to the AWS console, its important to know because *IAM lets you set up users, groups, permissions and roles* everything that you would need to administrate an entire company if you consider an AWS account as being for an entire company

IAM gives you:
* Centralized control
* Shared access
* Granular permissions
* Identity Federation with AD, Facebook etc.
* Multi factor auth
* Temporary access where necessary
* Setup password rotation policy 
* Integrates with other AWS services
* Supports PCI DSS compliance

### Key Terminology

**Users** - The end user, they can be people, employees, etc.

**Groups** - A collection of users, *each user in the group inherits the permissions of the group*, for example a user who is put into the administrator group inherits all of the permissions of the administrator group

**Policies** - Consist of a number of *policy documents* and can be assigned to a User/Role/Group

**Policy Documents** - A number of rules defined in JSON around what actions are permitted

**Roles** - Similar to a group, however *roles are considered to be temporary* for example to access a room you might get a special badge from the security desk, but you would return it when you have finished your task

**Groups define who a group is, Roles define what extra responsibilities an entity can temporarily assume** roles are typically assigned to services for example you might assign a role to an EC2 instance to allow it to talk to S3 to get some files you care about

## Lab

Remember, IAM is one of *the most* important AWS services as it allows you to control access to other parts of AWS

IAM is under the *Security, Identity and Compliance* list under the AWS console, at the top of the splash screen for IAM you'll see a link underneath "IAM users sign-in link" *this is the link you would distribute to your end users* when they've got IAM accounts set up to sign into

If you want, you can alias ths sign in link so that instead of the account number at the start you've got a friendlier name but these aliases must be globally unique among everyone

The first thing you should do is **activate MFA on the root account**

**What is a root account?** The root account is the account that you sign into AWS with your email and password, this account has *the most* privalege of any user account level and is able to do absolutely anything inside of AWS

### Setting up MFA for an IAM User

1. Go to the IAM service in the AWS Console
2. Select *Users* in the left hand menu
3. Select the user from the list that you want to add MFA to
4. Go to the *Security Credentials* tab
5. Click on *Assigned MFA Device* and follow the prompts to add a new MFA device
6. Sign out and sign back in to test the setup


**It's a good idea to take a photo of the MFA QR code before you scan it** this means that in the event that you lose the authentication device, you can add another device using the same QR code

### Best Practices

In general, you dont want *anyone* to log in with the root account, instead you would create individual users, groups and roles for that persons requirements with *the principal of least privalege* this means that in the event that a persons account is compromised or intentionally intends to do damage, the scope of the damage that can be inflicted is reduced

You'll notice that in the top right hand corner where the region normally is, when you're inside IAM its listed as global. This is because **any Identity Access Management changes are done on a global basis** they are *not* region specific, if you create a user in Tokyo you don't then have to create the same user in London

On the IAM splash screen you'll see a *Security Status* list with a number of flags, these are AWS recommendations for security that you should follow, ideally you'll want a list of green ticks

### Creating an IAM User

1. Go to the IAM service in the AWS Console
2. Go to the *Users* tab on the left hand side menu
3. Click the Blue *"Add User"* button at the top of the screen
4. Add a username for the user 
5. Select the appropriate AWS Access Types for the user, this can be either or both but not none
6. Its recommended that you let the console autogenerate the password and *force* the user to reset their password on initial login
7. Add permissions to the user, either through groups or direct policies
8. Click next to create the user
9. The user credentials including secret and password will be displayed - Make a note of these **as they will be displayed only once, after which they are unrecoverable, if forgotten you will need to delete and re-create the acccount**

If you want to see the definition of the policy, you can just click the little drop-down arrow next to it in the list, it will then show you the full JSON definition of that policy

Power User Access role - Allows access to all AWS services **but not** management of groups and users in IAM, this prevents them from creating or modifying their own or other accounts but allows complete day to day usage, this is basically one step down from a complete administrator of the AWS account

#### Adding Permissions to a user

**What are the options for adding permissions to a new user?**
* Add the user to a group - If we don't have any groups created it will prompt us to create one, this allows us to better manage access by grouping users together into access levels
* Copy permissions from an existing user - Lets us directly copy the permissions from an existing user onto this one, in this scenario **the policies are directly applied to the user**
* Attach existing policies directly - This is similar to the copy in that permissions are directly applied to the user, but an open buffet is provided where we can pick and choose any policies available in the system to apply to this user

The **recommended** approach is to manage your user permissions through groups, the reason for this is that if you have 200 users in a system its easier to have a consolidated approach to security by adding/removing permissions to a group of users in contrast to comparing the individual policy lists of potentially thousands of users

#### Policy Types

**What are the types of policies?** 
There are a number of different policy types in AWS, if next to the policy name there is a small amazon box symbol then it means that this policy is generated and managed by the system 
* Job Function - These are high level policy groups that are typically associated with a job role, for example a billing job function policy might have complete visibility over costs but none everywhere else, a database administrator job role might have full control over databases but no visibility of costs etc.
* AWS Managed - These have finer grained control than job roles, they typically take the form of full/read/write access to individual services 
* Beskpoke - These are policies that you will create, for example you might have a service that you only want to provide specific read access to a single database, or connect permissions to a single EC2 instance

**Bespoke roles are recommended for deployed services, as they should only have access to the things they absolutely need and nothing more. Job Functions can be useful for end users as a starting point, but transition to Bespoke roles over time for the same reason**

#### User Access Types

**What are the AWS Access Types?**
* Programmatic Access - Provides an access id and secret that can be supplied during calls to the AWS SDK to programatically create, manage and use resources
* AWS Management Console Access - The web UI interface that provides a GUI and an overview of AWS services and the management of them

#### Applying an IAM Password Policy

1. Under Account Settings on the left hand tab
2. Scroll down to the *"Password Policy*" section
3. Select details of the password policy such as minimum length and complexity requirements
4. Click apply to apply the password policy to all users

#### Resetting User Passwords

While the password and API secret are never displayed again after the intial creation, we can regenerate passwords for users incase they forget them, to do this:
1. Go to the Users tab on the left hand side
2. Select the User to reset the password for
3. Go to the security tab
4. Under *"Console Password"* click Manage
5. We can then Enable/Disable access, set a new password or require the user to reset their password the next time they log in

On the same Security Credentials page we also can see the Access Keys assigned to this user, including **Creating new access keys and making existing keys inactive/removing them**, this is important in the case that keys are compromised that we quickly mark them as inactive so they can't be used

#### Creating Roles

Roles are a way for one AWS Service to use another AWS service, to create a role:

1. Select Roles in the left hand tab
2. We then have a prompt of *"What service will use this role*" select the appropriate service
3. Select an existing policy or policies to attach, or create your own
4. Give the role a unique name and a description
5. Click create

This role can then be assigned to servies that you create

### What We learned

* IAM is global - Its not region specific
* The root email account has complete access
* New IAM users have no permissions by default
* New users get password, access key and secret on creation, but it is only shown once
* Always set up Multi Factor Authenticator
* Setup a password rotation policy

## Creating a Billing Alarm

Setting up a billing alarm is important as this will notify you when your account reaches a certain spending threshold to prevent you from overspending by accident



To set an account Username:
1. Click on the name of the account in the top right hand corner next to the region
2. Select "My Account" from the dropdown
3. Select "Edit" next to Account Settings
4. Put in a desired username, unlike the Login link alias this doesn't need to be globally unique so it can be a vanity name

To create a billing alarm:
1. Click on the name of the account in the top right hand corner next to the region
2. Select "My Account" from the dropdown
3. Select "Dashboard" from the left hand menu
4. Scroll to the bottom and select "Set your first budget" from the prompts in "Alerts & Notifications"

**OR**

To create a billing alarm directly through CloudWatch:
1. Go to CloudWatch in the console services menu (Under Management Tools)
2. Under Alarms tab on the left, click billing
3. Select *"Create Alarm"*

Either way, you will be prompted to enter a threshold amount and an email to send the notification to, you may be prompted to confirm the email address to ensure that you will recieve delivery of the amount notification

## Summary

IAM consists of:
* Users - the end user accounts
* Groups - Sets of policies to be applied to users
* Roles - Sets of policies to be applied to services
* Policies - Descriptions of rules around permissions for access to specific services

Policies are written in JSON and describe the version, the effect they have, the actions they permit and on what resources they're implemented against

IAM is a global services, its not related to a specific region

The original email account used to create the AWS account is known as the *root account* this has complete administrator access over everything

New permissions don't have any permissions when they're first created

Users can be created to have either only access to the web console, API access only, or both

When creating a user, the password and secret are only displayed **once** the only way to recover it after this if forgotten is to regenerate a new one or reset it to a known value

Set up MFA on your root account

You can create your own password policy