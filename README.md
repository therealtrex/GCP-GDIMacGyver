# Welcome to a GCP / Splunk GDI MacGyver Workshop
#### a T-REX production

<p align="center">
<img width="400" src="/static/gdimacgyverlogo.png">
</p>

## Overview 

Welcome to the Splunk and GCP GDI MacGyver workshop. This workshop forms part of the GCP Workshop platform found <a>[HERE](https://explore.qwiklabs.com/classrooms/15807/labs/93937) </a>

In this workshop, we will study the two main different solutions to get GCP logging data into Splunk, these are:
1) Configure Google Cloud Pub/Sub and log sink to allow Splunk subscribe to the logs <b>(PULL method)</b>
2) Configure a Cloud Pub/Sub, Dataflow export of logs to Splunk HEC endpoint <b>(PUSH method)</b>
3) Cofigure exactly the same lab as <b>Lab 1<b> but this time do it via the Google Cloud CLI
4) Configure exactly the same lab as <b>Lab 2</b> but thie time do it via the Google Cloud CLI

Finally in <b>lab 5</b> you will wrap up by exploring some of your ingested data in Splunk using SPL and saving that data into a custom dashboard!

>[!NOTE]
> -This workshop is expected to take approximately 1-2 hours to complete.<br>
> -This workshop is created for target audiences like Cloud Architects, Security Analysts and Splunk Administrators.<br>
> -The participants are expected to have a general experience with GCP products and services, along with basic knowledge Splunk, Splunk Apps and running Splunk SPL Queries.<br>
> -This workshop is intended to be run as part of a joint Google and Splunk event.<br>
> -To make things easier we have already configured the Google Cloud add-on on your Splunk service including an index called `gcp-data` for you to use. Please use these are part of the labs. 

>[!IMPORTANT]
>Please make sure you sign out or use an incognito tab when running the labs as you do not want to accidentally use your work/personal Google account and end up with a bill or get in trouble!
>TRUST ME, it has happened to people!!

Once you have completed this workshop you will end up deploying the following archiecture pattern:

![gcp_gdi_architecture](/static/gcp_gdi_workshop_architecture.png)

## Time to get to get started!
Below are the links to each of the labs

<a>[LAB1 - Configure Splunk GCP Add-on to PULL logs to Splunk](/content/Lab1_gcpaddon/lab_1_overview.en.md) </a>

<a>[LAB2 - Configure Google Dataflow to PUH logs to Splunk ](/content/Lab2_dataflow/lab_2_overview.en.md) </a>

<a>[LAB5 - TBD ](/content/Lab5_data_exploration/exploring_data_1.md) </a>