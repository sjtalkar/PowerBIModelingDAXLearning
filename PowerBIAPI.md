# Using Power BI REST API to get information about the environment

> - [REGISTER AN AZURE APP TP USE WITH POWER BI](https://carldesouza.com/how-to-register-an-azure-app-to-use-with-power-bi/)
> - [HOW TO CALL THE POWER BI REST API FROM POSTMAN](https://carldesouza.com/how-to-call-the-power-bi-rest-api-from-postman/)
> - [Configuring Postman for Use with the Power BI REST API](https://dataveld.com/2020/05/09/using-postman-with-the-power-bi-rest-api/)


1. The Power BI API contains many useful features if you’re looking to interact with Power BI at the API level. In order to use it, we need to register an Azure App first.
2. Registering an app article has all the details about creating the app. Name of URL https://powerbiapp.cm.com


Four variables are used in the request body to obtain the access token from https://login.microsoftonline.com/common/oauth2/token. These are local session variables only (specify Current Value but do not add sensitive data to Initial Value).

1) username – Power BI organizational user
2) password – Power BI organizational password
3) client_id – ID from your Azure Active Directory application
4) client_secret – Secret from your Azure Active Directory application

The fifth variable, temp_access_token, is not specified manually like the other four. Instead, you will use code in the Postman Tests tab to write the access token to that variable. Tests is an area to place JavaScript code that runs after the request occurs.
