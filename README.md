# RAG Automation

This repo will help you to deploy a RAG solution based on the chat on your data accelerator that you can find [here](https://github.com/Azure-Samples/chat-with-your-data-solution-accelerator/tree/main)

## Architecture

The architecture is based on the one deployed for AML.  
A central hub VNET (with Firewall) and all the applications use their own VNET (as a spoke) which is peered to the hub.  
NSG and UDR are applied on all subnet and filtering is done by the Firewall.  

The new subnets must be added to the IP Firewall Group to enable filtering.

[![Deploy To Azure](https://raw.githubusercontent.com/deuch/RAG/master/images/deploytoazure.svg?sanitize=true)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2Fdeuch%2FRAG%2Fmain%2Fdeployment.json)


## Pre-requisites

1. You need a ACR **in the hub resource group**, with premium SKU.
2. Create a private endpoint in the *pe* subnet
3. Disable public access
3. Enable admin login
4. Keep the IPs of the 2 private endpoint and the associated FQDN (for eg *myacr*.azurecr.io and *myacr.region*.data.azurecr.io)
5. You need to push the images in the new registry with those names (it can be modified in the deployment parameters later if needed):
    - rag-webapp:2.1
    - rag-adminwebapp:2.1
    - rag-backend:2.1 
6. You need to create a scope map and a associated token to be able to pull those images

## Deployment parameters

| Parameter | Value | Note |
| --- | --- | ------------- |
|ResourcePrefix||Provide a 2-13 character prefix for all resources|
|addressPrefixes||Address prefix of the virtual network| 
|hubVNetName||The name of the Hub VNET|
|hubVNetResourceGroupName||Resource Group of the Hub VNET|
|firewallIPGroupName||Name of the IP Group needed for the Azure Firewall|
|privateAZFWIP||Private IP of the Azure Firewall|
|ACRName||Name of the Azure Container Registry|
|ACRPrivateIP||Private IP of the Azure Container Registry|
|ACRDataPrivateIP||Private IP of the Azure Container Registry Data Endpoint|
|ACRUserName||User name (Token name) for Azure Container Registry|
|ACRPassword||Password (token) of the Azure Container Registry|
|WebsiteName||Name of Web App|
|ACRWebAppImageName|rag-webapp:2.1|Name/tag of Container Image for Web App| 
|ACRAdminWebAppImageName|rag-adminwebapp:2.1|Name/tag of Container Image for Admin Web App|
|ACRBackendImageName|rag-backend:2.1|Name/tag of Container Image for Admin Web App|
|AzureOpenAIResource||Name of Azure OpenAI Resource|
|AOAIResourceGroupName||ResourceGroup Name of Azure OpenAI Resource|
|AzureOpenAIKey||Azure OpenAI Key|
|AzureOpenAIModel|gpt-35-turbo|Azure OpenAI Model Deployment Name|
|AzureOpenAIModelName|gpt-35-turbo|Azure OpenAI Model Name|
|AzureOpenAIEmbeddingModel|text-embedding-ada-002|Azure OpenAI Embedding Model Deployment Name|

### POST Manual actions

1. You need to approve the Shared Private Access Link on the storage account for AI Search service
2. You need to add the **Storage Blob Data Reader** Role to the System Managed Identity of your Document Intelligence service on the Storage Account
3. Please add an Identity Provider for the 2 webapps (webapp and adminwebapp)
