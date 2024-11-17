2024-08-20 12:16

Status:

Tags: [[AWS]]

# AWS Setup
## Root vs IAM
AWS has two different logins: Root and IAM (Identity access management)
- The root account holds special privileges
	- Create first IAM user
	- Change root password
	- Change support plan (how much money we're paying Amazon)
	- Close the account
- IAM is a limited view that doesn't have access to root level controls
	- Almost always use IAM account when working with AWS
- Physical MFA key may be needed to login to a root account
	- A literal physical device to make your root account as secure as possible

## Best Practices for Root User
**Limit Access:** Only admin users should have access to the root account
**Limit Use:** Only use the root account for tasks that require the root account 
**Limit Workload**: Only deploy cloud resources/data to the root account if necessary
**Delegate**: Certain root privileges can be given to specific IAM users. Decentralize root control by giving these permissions to different admins
- Ex: You can control whether IAM users can see billing information
### [[Amazon Resource Names]]
- Commonly shorthanded to ARN
- Used to identify resources to the cloud infrastructure
	- Each component will have a unique ID
## IAM Users
The root account has access to the IAM service, allowing them to control:
- **User Groups**: Create a group with specific permissions, any IAM user added to that group will inherit those permissions
- **Policies**: Allow you to control the specific permissions of a IAM user or group
	- The "AdministratorAccess" policy has all the basic admin permissions
	- Can combine policies together to create custom permission groups for specific users/teams
- **Creating Users**: 
#### Best Practices
- **Least-Privilege Permissions:** Only give teams the minimum permissions they need to do their job, any more create security issues

## References
