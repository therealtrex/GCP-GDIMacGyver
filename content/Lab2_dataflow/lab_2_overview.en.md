# Ingesting GCP cloud logging data into Splunk using the Pub/Sub to Splunk Dataflow Template 
Welcome to Lab 2. This lab will step you through all required steps to configure the Pub/Sub to Splunk Dataflow template.

This lab will go through the following: 
- Create a Splunk HEC token endpoint for ingestion
- Creating a Cloud Storage bucket for our dataflow temporary files
- Expand permissions for our GCP service account used for Dataflow
- Deploy a Pub/Sub topic and subscription for deadletter queue to handle failed dataflow events
- Deploy the Splunk dataflow template
- Validate the data is flowing into Splunk sucessfully. 

## Architecture Diagram
Below is the architecture pattern used for this lab:

![image_tag](/static/Lab2_dataflow/lab2_architecture.png) 


>[!NOTE]
>Usually this lab would have a pre-requisite of the <a>[GCP add-on](https://splunkbase.splunk.com/app/3088)</a> installed on your Splunk installation. To make things simpler we have already done this. 


### Click <a>[Next](/content/Lab2_dataflow/lab_2_dataflow.en.md)</a> to continue or click <a>[Back](/content/README.md) to go back to the beginning</a>