# mule4-skeleton-sys-template
This is a Mule 4.2 example of an API using API Management that can be used as a starting point for building system APIs in Mule 4.2.


## Uses

The project contains examples using:

* The JSON Logger connector,
* The reusable common-error-handler plugin to emit standard structure for Science 37
* Maven deployment using the mule-maven-plugin descriptor, Munit configuration
* Using the Maven filter "feature" to add Maven properties to code in files stored in the resources-filtered directory...specifically, reading the attributes from maven and log4j2 configurations with the project name.
* Example Munit test cases for flows

##Purpose

This project is an example that can be deployed and will run when properly configured. However, the main usage for this is a starting point for building new APIs.

Developers of an API can copy this project to get all the "boilerplate" elements for an API project. Then begin adding and modifying these elements to create the new API.

##Configuration Properties
Properties that are environment specific can be stored in src/main/resources/${env}-config.yaml file.

Properties that do not change for any of the deployment environments can be stored in the src/main/resources-filtered/common.yaml file. This file will be filtered by Maven which means properties can take on values from the Maven build process. Example of what is the common.property file is:

* api.semantic.version is the API syntax version, used to form the API's base uri.
* JSON Logger configuration related properties


##Runtime properties

There are many properties associated with the API as part of its Runtime configuration. These are set in deployment properties for CloudHub, the wrapper.conf file for standalone Mule Runtimes and in the Studio Run configuration arguments.

An example of the runtime properties needed for Studio are in the file "example-studio-run-arguments.txt".

_Note: Maven command arguments -Dmule.env=local needs to specified in Anypoint Studio to run the project_

The pom.xml file contains the Runtime properties for CloudHub deployment and if we need to update the runtime properties for application we need add those properties in cloudhub deployment properties

For example in the pom.xml you need add your application properties under property name and element name should match the property name and value will be passed at runtime
```
    <cloudHubDeployment>
						<uri>https://anypoint.mulesoft.com</uri>
						<muleVersion>${app.runtime}</muleVersion>
						<username>${u}</username>
						<password>${p}</password>
						<applicationName>${cloudhub.name}</applicationName>
						<businessGroup>${deploy.businessGroup}</businessGroup>
						<environment>${deploy.environment}</environment>
						<workers>${deploy.workers}</workers>
						<workerType>${deploy.workerType}</workerType>
						<deploymentTimeout>1000000</deploymentTimeout>
						<region>us-west-2</region>
						<properties>
							<mule.env>${mule.env}</mule.env>
							<anypoint.platform.client_id>${api.anypoint.platform.client_id}</anypoint.platform.client_id>
							<anypoint.platform.client_secret>${api.anypoint.platform.client_secret}</anypoint.platform.client_secret>

							<!-- application Runtime Properties override using mvn deploy command
							<api.id>${api.id}</api.id>
							<my.client_id>${my.client_id}</my.client_id>
							<my.client_id>${my.client_secret}</my.client_id>
							-->
							<!--  Log4J properties
							<splunk.host>${splunk.host}</splunk.host>
							<splunk.port>${splunl.port}</splunk.port>
							<splunk.host>${fluentd.port}</splunk.host>
							<splunk.port>${fluentd.host}</splunk.port>
							 -->
						</properties>
					</cloudHubDeployment>
```
For Deployment using anypoint-cli command the propery can be overwritten using -D<<propertyname>>=<<propertyValue>>

In cloudhub inorder to secure and hide the properties update the mule-artifact.json with list of properties to you want to secure for example: "secureProperties": ["property1","property2"]


##Maven Settings

The Mule deployment assumes that certain deployment properties will come from profiles specified when the mvn command is executed. An example-settings.xml is provided as a reference for creating your own settings.xml file. The settings.xml file is a standard part of Maven and is described in its online documentation. Once the settings.xml file has been created, replace a ${u} and a ${p} shell environment variables containing your Anypoint user name and password.
### For Federated access (Anypoint platform with SSO login)
[Federated Access](https://docs.mulesoft.com/exchange/to-publish-assets-maven#publish-federated-assets)
- Note: In example-settings.xml you need to replace the muleNexusUser and muleNexusPass with Amgen credentials for mule repository -

Now you should be able to run the project in the studio

## Deploy the code to cloudhub environment

Then use this maven command to deploy the API project:

```
mvn clean install deploy -Denv=xxx -DmuleDeploy -Dmule.env=xxx -Du=xxx -Dp=xxx

Replacing the xxx's with the appropriate values.

For Example to deploy in CI using maven

mvn clean install deploy -DmuleDeploy -Dmule.env=LOCAL -Du=test_user -Dp=password
```


## publish the template to exchange

```
Make sure pom.xml version should match with pom.xml.template-publish
mvn clean deploy  -f pom.xml.template-publish -Dmule.env=LOCAL

You can also publish through Anypoint studio
Right Click on the mule project (e.g., skeleton-sys-template) and clik Anypoint Studio->Publish to Exchange
```
