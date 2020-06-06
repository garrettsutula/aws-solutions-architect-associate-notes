# Identity and Access Management
[IAM FAQ](https://aws.amazon.com/iam/faqs/)

Allows for the management of:
- Users
- Groups
- Roles
- Policies
for accessing AWS services and resources.

IAM is *universal* and does not apply to regions at this time. The *root account* is the account created when first setting up AWS and has complete administrator access. It's not a good idea to use this account and instead you should create one or more user accounts that have the Roles necessary to use AWS.

New users have no permissions when first created following the least-privilege design policy. New users are assigned an `Access Key ID` and `Secret Access Keys` when first created, these provide programmatic access but not AWS console access. You only get to view these once, if they're lost they must be regenerated. Users must be granted AWS console access (and provided a password to log in) to use the console.

Always set up Multi-factor Authentication (MFA) on the root account. You can create and customize password/password rotation policies and should follow NIST guidance for password management.

## Building Organizations
An account management service that enables you to consolidate multiple aws accounts into an organization that's centrally managed.

![aws org sample](./img/Organizations.svg)

Accounts organized into Organizational Units (OUs), pretty standard directory management.

Policies are applied at the OU or Account level.

"Paying" account is indepdendent of linked accounts, linked accounts own and manage their resources in AWS. Consolidates billing to one account, volume pricing discount benefits.
