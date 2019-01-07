# Use go SDK {#concept_mkk_vpj_zdb .concept}

This document introduces how to install and call Alibaba Cloud Go SDK.

## Installation {#section_kjb_1qj_zdb .section}

Alibaba Cloud Go SDK supports Go 1.7 or later. You can install Go SDK using the following methods:

-   Use Glide \(recommended\)

    ```
    glide get github.com/aliyun/alibaba-cloud-sdk-go
    ```

-   Use govendor

    Run the following command to install Go SDK:

    ```
    go get -u github.com/aliyun/alibaba-cloud-sdk-go/sdk
    ```


## Set up credentials {#section_vjb_1qj_zdb .section}

When using the Alibaba Cloud Go SDK to access Alibaba Cloud services, you need an Alibaba Cloud account for authentication.

Go SDK supports the following authentication methods.

|Authentication|Description|
|:-------------|:----------|
|AccessKey|Use the AccessKeyID/Secret to do the authentication|
|StsToken|Use the STS Token to do the authentication|
|RamRoleArn|Use the AssumeRole of the RAM account to do the authentication|
|EcsRamRole|Use the RAM role of an ECS instance to do the authentication|
|RsaKeyPair|Use the RSA key pair to do the authentication \(supported only on Japan site\)|

This document uses AccessKey as an example to illustrate how to set up credentials. To ensure the security of your account, we recommend using your RAM account instead of the primary account. The primary account has full access to all of your cloud services, while the RAM account has limited access granted by the primary account to the cloud services. Firstly, create an AccessKey as described in ,

and then set up your credentials when initializing AcsClient as follows:

**Note:** Do not disclose any code containing your AccessKey \(do not commit the code to public GitHub projects\). Otherwise, your Alibaba Cloud account may be compromised.

```
// Create the ecsClient instance
ecsClient, err := ecs.NewClientWithAccessKey(
        "<your-region-id>", // your region ID
        "<your-access-key-id>", // your AccessKey ID
        "<your-access-key-secret>") // your AccessKey Secret
```

If you want to use a custom Config or credentials other than AccessKey, you must first create a `Credential` object to save the credentials, and then submit the `Credential` object when initiating the client.

```
// Customize config
config := NewConfig()
// Create a credential object
credential := &credentials.BaseCredential{
    AccessKeyId: "<your-access-key-id>",
    AccessKeySecret: "<your-access-key-secret>",

// Initiate the client
client, err := NewClientWithOptions("cn-hangzhou", config, credential)
```

## Call a service {#section_bkb_1qj_zdb .section}

This document uses ECS as an example to illustrate how to use Alibaba Cloud Go SDK to initiate a request:

1.  Import Alibaba Cloud Go SDK.

    ```
    import github.com/aliyun/alibaba-cloud-sdk-go/services/ecs
    ```

2.  Create an ECS client.

    ```
    ecsClient := ecs.NewClientWithAccessKey(
        "<your-region-id>",                 
        "<your-access-key-id>",                
    	"<your-access-key-secret>")
    ```

3.  Use the `xxx.CreateXXXRequest` method to create a request, and assign values to related request parameters.

    The naming convention for the method is `${service}. Create${apiName}Request`, where:

    -   $\{service\} is the product name in lower case, such as ecs, oss, and slb.
    -   $\{apiName\} is the API name, such as DescribeInstanceAttribute.
    ```
    
    request := ecs.CreateDescribeInstanceAttributeRequest()
    request.InstanceId = ""
    ```

4.  Initiate a call.

    ```
    response, err := ecsClient.DescribeInstanceAttribute(request)
    ```

5.  Handle the response.

    All the returned attributes are deserialized into the response. You can directly call `response.getXXX()` to obtain the response attributes.

    ```
    instanceStatus := response.Status
    ```

    If an error `(err nil)` occurs or you want to get the original HTTP response, you can obtain the original data as follows:

    ```
    // Obtain the original http response, and the response type is *http.Response
    httpResponse := response.GetOriginHttpResponse()
    
    // Obtain the status code of the original http response, and the response type is int
    statusCode := response.GetHttpStatus()
    
    // Obtain the header of the original http response, and the response type is map[string][]string
    headers := response.GetHttpHeaders()
    
    // Obtain the content of the original http response, and the response type is []byte
    contentBytes := response.GetHttpContentBytes()
    
    // Obtain the content of the original http response and transcode it to string according to UTF-8, and the response type is string.
    contentString := response.GetHttpContentString()
    ```


