# Lab 1: Ingesting GCP cloud logging data into Splunk using the Splunk Add-on for GCP (PULL) method. 
Welcome to Lab 1. This lab will step you through all required steps to ingest GCP cloud data using the Splunk Add-on for Google Cloud PULL method.

This lab will go through the following: 
- Creating a Pub/Sub topic and Subscription for our logs
- Create a Cloud Logging log sink to log data to our Pub/Sub topic
- Create a Google Cloud Service Account to use with the Splunk Add-on for Google Cloud Platform 
- Configure the service account credentials into the Splunk Google Cloud Add-on
- Create three modular inputs on the Google Cloud Add-on for Pub/Sub Logs, Cloud Monitoring CPU metrics and Compute Engine Metadata
- Lastly we will briefly explore the data to make sure it is ingesting ok. 

## Architecture Diagram
Below is the architecture pattern used for this lab:

![image_tag](/static/Lab1_gcpaddon/lab1_architecture.png) 

### Click <a>[Next](/content/Lab1_gcpaddon/lab_1_gcp_ta.en.md)</a> to continue or click <a>[Back](/README.md) to go back to the beginning</a>