# Get started {#concept_b43_j4j_zdb .concept}

The Alibaba Cloud Go Software Development Kit \(SDK\) allows you to access Alibaba Cloud services such as Elastic Compute Service \(ECS\), Server Load Balancer \(SLB\), and CloudMonitor. You can access Alibaba Cloud services without the need to handle API related tasks, such as signing and constructing your requests. This document introduces how to obtain and call Alibaba Cloud Go SDK. If you have any problem while using Go SDK, please join the DingTalk group: 11771185 \(the official SDK customer service group of Alibaba Cloud\) for consultation.

## Prepare the environment {#section_xpr_q4j_zdb .section}

-   To use Alibaba Cloud Go SDK, you must have an Alibaba Cloud account as well as an AccessKey. The AccessKey is required when initializing AcsClient. You can create an AccessKey in the [AccessKey console](https://usercenter.console.aliyun.com/?spm=5176.doc52740.2.3.QKZk8w#/manage/ak) of Alibaba Cloud console.
-   To use Alibaba Cloud Go SDK to access the APIs of a product, you must first activate the product on the Alibaba Cloud console if required.

## Install Go SDK {#section_hts_q4j_zdb .section}

Run the following commands to install Alibaba Cloud Go SDK:

```
glide get github.com/aliyun/alibaba-cloud-sdk-go
```

## Use Go SDK {#section_m4y_npj_zdb .section}

The following code example shows the three main steps to use Alibaba Cloud Go SDK:

1.  Create the client.
2.  Create an API request and set parameters.
3.  Initiate the request and handle exceptions.

    ```
    package main
    
    import (
        "github.com/aliyun/alibaba-cloud-sdk-go/services/ecs"
        "github.com/aliyun/alibaba-cloud-sdk-go/sdk/requests"
        "fmt"
    
    
    func main() { 
        // Create an ECS client
        ecsClient, err := ecs.NewClientWithAccessKey(
            "<Your-region-ID>", // your region ID
            "<Your-access-key-ID>", // your AccessKey ID
            "<your-access-key-secret>") // your AccessKey Secret
        if err ! = nil {
            // Handle exceptions
            panic(err)
        
         // Create an API request and set parameters
        request := ecs.CreateDescribeInstancesRequest()
        // Set the request.PageSize to "10"
        request.PageSize = requests.NewInteger(10)
        // Initiate the request and handle exceptions
        response, err := ecsClient.DescribeInstances(request)
        if err ! = nil {
            // Handle exceptions
            panic(err)
        
        fmt.Println(response)
    
    ```


