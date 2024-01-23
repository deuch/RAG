# RAG Automation

This repo will help you to deploy a RAG solution based on the chat on your data accelerator that you can find [here](https://github.com/Azure-Samples/chat-with-your-data-solution-accelerator/tree/main)

## Architecture

The architecture is based on the one deployed for AML.  
A central hub VNETT (with Firewall) and all the applications use their own VNET which is peered to the hub.  
NSG and UDR are applied on all subnet and filtering is done by the Firewall.  

The new subnets must be added to the IP Firewall Group to enable filtering.

## Pre-requisites

1. You need a ACR **in the hub resource group**, with premium SKU.
2. Create a private endpoint in the *pe* subnet
3. Disable public access
3. Enable admin login
4. Keep the IPs of the 2 private endpoint and the associated FQDN (for eg *myacr*.azurecr.io and *myacr.region*.data.azurecr.io)
5. You need to push the images in the new registry with those names (it can be modified in the deployment parameters later if needed):
  - rag-webapp:1.0
  - rag-adminwebapp:1.0
  - rag-backend:1.0
6. You need to create a scope map and a associated token to be able to push those images

## Deployment parameters

| Parameter | Value | Note |
| --- | --- | ------------- |
|ResourcePrefix||Provide a 2-13 character prefix for all resources|
|addressPrefixes||Address prefix of the virtual network| 
|hubVNetName||The name of the Hub VNET|
|hubVNetResourceGroupName||Resource Group of the Hub VNET|
|firewallIPGroupName||Private IP of the Azure Firewall|
|privateAZFWIP||The name of the semantic search configuration to use if using semantic search.|
|ACRName||Name of the Azure Container Registry|
|ACRPrivateIP||Private IP of the Azure Container Registry|
|ACRDataPrivateIP||Private IP of the Azure Container Registry Data Endpoint|
|ACRUserName||User name (scope map) for Azure Container Registry|
|ACRPassword||Password (token) of the Azure Container Registry|
|WebsiteName||Name of Web App|
|ACRWebAppImageName|rag-webapp:1.0|Name/tag of Container Image for Web App| 
|ACRAdminWebAppImageName|rag-adminwebapp:1.0|Name/tag of Container Image for Admin Web App|
|ACRBackendImageName|rag-backend:1.0|Name/tag of Container Image for Admin Web App|
|AzureOpenAIResource||Name of Azure OpenAI Resource|
|AOAIResourceGroupName||ResourceGroup Name of Azure OpenAI Resource|
|AzureOpenAIKey||Azure OpenAI Key|
|AzureOpenAIModel||Azure OpenAI Model Deployment Name|
|AzureOpenAIModelName|gpt-35-turbo|Azure OpenAI Model Name|
|AzureOpenAIEmbeddingModel||Azure OpenAI Embedding Model Deployment Name|

### POST Manual actions