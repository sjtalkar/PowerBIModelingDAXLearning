[Why do we need Power BI gateway](https://www.youtube.com/watch?v=i1W5kISMF50)

> ** Bridge between ON Premise data source > Power BI service ** ** THINK RERESH**
> For the gateway you do not need a paid licence. You can create a personal gateway even with free licence.
> You know you need a gateway when you cannot refresh the dataset in the service and when you cannot schedule a refresh in the service.
> ![Setup](https://github.com/sjtalkar/PowerBIModelingDAXLearning/blob/main/GatewayandDataSources.JPG)

[Download the Power BI Data Gateway](https://powerbi.microsoft.com/en-us/gateway/)
For Gateway evaluation choose personal mode
- For Gateway in an enterprise Choose standard model 
    - Typically set up by IT (but can be set up by business user) But advantage is can be used by multiple connections.
    

When is it feasuble to avoid using a Gateway
- When you do not have on-premise, that is local sources
- When you have Cloud services (such as Sharepoint files)
- When you do not need to refersh in the sevice! ( refresh manually and publish to the cloud)


