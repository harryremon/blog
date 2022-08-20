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

1. [Create an Azure Key Vault](#create-an-azure-key-vault)
2. [Generate a Certificate in the Azure Key Vault](#generate-a-certificate-in-the-azure-key-vault)
3. [Create a client Application in the Azure Active Directory](#create-a-client-application-in-the-azure-active-directory)
4. [Create an application user in the environment](#create-an-application-user-in-the-environment)
5. [Import the certificate to the local machine](#import-the-certificate-to-the-local-machine)
6. [Connect to the environment](#connect-to-the-environment)

### Create an Azure Key Vault

Login to your [Azure Portal](https://portal.azure.com) and create a new azure key vault resource:

![Create New Azure Key Vault](/assets/create-azure-keyvault.PNG)

Choose your resource group or create a new one, choose a name for the key vault and a region to deploy it to.

Now, Click on Review + create. Make sure that everything is in order, then go ahead and create the Key Vault resource.

### Generate a Certificate in the Azure Key Vault

Now, open the azure key vault resource and go to "**Certificates**", and click on "**Generate/Import**"

![Create a Certificate](/assets/create-certificate.PNG)

Choose a name for your certificate, and enter this as the subject:
```json
CN=dynamics.com
```
Now create your certificate and enable it.

Open your certificate and download it in both CER and PFX/PEM format, we will need those later on.

Next, We'll need to create an application under your Azure Active Directory tenant to act as the client app we will use to access the environment.

### Create a client Application in the Azure Active Directory

Go to your Azure Active Directory tenant in azure portal, and go to "**App registrations**".

Create a new registration and name it whatever you want.

Now open the app you just created, and go to "**Certificates & secrets**"

Click on "**Upload certification**" and choose the **.cer** file you downloaded in the previous step.

![Certificate](/assets/certificate.PNG)

Now copy the certificate Thumbprint as we will need to use it later in the connection string.

### Create an application user in the environment

Go to your dataverse or CRM Online environment, and go to Settings > Security > Users (You might need to open "Advanced Settings")

Change the user view to "Application Users" and create a new user, make sure that the form you're using is for "**Application User**", it should look like this

![New Application User](/assets/newuser.PNG)

Fill in the Application ID of the app you registered in the Azure Active Directory before and Save.

**At this point you should be all set to connect to the environment using the certificate.**

### Import the certificate to the local machine

Now, in your local machine (or the one you'll be using to connect to the environment), open the PFX/PEM certificate file to import it. skip the page where the wizard asks for a password for the file.

### Connect to the environment

From now on, whenever you need to connect to the environment, either using a tool or from within your code, you can use the following connection string to connect:

```xml
AuthType=Certificate;
url=https://contosotest.crm.dynamics.com;
thumbprint={CertThumbPrintId};
ClientId={AppId};"
```

Just insert the certificate thumbprint and your application client ID you used before and you should be good to go.
