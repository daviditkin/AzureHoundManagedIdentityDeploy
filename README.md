# AzureHoundDeploy

## Overview

AzureHound now supports Managed Identity authentication.  This allows AzureHound to be run in an Azure Container Instance.  The Container Instance must be associated with a Managed Identity that has the required RBAC Roles and Graph Permissions that AzureHound requires.

This repository containes the Azure Resource Manager (ARM) Template along with supporting scripts that
allow a user to conveniently deploy and configure AzureHound to an Azure Container Instance.  Specifically this ARM template provides the following functionallity.

1) Deploy an AzureHound Instance that runs in an Azure Container Instance
2) Creates or uses an existing Container Instance
3) Creates or uses an existing Managed Identity for the Container that provides Azure permissions
4) Provides a wizard that configures the AzureHound Instance

## Process


## Prerequisites

In order for this ARM Template to create the container's User Managed Identity, the ARM Template requires an existing User Managed Identity with the following permissions:

   - Application.ReadWrite.All
   - Managed Identity Contributor
   - User Access Administrator

This repository contains a `create-deployment-umi-with-perms.ps1` script that can be used to create deployment's user managed identity.  Alternatively you can create the User Managed Identity in the Azure portal.

## AzureHound Required Permissions

AzureHound requires the following Azure permissions

 - Directory.Read.All
 - Reader 
 
The ARM Template will create the Container Instance along with a User Managed Identity that provides this permissions to the AzureHound Instance.

## Supporting Scripts

The ARM Template is designed to create the Container Instance along with a User Managed Identity that will provide AzureHound with all the permissions it needs to run.  However, it

- Create/Fix Managed Identity For The ARM Template
   `create-deployment-umi-with-perms.ps1`
- Create/Fix Managed Identity For the Container
   `create-container-umi.ps1`
- Full end to end script.
   `single-script-full-deployment.ps1`


## Notes About Approach
ManagedIdentities can be assigned permissions just like App Registration (Enterprise Applications), however you are assigning the permissions to 
the managed identity's application object id.  After creation of a Managed Identity it takes some amount of time before the application id is associated with the managed identity.  Therefore we add retry logic.

## Permissions DeploymentScript requires
The `managed-identity-permissions.sh` script will require 
the following permissions to be assigned to a managed identity.  

