# Errors and troubleshooting {#concept_ul4_wlk_zdb .concept}

The following are some common errors you may experience when using Alibaba Cloud SDK. Potential causes and resolutions are shown for these commonly experienced errors. Submit your problem to Alibaba Cloud through DingTalk group: 11771185, or [GitHub Issue](https://github.com/aliyun/aliyun-openapi-java-sdk/issues).

|Error Code|Error information|Cause:|Resolution:|
|:---------|:----------------|:-----|:----------|
|`SDK.CanNotResolveEndpoint`|`Can not resolve endpoint, please check the user guide`|The SDK cannot automatically obtain the endpoint of the called product in the specified region.|Checks whether the provided region ID and endpoint are correct. Run the following codes to set the endpoint:```
DefaultProfile.addEndpoint("cn-hangzhou", "cn-hangzhou", "Ecs", "ecs.aliyuncs.com");
```

|
|`SDK.JsonUnmarshalError`|`Failed to unmarshal response`|SDK response deserialization fails. In most cases, it is because the response structure received by the SDK does not conform to the API metadata. For example, the fields do not match or the format is incorrect.|You can use the `client.doAction(request)` method to obtain the original HTTP response.|
|`SDK.TimeoutError`|`*The request timed out 4 times\(3 for retry\), perhaps we should have the threshold raised a little?*`|The request times out and all retry attempts fail.| -   In scenarios such as cross-region calls or low network quality, we recommend that you increase the timeout or the maximum number of retries.

-   If the problem persists and the network quality is confirmed to be good, we recommend that you open a ticket.


 |
|`SDK.ServerError：InvalidProtocol.NeedSsl`|`Your request is denied as lack of ssl protect.Recommend:https://error-center.aliyun.com/status/search?Keyword=InvalidProtocol.NeedSsl&source=PopGw`|The API only accepts HTTPS request and does not accept HTTP request.|Add the following codes before sending a request:```
request.setProtocol(ProtocolType.HTTPS)
```

|

