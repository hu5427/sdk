# Get started {#concept_b43_j4j_zdb .concept}

Welcome to use the Alibaba Cloud Software Development Kit \(SDK\). The Alibaba Cloud SDK for Java allows you to use Alibaba Cloud resources such as Elastic Compute Service \(ECS\), Server Load Balancer \(SLB\), and CloudMonitor without complex coding to build your own cloud Java applications. This tutorial provides a step-by-step guidance on setting up a development environment in Java. If you have any problem when using Alibaba Cloud SDK, please join the DingTalk group with ID 11771185 \(the official SDK customer service group of Alibaba Cloud\) for consultation.

## Prerequisites {#section_xpr_q4j_zdb .section}

-   An Alibaba Cloud account, and the corresponding Access Key ID and secret. You can create and get your AccessKey on the [Alibaba Cloud console](https://usercenter.console.aliyun.com/?spm=5176.doc52740.2.3.QKZk8w#/manage/ak), or contact your administrator.
-   To use Alibaba Cloud Java SDK to call APIs of a product, you must first activate the product on the Alibaba Cloud console if required.
-   JDK 1.6 or later.

## Install Java SDK {#section_hts_q4j_zdb .section}

If you use Apache Maven to manage Java projects, you only need to add corresponding dependencies to the pom.xml file of the projects.  You can download Maven dependencies of different cloud products from [Alibaba Cloud GitHub](https://github.com/aliyun/aliyun-openapi-java-sdk).

The core library must be installed no matter which cloud product to use. For example, you must install both the ECS SDK library and the SDK core library if you want to use ECS resources.

If the version of the core library is 3.7.0 and the version of ECS library is 4.11.0, you must declare these two SDK libraries in the pom.xml file as follows.

```
<dependency>
    <groupId>com.aliyun</groupId>
    <artifactId>aliyun-java-sdk-core</artifactId>
    <version>3.7.0</version>
</dependency>
<dependency>
    <groupId>com.aliyun</groupId>
    <artifactId>aliyun-java-sdk-ecs</artifactId>
    <version>4.11.0</version>
</dependency>
```

## Use Java SDK {#section_m4y_npj_zdb .section}

The following code example shows the three main steps to use Alibaba Cloud Java SDK:

1.  Create and initiate the DefaultAcsClient instance.
2.  Create an API request and set parameters.
3.  Initiate the request and process the response.

    ```
    package com.testprogram;
     import com.aliyuncs.profile.DefaultProfile;
     import com.aliyuncs.DefaultAcsClient;
     import com.aliyuncs.IAcsClient;
     import com.aliyuncs.exceptions.ClientException;
     import com.aliyuncs.exceptions.ServerException;
     import com.aliyuncs.ecs.model.v20140526.*;
     public class Main {
         public static void main(String[] args) {
             // Create and initialize a DefaultAcsClient instance
             DefaultProfile profile = DefaultProfile.getProfile(
                 "<your-region-id>", // Region ID
                 "<your-access-key-id>", // The AccessKey ID of the RAM account
                 "<your-access-key-secret>"); // The AccessKey Secret of the RAM account
             IAcsClient client = new DefaultAcsClient(profile);
             // Initiate the request and handle the response or exceptions
             DescribeInstancesRequest request = new DescribeInstancesRequest();
             request.setPageSize(10);
             request.setConnectTimeout(5000); // Set the connection timeout to 5000 milliseconds
             request.setReadTimeout(5000); // Set the read timeout to 5000 milliseconds
             // Initiate the request and process the response
             DescribeInstancesResponse response;
             try {
                 response = client.getAcsResponse(request);
                 for (DescribeInstancesResponse.Instance instance:response.getInstances()) {
                     System.out.println(instance.getPublicIpAddress());
                 }
             } catch (ServerException e) {
                 e.printStackTrace();
             } catch (ClientException e) {
                 e.printStackTrace();
             }
         }
     }
    ```


