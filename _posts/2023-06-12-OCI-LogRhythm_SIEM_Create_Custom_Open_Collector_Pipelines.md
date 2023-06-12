---
layout: post
title:  "LogRhythm-SIEM_Create_Custom_Open_Collector_Pipelines_For_Kafkabeat"
date:   2023-06-12 21:21:06 +0530
categories: [oracle_cloud_infrastructure, OCI_SIEM_Integration, LogRhythm_Open_Collector]
tags: [OCI, OCI_Kafka_Streaming, SIEM, Audit, LogRhythm, OpenCollector, Kafkabeat]
---


## Creating Custom Open Collector Pipelines For Kafkabeat.

By default LR OpenCollecor does not have pipelines to process kafkabeat logs.
Run below command to view the list of available pipelines in OC. We noticed by default OC does not have pipelines to process kafkabeat logs

```bash
./lrctl oc  -  - pipe status
```

![Img1](/assets/img/posts/2023-06-12/1.png){: width="700"}

### Steps to create a custom pipeline for kafkabeat.

* Install ocpipeline utility

     ```bash
    ./lrctl ocpipeline  install
    ```
* Install lrjq utility

    ```bash
    ./lrctl lrjq install
    ```
* ![Img2](/assets/img/posts/2023-06-12/2.png){: width="700"}


    ```bash
        ./ocpipeline create --name ociKafkaStream --destination .
     ```



![Img3](/assets/img/posts/2023-06-12/3.png){: width="700"}

* Paste the content of zip file to folder `ociKafkaStream`. 
    
* To Understand the content of zip file go through [Logrhythm Official document](https://docs.logrhythm.com/docs/OCbeats/logrhythm-open-collector/create-custom-open-collector-pipelines).

  [ocikafkaStream.zip](/assets/img/posts/2023-06-12/ociKafkaStream.zip)
    
* Run below command to Package and import the pipeline.

    ```bash
    ./ocpipeline package --source ociKafkaStream --destination . –-force
     ```
* We will see below output once above command executed successfully.

    ```bash
    .pipe file generated at: /root/ociKafkaStream.pipe
    ```

![Img4](/assets/img/posts/2023-06-12/4.png){: width="700"}

* Import .pipe file into your Open Collector.

    ```bash
    cat ociKafkaStream.pipe  | ./lrctl oc pipe import
    ```

![Img5](/assets/img/posts/2023-06-12/5.png){: width="700"}

* Restart Services.
    ```bash
    ./lrctl metrics restart
    ```
    ```bash
    ./lrctl oc restart
    ```

![Img6](/assets/img/posts/2023-06-12/6.png){: width="700"}

* Validate the status os all pipeline 

    ```bash
    ./lrctl oc  -  - pipe status 
    ```

![Img7](/assets/img/posts/2023-06-12/7.png){: width="700"}





