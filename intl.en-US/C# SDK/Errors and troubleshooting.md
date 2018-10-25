# Errors and troubleshooting {#concept_ul4_wlk_zdb .concept}

The following are some common errors you may experience when using Alibaba Cloud .NET SDK. Potential causes and resolutions are shown for these commonly experienced errors.

|Error code|Cause|Resolution|
|:---------|:----|:---------|
|`SDK.InvalidRegionId`|The SDK cannot automatically obtain the endpoint of the called product in the specified region.|Provide the correct endpoint of the service to call. You can find the endpoint from the API documentation of the product.|
|`InvalidAccessKeyId.NotFound`|The AccessKey ID is invalid.|Provide a correct AccessKey ID.|
|`SDK.ConnectionReset`|The endpoint cannot be connected when you initiate a request.|Make sure your network is available.|
|`SDK.InvalidProfile`|The DefaultAcsClient parameter that you provided when initializing DefaultAcsClient is invalid.|Make sure that the DefaultProfile parameter object that you provided is properly initialized and configured with corresponding parameters.|
|`SDK.SessionTokenExpired`|The DefaultAcsClient parameter that you provided when initializing DefaultAcsClient is invalid.|Make sure that the SessionToken parameter is in the validity period, which generally is one hour.|
|`SignatureDoesNotMatch`|The SessionToken you provided is invalid or has expired.|Make sure that the DefaultProfile parameter object that you provided is properly initialized and configured with corresponding parameters.|
|`SDK.InvalidProfile`|The AccessKey Secret does not match the AccessKey ID.|Make sure that the AccessKey Secret is valid.|

