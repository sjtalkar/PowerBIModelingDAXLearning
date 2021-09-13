[Why do we need Power BI gateway](https://www.youtube.com/watch?v=i1W5kISMF50)
[Documentation](https://docs.microsoft.com/en-us/power-bi/connect-data/service-gateway-onprem-indepth)

> ** Bridge between ON Premise data source 
> > Power BI service ** ** THINK RERESH**
> For the gateway you do not need a paid licence. You can create a personal gateway even with free licence.
> You know you need a gateway when you cannot refresh the dataset in the service and when you cannot schedule a refresh in the service.


**On-premises** data gateway: Allows multiple users to connect to multiple on-premises data sources. With a single gateway installation, you can use an on-premises data gateway with all supported services. This gateway is well suited to complex scenarios where multiple people access multiple data sources.

In addition, there's a virtual network (VNet) data gateway that lets multiple users connect to multiple data sources that are secured by virtual networks. No installation is required because it's a Microsoft managed service. This gateway is well suited to complex scenarios in which multiple people access multiple data sources. Virtual network data gateways are discussed in depth in What is a virtual network (VNet) data gateway.



> ![Setup](https://github.com/sjtalkar/PowerBIModelingDAXLearning/blob/main/GatewayandDataSources.JPG)

[Download the Power BI Data Gateway](https://powerbi.microsoft.com/en-us/gateway/)
For Gateway evaluation choose personal mode
- For Gateway in an enterprise Choose standard model 
    - Typically set up by IT (but can be set up by business user) But advantage is can be used by multiple connections.
    

When is it feasuble to avoid using a Gateway
- When you do not have on-premise, that is local sources
- When you have Cloud services (such as Sharepoint files)
- When you do not need to refersh in the sevice! ( refresh manually and publish to the cloud)

[INSTALLATION](https://docs.microsoft.com/en-us/data-integration/gateway/service-gateway-install)
