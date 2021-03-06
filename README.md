# Cloud Service Management and Operation for Hybrid application

## Architecture Overview
This project provides is a reference implementation for managing a BlueCompute Application that is hybrid in nature.
Cloud based applications need to be available all the time. Proper processes need to be put in place to assure availability and performance. This includes Incident and Problem management to respond to outages, but also Release Management to assure a seamless deployment and release of new versions.

  The Logical Architecture for overall Cloud Service Management and Operations is shown in the picture below.
   ![CSMO Architecture](static/imgs/Cloud_Service_Management_Incident_Mgmt_Overview-v2.png?raw=true)  

For more details on this reference architecture visit the [Architecture Center for Service Management](https://developer.ibm.com/architecture/serviceManagement)

## Incident Management Architecture Overview

Incident management and its operations are key to cloud service management. Incident management is optimized to restore the normal service operations as quickly as, thus ensuring the best possible levels of service quality and availability are maintained. Following figure provides deep dive into Incident Management 
![CSMO Incident Management Architecture](static/imgs/Cloud_Service_Management_Incident_Management-03.png?raw=true)  

For more details on this reference architecture visit the [Architecture Center for Incident Management](https://developer.ibm.com/architecture/gallery/incidentManagement)

### Reference Tools Mapping 
For this project we utilized a reference set of tools to showcase end-to-end incident management.

**Monitoring** - uses [NewRelic](https://newrelic.com/) for resource monitoring and IBM Bluemix Application Management (BAM) for synthetic monitoring of the hybrid application and components.

**Event Correlation** - use the [IBM Netcool Operations Insights](http://www-03.ibm.com/software/products/en/netcool-operations-insight) to fulfill the event management and correlation activities.

**Notification** - uses [IBM Alert Notification](http://www-03.ibm.com/software/products/en/ibm-alert-notification)

**Dashboard** - uses open source [Grafana](http://grafana.org/)

### Understanding System Context Flows for the Tools in CSMO Toolchain Connecting BlueCompute Application


#### System Context Flow for New Relic

<upcoming pic>

The above figure shows the deep dive of NewRelic Ressource Monitoring and its various components and various integrated tools for incident management and their interactions. 

1.	NewRelic offers the instrumenation of Node.js application, nginx webserver/loadbalancer, java microservices and mysql database and allows monitoring of key performance indicators for those ressources. It detects also Bluemix services used by various Bluemix applications. Both Public and SoftLayer instances are monitored. In Bluemix we will find native clound foundry applications as well as docker containers. 

2.	The data will be transferred to NewRelic management system and accessible with the UI and API calls.

3.	If thresholds are exceeded based on defined alert policy settings one or more alerting channels can be used.

4.	These channels actually forward the incident to external event correlation and/or a Dashboard solutions. In this scenario we are using NOI for event correlation and NewRelic forwards its events to an Omnibus message bus probes via a WebHook channel. 


#### System Context Flow for BAM

<upcoming pic>

The above figure shows the deep dive of IBM Bluemix Application Monitoring (BAM) and its various components and various integrated tools for incident management and their interactions. BAM monitors the status Bluemix application in two ways. 

1.	BAM will check the base application URL and also run sample rest API calls against the URL. 

2.	Secondly we can record synthetic web transactions scripts with the Selennium IDE browser plugin, upload them into BAM and playback those scripts from three different data centers. 

3.	Both checks will provide status information of the application components and sampled transaction use cases. 

4.	Based on threshold settings violations in terms of availability or performance can be reported to the IBM Alert Notification System Bluemix Service.

5.	ANS then will also forward these incidents to an external event correlation system via email. In this scenario we are using NOI as event correlation system.


#### System Context Flow for IBM Netcool Operations Insights (NOI)

<upcoming pic>

The above figure shows the deep dive of NOI and its various components and various integrated tools for incident management and their interactions. One of the key takeaways from the diagram is that the solution supports a heterogeneous mixture of products and solutions, each feeding or being fed by the central NOI solution.

The following flow describes the setup and operations of this solution in an overall cloud service management space:

1.	BlueCompute application components & infrastructure are monitored by 3rd party solution NewRelic  for resource monitoring and IBM BAM for synthetic monitoring (via ANS).

2.	The probes normalize the events into a common format and send them to the central Omnibus. The monitoring events sent to NOI via these probes are then correlated, de-duplicated, analyzed & enriched. Further action may be automated or performed by an first responder/incident Owner/runbook automated service. 

3.	Impact has two roles. It extracts events from the BlueMix infrastructure and forwards events on to the collaboration and notification solutions. The analytics component of NOI enriches the events and finds correlations between events, limiting the number of alarms forwarded and making sure that important issues are prioritized. The dashboards are used both to display events and manually forward them if an operator decides to do so. Impact also enrich technical events with organizational and environmental context like deployment location, service relationship or affected client. Here a configuration data source can be leveraged.

4.	The correlated events are forwarded to collaboration and notification tools. Action may be performed to solve the issues detected. NOI supports a variety of such solutions and in this document we will detail integration with Slack for collaboration and Pager Duty for notification and escalation. NOI can publish events to generic targets (i.e. a single Slack channel which is used for all alerts) or specific ones (i.e. NOI will be automated to create a dedicated Slack channel for a single generated event ). 

5. Runbooks connected to NOI are automated to update the event status based on the resolution of the issue. The status updates can also be manually handled within NOI. It also has capability to have bidirectional communication with notification tool so that event status update can take place in either tool. This updated status is then propagate


#### System Context Flow for Grafana

<upcoming pic>

The above figure shows the deep dive of Grafana and its various components and various integrated tools for incident management and their interactions. 

1.	A Perl Runtime collects on a regular scheduled basis data from various data sources which provide monitoring and status information for the BlueCompute application. In this scenario this includes the ressource monitoring data from NewRelic  via a Rest API, the Bluemix Cloud Foundry information for applications and container via the CF API and the synthetic monitoring results from BAM via a Rest API

2.	Perl runtime also accesses a configuration data source on mysql to read and enrich the monitoring data with environment context data like location and service-relationship.

3.	The perl runtime mashes up all relevant data and writes the consolidated data into the InfluxDB.

4.	Grafana accesses the data via its defined data sources and displays the InfluxDB data inside the configured dashboard pages.

5.	Grafana integrates also the NOI data in two different ways. The Dashboard event viewer widget is embedded on dashboard pages via links.


### How to Use the toolchain
 
The following walkthrough guides you through how to use the toolchain for end-to-end monitoring of the hybrid application. You will learn how to implement basic incident management capabilities and how to build a more advanced, robust incident management solution.

###Step 1: Installation prerequisites

When deployed using an instant runtime, the solution for incident management requires the following items:

    IBM Bluemix account
    A hybrid application - [BlueCompute application set up instruction](https://github.com/ibm-cloud-architecture/refarch-cloudnative)
    
###Step 2: Monitoring

The cloud native Cloud Service Management and Operations [incident management walkthrough](https://developer.ibm.com/architecture/gallery/incidentManagement/walkthrough/Introduction) is provided with the tools in the toolchain.

This section only focusses on updates needed to instrument or use a hybrid application.  

####Step 2a: How to Use New Relic for BlueCompute


####Step 2b: How to Use for BAM for BlueCompute




