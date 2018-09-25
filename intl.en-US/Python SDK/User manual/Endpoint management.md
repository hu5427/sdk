# Endpoint management {#concept_qnj_hf3_ffb .concept}

An endpoint is the service entry of an Alibaba Cloud service. The endpoints of a service vary by regions. For example, the endpoint of an ECS instance in the China \(Hangzhou\) region is `ecs-cn-hangzhou.aliyuncs.com`, but the endpoint is `ecs.ap-northeast-1.aliyuncs.com` if the ECS instance is located in the Japan \(Tokyo\) region. Alibaba Cloud SDK has a built-in endpoint addressing module. After a request is sent, Alibaba Cloud SDK will find the endpoint to use according to the region ID and product ID that are specified when creating a client.

## Exception handling {#section_qws_5m3_ffb .section}

If an error occurs, such as `SDK.InvalidRegionID` or `SDK.EndpointResolvingError`, refer to the following solutions to troubleshoot.

|Error codes|Error message|Resolution|
|:----------|:------------|:---------|
|SDK.InvalidRegionId|can not find endpoint to access|The current SDK version is too low. Please upgrade the SDK core library `aliyun-python-sdk-core` to version 2.9.0 or higher.|
|SDK.EndpointResolvingError|No such region <region-id\>. Please check your region ID.|Make sure the region ID that you entered is correct.You can call the DescribeRegions API to find the region ID.

|
|SDK.EndpointResolvingError|No endpoint for product <product-id\>.|Solution:-   The current SDK version is too low. Please upgrade the SDK core library `aliyun-python-sdk-core` to version 2.9.0 or higher, and upgrade the product SDK such as aliyun-java-sdk-ecs to the latest version.

-   Configure the endpoint to use directly. For more information, see [Set up an endpoint]().


|
|SDK.EndpointResolvingError|No endpoint in the region <region-id\> for product <product-id\>.|Cannot find the endpoint of the product in the specified region. Solution:-   The product may be not available in the specified region. Please change the region ID accordingly.
-   Upgrade the SDK core library `aliyun-python-sdk-core` to the latest version to use the latest endpoint addressing configurations. A new version of the SDK core library will be released when the service endpoint is changed.
-   Configure the endpoint to use directly. For more information, see [Set up an endpoint]().


|

## Set up an endpoint directly {#section_vdj_l43_ffb .section}

Refer to the following code to set up an endpoint for a request to use. See the API documentation of each product to find available endpoints.

```
request = DescribeInstancesRequest()
# set up an endpoint for this request
configuration.setEndpoint("ecs-cn-hangzhou.aliyuncs.com");
# use the configured endpoint to sent a request
response = self.client.do_action_with_exception(request)
```

