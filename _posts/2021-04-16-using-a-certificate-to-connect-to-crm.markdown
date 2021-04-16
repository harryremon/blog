---
layout: post
title:  "Using a Certificate to Connect to Dataverse"
date:   2021-04-16 20:00:00 +0200
categories: crm dynamics update
---

Recently, we created a dataverse environment to use in development, and we needed to connect to it using a connection string in multiple tools (like XRMToolbox).

The credentials for the environment were the same as our work account credentials, so I didn't feel comfortable with including them in the connection string which could sometimes be shared with other developers.

The solution ? Create a certificate and access the environment as an application user.

Here's what we need to do in details:

1. Create an Azure Key Vault
2. Generate a Certificate in the Azure Key Vault
3. Create an application user in the environment
4. Import the certificate to the local machine
5. Connect to the environment

### Create an Azure Key Vault

Login to your [Azure Portal](https://portal.azure.com) and create a new azure key vault resource:

![Create New Azure Key Vault](/blog/assets/create-azure-keyvault.PNG)

