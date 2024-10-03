# Lab 2: Ingesting GCP cloud logging data into Splunk using the Pub/Sub to Splunk Dataflow Templte 
Welcome to Lab 2. This lab will step you through all required to configure the Pub/Sub to Splunk Dataflow template.

This lab will go through the following: 
- Create a Splunk HEC endpoint for ingestion
- Creating a Cloud Storage bucket 
- Expand permissions for GCP service account 
- Deploy a Pub/Sub topic for dealletter queue
- Deploy dataflow template
- Validate the data is flowing into Splunk sucessfully. 

## Architecture Diagram
Below is the architecture pattern used for this lab:

![image_tag](/static/Lab1_gcpaddon/lab1_architecture.png) 


>[!NOTE]
>Usually this lab would have a pre-requisite of the <a>[GCP add-on](https://splunkbase.splunk.com/app/3088)</a> installed on your Splunk intallation. To make things simpler we have already done this. 


### Click <a>[Next](/content/Lab2_dataflow/lab_2_dataflow.en.md)</a> to continue or click <a>[Back](/content/README.md) to go back to the beginning</a>