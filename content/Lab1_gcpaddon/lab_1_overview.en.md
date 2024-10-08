# Lab 1: Ingesting GCP cloud logging data into Splunk using the Splunk Add-on for GCP (PULL) method. 
Welcome to Lab 1. This lab will step you through all required steps to GCP cloud data using the Splunk Add-on for Google Cloud PULL method.

This lab will go through the following: 
- Creating a Pub/Sub topic Log Sink to use for sending log data to a Pub/Sub subscription
- Create a Google Cloud Service Account to use with the Splunk Add-on for Google Cloud Platform 
- Configure the service account credentials into the Splunk Google Cloud Add-on
- Create a three modular inputs on the Google Cloud Add-on
- Lastly we will briefly explore the data to make sure it is ingesting ok. 

## Architecture Diagram
Below is the architecture pattern used for this lab:

![image_tag](/static/Lab1_gcpaddon/lab1_architecture.png) 


>[!NOTE]
>Usually this lab would have a pre-requisite of the <a>[GCP add-on](https://splunkbase.splunk.com/app/3088)</a> installed on your Splunk intallation. <b>To make things simpler we have already done this</b>. 


### Click <a>[Next](/content/Lab1_gcpaddon/lab_1_gcp_ta.en.md)</a> to continue or click <a>[Back](/README.md) to go back to the beginning</a>