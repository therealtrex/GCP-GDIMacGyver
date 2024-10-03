# Lab 2: Ingesting GCP cloud logging data into Dataflow to Splunk HEC (PUSHL) method. 
Welcome to Lab 2. Let's get into it! 

>[!NOTE]
>Usually this lab would have a two pre-requisites of the <a>[GCP add-on](https://splunkbase.splunk.com/app/3088)</a> installed on your Splunk intallation and a Splunk index to store all the data. To make things simpler we have already done this.
>Throughout this lab whenever you are prompted to enter a Splunk index please use <b>gcp_data</b> 

### Create Dataflow HEC token
Before we begin we will create a HEC token in Splunk for Dataflow to use. 
- In your Splunk console. `Select Settings`, then `Data Inputs`

![image_tag](/static/Lab2_dataflow/image6.png)

- Click `+ Add new` on the right hand side `next to HTTP Event Collector`
- Name your HEC token `gcp-dataflow`
- `Click next` to continue

![image_tag](/static/Lab2_dataflow/image7.png)

- In the Input settings page. Select the button `select` in the sourcetype section and then `click the drop down` and find and `select google:gcp:pubsub:message` sourcetype. 
- Select gcp_data as your index. This will then automatically make it the default index as well. 

![image_tag](/static/Lab2_dataflow/image8.png)

- `Click Review` button
- `Click Submit` when ready
- `Copy your Token` to use later in this lab

![image_tag](/static/Lab2_dataflow/image9.png)

>[!NOTE]
>You will noticed we did not have HEC acknowledgement enabled on this HEC token. That is because the Google Dataflow template does not have the ability to check the acknowledgement of indexed data in Splunk. 

### Update service account permissions
Before we can use this lab we need to update the google service account details that Dataflow will use to publish. 

- From your Google Cloud Console. `Select The Top left hand handburger` followed by `IAM & Admin`

![image_tag](/static/Lab2_dataflow/image1.png)

- From the IAM screen you will see a number of accounts. There should be one called `<number>-compute@developer.gserviceaccount.com`
- `Click the Pencil Icon` on the right-hand side next to this service account
- From the `Role drop down` where it currently should say `Editor`. Select this and change it to `Dataflow Admin`.
- Click `+ ADD ANOTHER ROLE` and also add `Daaflow Worker` and then `Pub/Sub Subscriber`
- Once compelted your role should now look like this.

![image_tag](/static/Lab2_dataflow/image2.png)

- When happy click Save to save changes. 

### Create storage bucket for dataflow
- From your Google Cloud Console. `Select The Top left hand handburger` followed by `Cloud Storage`
- Click the `+ CREATE` button
- On the Get started section. Enter a unique name for your bucket. Eg `<gcp-project-id->-dataflow`

![image_tag](/static/Lab2_dataflow/image3.png)

- `Click Continue`
- `Click the Region radio button` and `leave the defaulted region` in the drop down (should be us-east1 (South Carolina))

![image_tag](/static/Lab2_dataflow/image4.png)

- Leave all remaining defaults and `click CREATE`
- `Click CONFIRM` if prompted for `Public access will be prevented` panel appears. 

![image_tag](/static/Lab2_dataflow/image5.png)

### Create a Pub/Sub Topic
- From your Google Cloud Console. `Select The Top left hand handburger` followed by `VIEW ALL PRODUCTS`

![image_tag](/static/Lab2_gcpaddon_cli/NEEDONE.png)

- Select `Analytics` on the left hand menu then `select Pub/Sub`
- From the Pub/Sub screen select `+ CREATE TOPIC`
- Name your topic `splunk-dataflow-topic`
- `Click Create` leaving all other defaults

![image_tag](/static/Lab2_gcpaddon/NEEDOE.png)

>[!NOTE]
> This will take around 15 seconds to create.

### Create a Log Sink
- From your Google Cloud Console click the `hamburger again` following by `VIEW ALL PRODUCTS`
- `Select Observability`
- `Select Logging`

![image_tag](/static/Lab1_gcpaddon/image23.png)

- On the left hand menu (you may need to hover your mouse over it to expand it) `select Log Router` (near the bottom)

![image_tag](/static/Lab1_gcpaddon/image24.png)

- `Select Create sink`
- name the sink `splunk-dataflow-sink`
- click next

![image_tag](/static/Lab1_gcpaddon/image25.png)

- For Sink destination `select Splunk` as the sing service and the `splunk-dataflow-topic` Pub/Sub topic we just created in the Pub/Sub topic; see example below

![image_tag](/static/Lab1_gcpaddon/image26.png)

>[!NOTE]
> Do not accidentally select then non dataflow one we created from Lab1. 

- Click Next
- Leave Choose logs to include in sing as empty (default) and click Next

>[!NOTE]
> This means we will be sending everything by default

![image_tag](/static/Lab1_gcpaddon/image27.png)

- `Select + Add exclusion` for Choose logs to filter out of sink(optional)
- enter in `exclude_dataflow` as Exclusion filter name
- `copy and paste` the below in for the exclusion inside the Build an exclusion filter box

```test
resource.type="dataflow_step"
```
![image_tag](/static/Lab1_gcpaddon/image28.png)

>[!WARNING]
> If we don't put this exclusion in then the logs from dataflow will be sent to dataflow creating more logs and therefore we would have just created a log infinite loop!

- `Click Create sink` when ready
- Once completed it should say this

![image_tag](/static/Lab1_gcpaddon/image29.png)

## Creating Dead Letter Queue
- From your Google Cloud Console. `Select The Top left hand handburger` followed by `VIEW ALL PRODUCTS`

![image_tag](/static/Lab2_gcpaddon_cli/image10.png)

- Select `Analytics` on the left hand menu then `select Pub/Sub`
- From the Pub/Sub screen select `+ CREATE TOPIC`
- Name your topic `splunk-dataflow-deadletter-topic`
- `Click Create` leaving all other defaults

![image_tag](/static/Lab2_gcpaddon_cli/image11.png)

- While on the Pub/Sub topic page under the Subscriptions section, select CREATE SUBSCRIPTION

![image_tag](/static/Lab2_gcpaddon_cli/image12.png)

- Enter `deadletter` as Subscription ID
- Leave all other defaults, scroll down and `click Create`

![image_tag](/static/Lab2_gcpaddon_cli/image13.png)

- Whle on the pub/sub page, select topics


### Deploying Pub/Sub Dataflow to Splunk template






```test
index="gcp_data"
```


#### Congrautations!!! 
You have now completed Lab 1. You can now move on to Lab 2 (when told to do so) or go back to main page. 

### Click <a>[Next](/content/Lab1_awsaddon/setup_aws_sqs.md)</a> to continue or click <a>[Back](/README.md) to go back to the beginning</a>