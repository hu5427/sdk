# Errors and troubleshooting {#concept_ul4_wlk_zdb .concept}

The following are some common errors you may experience when using Alibaba Cloud SDK. Potential causes and resolutions are shown for these commonly experienced errors. Submit your problem to Alibaba Cloud through DingTalk group: 11771185, or [GitHub Issue](https://github.com/aliyun/aliyun-openapi-java-sdk/issues).

|Error code|Error message|Cause|Resolution|
|:---------|:------------|:----|:---------|
|`SDK.InvalidRegionId`|`Can not find endpoint to access.`|The SDK cannot automatically obtain the endpoint of the called product in the specified region.|Checks whether the provided region ID and endpoint are correct. Run the following codes to set the endpoint:```
from aliyunsdkcore.profile import region_provider
region_provider.modify_point('Ecs', 'cn-zhangjiakou', 'ecs.aliyuncs.com')
```

|
|`SDK.TimeoutError`|—|The request times out and all retry attempts fail.| -   In scenarios such as cross-region calls or low network quality, we recommend that you increase the timeout or the maximum number of retries.

-   If the problem persists and the network quality is confirmed to be good, we recommend that you open a ticket.


 |
|`SDK.ServerError：InvalidProtocol.NeedSsl`|`Your request is denied as lack of ssl protect.Recommend:https://error-center.aliyun.com/status/search?Keyword=InvalidProtocol.NeedSsl&source=PopGw`|The API only accepts HTTPS requests and does not accept HTTP requests.|Before sending a request, add the following code:```
Request. set_protocol_type ('https ')
```

|

