# Use .NET SDK {#concept_mkk_vpj_zdb .concept}

This document introduces how to install and call Alibaba Cloud .NET SDK.

## Install .NET SDK {#section_kjb_1qj_zdb .section}

Install the Alibaba Cloud .NET SDK using one of the following methods:

-   Add DLL reference
    1.  Download the DLL package from [.NET SDK](https://develop.aliyun.com/tools/sdk#/dotnet).
    2.  Right click your project in the Solution Explorer of Visual Studio and click **Reference**.
    3.  In the displayed menu, click **Add Reference**.
    4.  In the displayed dialog box, click **Browse**. Then select the downloaded DLL file and click **Confirm**.
-   Add project reference
    1.  Run the following command to clone the SDK source codes from GitHub.

        ```
        git clone https://github.com/aliyun/aliyun-openapi-net-sdk.git
        ```

        There are many folders prefixed with aliyun-net-openapi-XXXin the cloned directory. Each folder contains the \\\\\*.csproj file, which is the .NET project file. For example, there is an `aliyun-net-sdk-ecs.csproj` file under the `aliyun-net-openapi-ecs` subfolder.

    2.  In Visual Studio, right click your solution.
    3.  Click**Add** \> **Existing Project**.
    4.  In the displayed dialog box, select the project file, for example, `aliyun-net-sdk-ecs.csproj`, and click then **Open**.
    5.  Right click your project and click**Reference** \> **Add Reference**.

## Configure credentials {#section_vjb_1qj_zdb .section}

When using the Alibaba Cloud .NET SDK to access Alibaba Cloud services, you need an Alibaba Cloud account for authentication.

Currently, .NET SDK supports the following authentication methods:

|Authentication|Description |
|:-------------|:-----------|
|AccessKey|Use the AccessKey to complete the authentication.|
|STS Token|Use the STS Token to complete the authentication.|
|RamRoleArn|Use the AssumeRole of the RAM account to complete the authentication.|
|EcsRamRole|Use the RAM role of an ECS instance to complete the authentication.|

This document uses AccessKey as an example to illustrate how to set up credentials. To ensure the security of your account, we recommend using your RAM account instead of the primary account. The primary account has full access to all of your cloud services, while the RAM account has limited access granted by the primary account to the cloud services. Firstly, create an AccessKey as described in [Create an AccessKey](https://www.alibabacloud.com/help/doc-detail/66453.htm),

and then set up your credentials when initializing the Client as follows:

**Note:** Do not disclose any code containing your AccessKey \(do not commit the code to public GitHub projects\). Otherwise, your Alibaba Cloud account may be compromised.

```
IClientProfile clientProfile = DefaultProfile.GetProfile(
    "<your-region-id>", // region ID
    "<your-access-key-id>", //  AccessKey ID of RAM account
    "<your-access-key-secret>"); // AccessKey Secret of RAM account
```

## Initiate a call {#section_bkb_1qj_zdb .section}

This document takes ECS as an example to introduce how to use Alibaba Cloud .NET SDK to initiate a request.

1.  Creates a default client instance.

    ```
    DefaultAcsClient client = new DefaultAcsClient(clientProfile);
    ```

2.  Create a request.

    The naming convention for requests is `${apiName}Request`. Where $\{apiName\} is the API name, such as DescribeInstances.

    When multiple product SDKs are used, different requests may have the same name. Differentiate the requests according to the package.

    ```
    DescribeInstancesRequest request = new DescribeInstancesRequest();
    request.PageSize = 10;
    ```

3.  Initiate a call and process the response.

    ```
    DescribeInstancesResponse response = client.GetAcsResponse(request);
    ```

    Normally, all fields in the response will be deserialized to the response. You can obtain the fields in the response by calling `response.getXXX()`.

    ```
    System.Console.WriteLine(response.TotalCount);
    ```


