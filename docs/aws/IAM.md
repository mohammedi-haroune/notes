# IAM
IAM allows management for users and resoruces
Policies are attached to IAM identites (Users, Groups and Roles)
- Common usecase: users belong to a group. Polies are attached to roles. Roles can be applied to groups to quickly add and remove permissions en-masse to users
-  Policies can be attached directly to users, theses are called "inline policies"
-  Roles can have many policies attached
-  Various AWS resources can have roles directly attached to them

## Types of policies
Managed Policies: most common permission we can neeed (orange box)
Custom Manager Policies
Inline Policies

## Policies
- Version: policy language version, **2012-10-17** is the latest version
- Statement
	- SId (optional): just a label
	- Effect: Allow or Deny
	- Action: list of actions
	- Principal: account, user, role or federated user
	- Resource
	- Codition

## Password policy
Set minimum requirement, rotate passwords so users have to udpate their passwords after X days

## Programmatic access keys
Interact with AWS using the CLI, SDK or REST API
Maximum is 2 keys per users, if we want more we have to remove old ones. 

## MFA
Admin can't require users to setup MFA but they can allow access to some resources only to users who have MFA.


