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
- When using ADF Azure Data Factory Integration Runtime.
    Which is similar to a data gateway
    The runtime is an application installed on the VM or machine running SQL Server.
    If the client has an on-prem database and you want to use it as an ADF data source, they must install the runtime on a machine in their on-prem environment.

[INSTALLATION](https://docs.microsoft.com/en-us/data-integration/gateway/service-gateway-install)
[Video on Enterprise setup](https://www.youtube.com/watch?v=fejSQmshwrE&t=2s)

> Where should the gateway be installed
 - A machine that is always on and is connected to a wired netweork (not WIFI - preferably)
 - Recommended that the machine have a Solid State Drive (data is often cached on the hard drive before sending it out on the Gateway
 - Locate the Gateway close to the Source (perhaps not on the same machine as the DB server)
 - Start viewing for installation from 6:30 (https://www.youtube.com/watch?v=fejSQmshwrE&t=2s)
- After installation 
    - Email Address to use with this gateway Sign in using the same account you use to login to PowerBI.com (the service) 
    - This will become "an" admin for the gateway
    - Other Admins can be added at a later time
- Set up the gateway name - set up a convention 
- **Recovery key** -- STORE THIS CAREFULLY

> Evaluating the usage of the gateway in the service
 - Go to the dataset and check out the Settings and the Gateway connections
 - OR in the service, in the top right Settings Gear, select Manage Gateways
 - By seelcting a Gateway Connections in Settings there will be provided a dropdown from which you can select your datasources
