# Ingesting GCP cloud logging data into Dataflow to Splunk HEC (PUSHL) method. 
Welcome to Lab 2. Let's get into it! 

>[!NOTE]
>Usually this lab would have a two pre-requisites of the <a>[GCP add-on](https://splunkbase.splunk.com/app/3088)</a> installed on your Splunk intallation and a Splunk index to store all the data. To make things simpler we have already done this.
>Throughout this lab whenever you are prompted to enter a Splunk index please use <b>gcp-data</b> 

### Create Dataflow HEC token
Before we begin we will create a HEC token in Splunk for Dataflow to use. 
- In your Splunk console. select `Settings`, then `Data Inputs`

![image_tag](/static/Lab2_dataflow/image6.png)

- Click `+ Add new` on the right hand side `next to HTTP Event Collector`
- Name your HEC token `gcp-dataflow`
- `Click next` to continue

![image_tag](/static/Lab2_dataflow/image7.png)

- In the Input settings page. Select the button `select` in the sourcetype section and then click the `drop down` and find and select the `google:gcp:pubsub:message` sourcetype. 
- Select `gcp-data` as your index. This will then automatically make it the default index as well. 

![image_tag](/static/Lab2_dataflow/image8.png)

- Click `Review` button
- Click `Submit` when ready
- `Copy your Token` to use later in this lab

![image_tag](/static/Lab2_dataflow/image9.png)

>[!NOTE]
>You will noticed we did not have HEC acknowledgement enabled on this HEC token. That is because the Google Dataflow template does not have the ability to check the acknowledgement of indexed data in Splunk. 

### Create Datafow Topic
- From GCP console, select the top left `hamburger` (three bars) and select `VIEW ALL PRODUCTS`

![image_tag](/static/Lab2_dataflow/image10.png)

- Select `Analytics` on the left hand menu then select `Pub/Sub`
- From the Pub/Sub screen select `+ CREATE TOPIC`
- Name your topic `splunk-dataflow-topic`
- Click `Create` leaving all other defaults

>[!NOTE]
> This will take around 15 seconds to create.

>[!IMPORTANT]
> The Google UI (different to the CLI) will automatically create a subscription for you whenever you createa a topic. This will be the same name as the topic with `_sub` appended to it.
> For this lab a subscription of `splunk_dataflow_topic_sub` will automatically be created. You can view this in the Google Console under Pub/Sub subscriptions.


### Create a Log Sink
- From GCP console, select the top left `hamburger` (three bars) and select `VIEW ALL PRODUCTS`
- Select `Observability`
- Select `Logging`

![image_tag](/static/Lab2_dataflow/image23.png)

- On the left hand menu (you may need to hover your mouse over it to expand it) select `Log Router` (near the bottom)

![image_tag](/static/Lab2_dataflow/image24.png)

- Select `Create sink`
- name the sink `splunk-dataflow-sink`
- click next

![image_tag](/static/Lab2_dataflow/image25.png)

- For Sink destination select `Splunk` as the sink service and the `splunk-dataflow-topic` Pub/Sub topic we just created in the Pub/Sub topic; see example below

![image_tag](/static/Lab2_dataflow/image26.png)

- Click `Next`
- `Leave Choose logs to include in sink as empty` (default) and click `Next`

>[!NOTE]
> This means we will be sending everything by default

![image_tag](/static/Lab2_dataflow/image27.png)

- Select `+ Add exclusion` for Choose logs to filter out of sink(optional)
- enter in `exclude_dataflow` as Exclusion filter name
- `copy and paste` the below in for the exclusion inside the Build an exclusion filter box

```text
resource.type="dataflow_step"
```
![image_tag](/static/Lab2_dataflow/image28.png)

>[!WARNING]
> If we don't put this exclusion in then the logs from dataflow will be sent to dataflow creating more logs and therefore we would have just created a log infinite loop!

- Click `Create sink` when ready
- Once completed it should say this

![image_tag](/static/Lab2_dataflow/image29.png)

### Update service account permissions
Before we can use this lab we need to update the google service account details that Dataflow will use to publish. 
- From GCP console, select the top left `handburger` (three bars) and select `IAM & Admin`

![image_tag](/static/Lab2_dataflow/image1.png)

From the IAM screen you will see a number of accounts. There should be one called `<number>-compute@developer.gserviceaccount.com`
- Click the `pencil icon` on the right-hand side next to this service account
- From the `Role drop down` where it currently should say `Editor`. Select this and change it to `Dataflow Admin`.
- Click `+ ADD ANOTHER ROLE` and also add `Dataflow Worker` and then `Pub/Sub Subscriber`
- Once completed your role should now look like this.

![image_tag](/static/Lab2_dataflow/image2.png)

- When happy click `SAVE` to save changes. 

### Create storage bucket for dataflow
- From GCP console, select the top left `handburger` (three bars) and select `Cloud Storage`
- Click the `+ CREATE` button
- On the Get started section. Enter a unique name for your bucket. Eg `<gcp-project-id->-dataflow`

![image_tag](/static/Lab2_dataflow/image3.png)

- Click `Continue`
- Click the `Region` radio button and `leave the defaulted region` in the drop down (should be us-east1 (South Carolina))

![image_tag](/static/Lab2_dataflow/image4.png)

- Leave all remaining defaults and click `CREATE`
- Click `CONFIRM` if prompted for `Public access will be prevented` panel appears. 

![image_tag](/static/Lab2_dataflow/image5.png)

### Creating Dead Letter Queue
- From GCP console, select the top left `hamburger` (three bars) and select `VIEW ALL PRODUCTS`

![image_tag](/static/Lab2_dataflow/image10.png)

- Select `Analytics` on the left hand menu then select `Pub/Sub`
- From the Pub/Sub screen select `+ CREATE TOPIC`
- Name your topic `splunk-dataflow-dl-topic`
- Click `Create` leaving all other defaults

![image_tag](/static/Lab2_dataflow/image11.png)

>[!IMPORTANT]
> The Google UI (different to the CLI) will automatically create a subscription for you whenever you createa a topic. This will be the same name as the topic with `_sub` appended to it.
> For this lab a subscription of `splunk-dataflow-dl-topic_sub` will automatically be created. You can view this in the Google Console under Pub/Sub subscriptions.
> This will be required later on in the lab. 

Before moving on you will noticed the two subscriptions which now exist after creating our two topics. See example image below. 

![image_tag](/static/Lab2_dataflow/image49.png)

### Deploying Pub/Sub Dataflow to Splunk template
- From GCP console, select the top left `hamburger` (three bars) and select `VIEW ALL PRODUCTS`

![image_tag](/static/Lab2_dataflow/image10.png)

- Select `Analytics` and then select `Dataflow`
- Select `CREATE JOB FROM TEMPLATE` 

![image_tag](/static/Lab2_dataflow/image48.png)

- Enter in `splunk-dataflow` for Job Name
- `Leave region as default` eg us-central1 (Iowa)
- In the Dataflow Tempalte area select it and type in `Splunk` to filter down the results. Select `Pub/Sub to Splunk template` 

![image_tag](/static/Lab2_dataflow/image36.png)

and

![image_tag](/static/Lab2_dataflow/image37.png)

>[!NOTE]
> Afer a few seconds more fields will appear below for you to fill in. 

>[!IMPORTANT]
> The next section of the lab is tricky. Please take your time and reade the steps carefully! When it doubt ask your instructor!


- Enter in the following details required for the Pub/Sub Dataflow template
    - Pub/Sub Input Subscription: `projects/<your-project-id>/subscriptions/splunk-dataflow-topic-sub`
    - Splunk HEC url: This is your Splunk Instance URL with 8088 port at the end. `EXAMPLE ONLY https://trex-i-01fff791c4c427ceb.splunk.show:8088`
    - Output Deadletter Pub/Sub Topic: `projects/<your-project-id>/topics/splunk-dataflow-dl-topic`
    - Leave streaming mode as defaults to Exactly Once
    - Temporaray Location: Click `Browse` and select the `dataflow bucket we created earlier` eg: <your-project-id>-dataflow. 
    - Click on `SELECT`
    - Click on the Temporary location and add a `/tmp` on the end. See example below
    ![image_tag](/static/Lab2_dataflow/image38.png)
    
    >[!NOTE]
    > You Temporary location field should go from RED to GREEN tick

    - Leave Encryption set as default (Google-managed encryption key)
    - Leave default settings for both Dataflow Prime (unticked) 
    

- Optional Parameters Section
    - HEC Authenticatoin Token: `Paste in the HEC token you created earlier`
    - Batch Size: `10`
    - Leave Disable SSL certifications validation (UNTICKED)
    - Maximum number of parallel requests: `4`
    - Leave other defatuls scrolling down to `Max Workers` and set to `2`
    - Click drop down for Workers Region and select `Use Jobs Regoinal Endpoint`

    ![image_tag](/static/Lab2_dataflow/image50.png)


    - `Untick Use Default machine type box`
    - `Select series E2`
    - make sure `e2-medium` is selected 
    - Leave remaining defaults and select `RUN JOB`

- Once you click RUN JOB look at the right hand side for status. It should change from `Starting...` to `Running`

![image_tag](/static/Lab2_dataflow/image39.png)

to

![image_tag](/static/Lab2_dataflow/image40.png)

- You will also see when the tempalte is deployed there is extra information displayed

![image_tag](/static/Lab2_dataflow/image40.png)


### Time to explore some data
- From your Splunk Console select Apps, Search and Reporting

![image_tag](/static/Lab2_dataflow/image41.png)

Let's just try a basic search again

- Type the following into Splunk Search bar and hit enter

```text
index="gcp-data"
```
- Got data like the below coming in? Well done! 

![image_tag](/static/Lab2_dataflow/image42.png)

- Now let's play around a little. Click the sourcetype fields, how many do you have coming in? 
- Do you have around 3? Notice its more than one? 

![image_tag](/static/Lab2_dataflow/image43.png)

- In our example we have three
    google:gcp:pubsub:audit:admin_activity
    google:gcp:pubsub:message
    google:gcp:pubsub:platform
- This is because dataflow automatically assigns the sourcetypes basd based on the data from the template. 

#### Congrautations!!! 
You have now completed Lab 2. 

### Click <a>[Done](/README.md) to go back to the beginning</a>