# Use Java SDK {#concept_mkk_vpj_zdb .concept}

This document introduces how to install and use Alibaba Cloud Java SDK.

## Install Java SDK {#section_kjb_1qj_zdb .section}

Alibaba Cloud Java SDK supports JDK 1.6 or later. You can install the Java SDK using the following two methods:

-   Use Maven \(recommended\)

    If you use Maven to manage project dependencies, add the following code blocks to the `pom.xml` file to install Java SDK :

    ```
    <dependency>
        <groupId>com.aliyun</groupId>
        <artifactId>aliyun-java-sdk-core</artifactId>
        <version>3.5.0</version>
    </dependency>
    <dependency>
        <groupId>com.aliyun</groupId>
        <artifactId>aliyun-java-sdk-ecs</artifactId>
        <version>3.0.0</version>
    </dependency>
    ```

-   Import JARs to the integrated development environment

**Note:** This installation method will be phased out in the next major version, and will only support the Maven installation method.

    If you are using Eclipse or IntelliJ, you can install the Java SDK directly by importing JAR files. You can download the JAR files of cloud products in [Alibaba Cloud SDK](https://develop.aliyun.com/tools/sdk?spm=5176.doc52740.2.4.wQVCcn#/java).

    -   Use Eclipse

        Complete the following steps to install Alibaba Cloud Java SDK:

        1.  Copy the downloaded `aliyun-java-sdk-xxx.jar` file to your project folder.
        2.  In Eclipse, right click the project, and then select **Properties**.
        3.  In the displayed dialog box, click**Java Build Path** \> **Libraries** \> **Add JARs**to add the downloaded JAR files.
        4.  Click **Apply and Close**.
    -   Use IntelliJ

        Complete these steps to install Alibaba Cloud Java SDK using IntelliJ:

        1.  Copy the downloaded `aliyun-java-sdk-xxx.jar` files to your project folder.
        2.  Open your project in IntelliJ, and click**File** \> **Project Structure**.
        3.  Click **Apply**, and then click **OK**.

## Set up credentials {#section_vjb_1qj_zdb .section}

When using the Alibaba Cloud Java SDK to use Alibaba Cloud resources, you have to provide your identification for authentication.

Java SDK supports the following authentication methods:

|Authentication|Description|
|:-------------|:----------|
|AccessKey|Use the AccessKey to complete the authentication.|
|STS Token|Use the STS Token to complete the authentication.|
|RamRoleArn|Use the AssumeRole of the RAM account to complete  the authentication.|
|EcsRamRole|Use the RAM role of an ECS instance to complete the authentication.|
|RsaKeyPair|Use the RSA key pair to complete the authentication \(supported only on Japanese site\)|

This document uses AccessKey as an example to illustrate how to set up credentials. To ensure the security of your account, it is recommend using your RAM account instead of the primary account. The primary account has full access to all of your cloud services, while the RAM account has limited access granted by the primary account to the cloud services. Firstly, create an AccessKey as described in ,

and then set up your credentials when initializing AcsClient as follows:

**Note:** Do not disclose any code containing your AccessKey, for example, do not commit the code to public GitHub projects. Otherwise, your Alibaba Cloud account may be compromised.

```
DefaultProfile profile = DefaultProfile.getProfile(
    "<your-region-id>", // region ID
    "<your-access-key-id>", //  AccessKey ID of RAM account
    "<your-access-key-secret>"); // AccessKey Secret of RAM account
```

## Initiate a call {#section_bkb_1qj_zdb .section}

This document uses ECS as an example to illustrate how to use Alibaba Cloud Java SDK to make a request:

1.  Create an AcsClient client.

    ```
    IAcsClient client = new DefaultAcsClient(profile);
    ```

2.  Create a request.

    The naming convention for requests is `${apiName}Request`. Where $\{apiName\} is the API name, such as DescribeInstances.

    When multiple product SDKs are used, different requests may have the same name. Differentiate the request according to the package.

    ```
    DescribeInstancesRequest request = new DescribeInstancesRequest();
    request.setPageSize(10);
    request.setConnectTimeout(5000); // Set the connection timeout to 5000 milliseconds
    request.setReadTimeout(5000); // Set the read timeout to 5000 milliseconds
    ```

3.  Make a call and handle the response.

    ```
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
    ```

    All the returned attributes are deserialized into the response. You can directly call `response.getXXX()` to obtain the response attributes.

    ```
    instanceStatus := response.getStatus()
    ```

    However, if an exception occurs or you want to obtain the original HTTP response, you can use the `doAction()` method to obtain the original response.

    ```
    HttpResponse response = client.doAction(request);
    ```


