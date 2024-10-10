# Ingesting GCP cloud logging data into Splunk using the Splunk Add-on for GCP (PULL) method. 
Welcome to Lab 1. Let's get into it! 

>[!NOTE]
>Usually this lab would have a two pre-requisites of the <a>[GCP add-on](https://splunkbase.splunk.com/app/3088)</a> installed on your Splunk intallation and a Splunk index to store all the data. To make things simpler we have already done this.
>Throughout this lab whenever you are prompted to select a Splunk index please use <b>gcp_data</b> 

### Create a Pub/Sub Topic
- From GCP console, select the top left `handburger` (three bars) and select `VIEW ALL PRODUCTS`

![image_tag](/static/Lab1_gcpaddon/image20.png)

- Select `Analytics` on the left hand menu then select `Pub/Sub`
- From the Pub/Sub screen select `+ CREATE TOPIC`
- Name your topic `splunk_ta_topic`
- Click `Create` leaving all other defaults

![image_tag](/static/Lab1_gcpaddon/image30.png)

>[!NOTE]
>This will take around 15 seconds to create.

>[!IMPORTANT]
> The Google UI (different to the CLI) will automatically create a subscription for you whenever you createa a topic. This will be the same name as the topic with `_sub` appended to it.
> For this lab our a subscription of splunk_ta_topic_sub will automatically be created. You can view this in the Google Console under Pub/Sub subscriptions.

### Create a Log Sink
- From GCP console, select the top left `handburger` (three bars) and select `VIEW ALL PRODUCTS`
- Select the `Observability` Category followed by `Logging`
- In the Logging section from the left hand menu (may need to hover mouse on left hand side) select `Log Router`

![image_tag](/static/Lab1_gcpaddon/image24.png)

- From the Log Router page select `Create sink`
- Name your sink `splunk_ta_sink`
- Add optional description. 
- Click `next` when ready

![image_tag](/static/Lab1_gcpaddon/image31.png)

- From the Sink destination section, enter in `Splunk` as the sink service
- Select the `Pub/Sub topic we created earlier` as the topic. 
- Click `Next` when ready

![image_tag](/static/Lab1_gcpaddon/image32.png)

- Leave defaults (empty) for logs to include in sink and click `Next`
- Leave defaluts (empty) for logs to filter out of sink and click `Next` 
- Click `Create sink` when ready. 


>[!NOTE]
> This will sent all logs to this sink. This may not be best practice for your environment but for this lab will be fine. 

### Create Service Account for GCP add-on
- From GCP console, select the top left `handburger` (three bars) and select `IAM & Admin` followed by `Service Accounts`
- Click `+ CREATE SERVICE ACCOUNT`
- Enter `splunk-ta` as the service account name
- Enter in optional description
- Click `CREATE AND CONTINUE`

![image_tag](/static/Lab1_gcpaddon/image3.png) 

>[!NOTE]
>When adding roles you can type the name you are looking for in the Filter by or permission section

- In the Select a Role section find and select `Compute Admin`

![image_tag](/static/Lab1_gcpaddon/image4.png) 

- Select `+ ADD ANOTHER ROLE` and then this time add `Logs Configuration Writer`
- Repeate the steps above to now add
    - `Logs Viewer`
    - `Monitoring Viewer`
    - `Storage Admin`
    - `Storage Object Viewer`
    - `Viewer`
    - `Pub/Sub Viewer` (don't accidentally click Pub/Sub lite!!)
    - `Pub/Sub Subscriber` (don't accidentally click Pub/Sub lite!!)

![image_tag](/static/Lab1_gcpaddon/image5.png) 
- Click `Continue` when ready
- Click `Done`to create account
- `Select your new account` from the list
- Select the `KEYS` tab
- Select the `ADD KEY` drop down followed by `Create new key`

![image_tag](/static/Lab1_gcpaddon/image8.png) 

- Make sure `JSON` is set as the Key type
- Click `Create`
- `Save your JSON file` somewhere on your computer to use for later (don't for forget where you saved it!)
- Click `Close` to close off the Private key saved dialog screen

### Configure your Google Cloud Add-on in Splunk
- From your Splunk console on the top left hand side. Select `Apps`, followed by the `Splunk Add-on for Google Cloud Platform`

>[!NOTE]
>By default the Configuration section of the add-on should be open. If it isn't then click the `Configuration tab` in the middle as shown in image below

![image_tag](/static/Lab1_gcpaddon/image10.png)

- From the Google Credentials tab select the green `Add button` on the right hand side 
- From the Add Google Credentials Page, enter in a name for your GCP credentials of `gcp_creds`
- Make sure `Account Type` is set to `Service Account`
- `Open the credentals file` you downloaded earlier in the lab and copy and paste the `entire JSON string` into the `Account Creds box`. See example below

![image_tag](/static/Lab1_gcpaddon/image11.png)

>[!NOTE]
> The image does not show the full JSON text as it is too large and is a scroll bar.  

- Click Add when done

### Create Inpout for Compute Engine metadata
- While still in the Google Cloud Add-on Configuration page, select the `Inputs tab` top left hand side
- From the right hand side now, select the `drop down` where it says `Create New Input` 

![image_tag](/static/Lab1_gcpaddon/image12.png)

- Select `Resource Metadata` option followed by the `Compute Engine` option
- Enter in a name of `resources`
- Select the `drop down for credentials` and `select the credentals you created earlier`
- If authenticated sucessfully you should see a project ID in the drop down for Project. Select `project id` 

>[!NOTE]
> If there is an error at this point then you may have incorrectly entereed the credentials JSON file earlier in the lab. Please re-try the steps again in the lab

- Select `us-central1-a, us-cental1-b, us-cental1-c and us-cental1-f` in the drop down for Zones. 
- `Leave all API settings as default`
- For index, select `default` and click the `cross (x)` to then bring up list of other indexes. select the `gcp-data` index. 
- Leave sourcetype as `google:gcp:resource:metadata`
- Once completed your panel should look like the below:

![image_tag](/static/Lab1_gcpaddon/image14.png)

- If happy, click `Add`
- Your first input should now appear in list

### Create Input for Compute Engine metadata
- From the Input page again, click `Create New Input`
- This time select `Cloud Monitoring` (note you may need to go back first as it will remember the input selection from last time)

![image_tag](/static/Lab1_gcpaddon/image16.png)

- Enter in a name of `monitoring`
- Select the `same credentials` as last time
- Select the `same project` as you did before
- Leave `Monitored Project` as `ALL`
- Under `Cloud Monitor Metrics` select `compute.googleapis.com/instance/cpu/usage_time`

>[!NOTE]
>This may take a little while to respond on the monitor metrics because it has to build out the list. It is also recommended you copy and paste from above to make it easier to find right one.

- Leave `Internal as default` of 300
- Leave `Start Date time as default` of 2024-09-21T19:43:45
- Under `index` select default then the cross then select `gcp-data`
- You should now see the following. 

![image_tag](/static/Lab1_gcpaddon/image17.png)

- Click `Add` when happy
- You should now have two inputs like below

![image_tag](/static/Lab1_gcpaddon/image18.png)

### Create Pub/Sub Input
- Again click `Create New Input` 
- Select `Cloud Pub/Sub`
- Name your input `logging`
- `Select your GCP credentials` again (same as last time)
- `Select your GCP project` again (same as last time)
- Under Pub/Sub Subscriptions you should see the one you created earlier (eg splunk-ta-topic_sub). `Select the pub/sub from the list`. 
- Again under Index click the `x` from index and select the `gcp-data` index again. 
- You should now see the following. 

![image_tag](/static/Lab1_gcpaddon/image33.png)

- Click `Add` when happy
- You should now have three inputs like below

![image_tag](/static/Lab1_gcpaddon/image34.png)

### Validate our data in Splunk
Now its time to check if our data is able to be PULL out of our GCP Cloud project. 
- Click `Apps` from the top left hand side of Splunk and then select `Search and Reporting`
- `Copy and paste` the following `SPL` into the search 

```text
index="gcp-data"
```
>[!TIP]
> Do you see data? 
> Select the sourcetype field under Selected fields on the left hand side. 
> How many sourcetypes do you see? If you got `three` then well done! 

![image_tag](/static/Lab1_gcpaddon/image35.png)

### Now let's try something a little more advanced. Let's see if we can check which service accounts have been created by whom?
- `Copy and  paste` the following `SPL` below into the search

```text
index="gcp-data" data.resource.type="service_account" data.protoPayload.methodName="google.iam.admin.v1.CreateServiceAccount"
| rename data.protoPayload.authenticationInfo.principalEmail as "Principal Email"
| rename data.protoPayload.requestMetadata.callerIp as "Source IP"
| rename data.protoPayload.requestMetadata.callerSuppliedUserAgent as "User Agent"
| rename data.protoPayload.response.email as "Service Account Email"
| rename data.protoPayload.response.project_id as Project
| table _time, "Principal Email", "Source IP", "User Agent", Project, "Service Account Email"
```
>[!TIP]
> Who was the person creating the splunk-ta services account?? It was us! Trick question!

### Now we want to find out Who created a service account and which IP it came from? 
- `Copy and  paste` the following `SPL` below into the search

```text
index="gcp-data" data.resource.type="service_account" data.protoPayload.methodName="google.iam.admin.v1.CreateServiceAccountKey"
| rename data.protoPayload.authenticationInfo.principalEmail as "Principal Email"
| rename data.protoPayload.requestMetadata.callerIp as "Source IP"
| rename data.protoPayload.requestMetadata.callerSuppliedUserAgent as "User Agent"
| rename data.protoPayload.response.name as "Key Name"
| rename data.protoPayload.response.valid_after_time.seconds as "Valid After"
| rename data.protoPayload.response.valid_before_time.seconds as "Valid Before"
| eval "Valid After"=strftime('Valid After', "%F %T")
| eval "Valid Before"=strftime('Valid Before', "%F %T")
| eval "Private Key Type" = case('protoPayload.request.private_key_type' == 0, "Unspecified", 'protoPayload.request.private_key_type' == 1, "PKCS12", 'protoPayload.request.private_key_type' == 2, "Google JSON credential file")
| table _time, "Principal Email", "Source IP", "User Agent", "Key Name", "Private Key Type", "Valid After", "Valid Before"
```
>[!TIP] 
> So who then exported a key... It was us again. Another trick question!

#### Congratulations!!! 
You have now completed Lab 1. You can now move on to Lab 2 (when told to do so) or go back to main page. 

### Click <a>[DONE](/README.md) to go back to the main page.</a>