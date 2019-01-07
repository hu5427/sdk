# Errors and troubleshooting {#concept_ul4_wlk_zdb .concept}

The following are some common errors you may experience when using Alibaba Cloud Java SDK. Potential causes and resolutions are shown for these commonly experienced errors. Submit your problem to Alibaba Cloud through DingTalk group: 11771185, or [GitHub Issue](https://github.com/aliyun/aliyun-openapi-java-sdk/issues).

|Error code|Error information|Cause:|Resolution:|
|:---------|:----------------|:-----|:----------|
|`SDK.CanNotResolveEndpoint`|`Can not resolve endpoint, please check the user guide`|The SDK cannot automatically obtain the endpoint of the called product in the specified region.|Checks whether the provided region ID and endpoint are correct. Run the following codes to set the endpoint:```
request.Domain = "<endpoint>"
```

|
|`SDK.JsonUnmarshalError`|`Failed to unmarshal response, but you can get the data via response.GetHttpStatusCode() and response.GetHttpContentString()`|SDK response deserialization fails. In most cases, it is because the response structure received by the SDK does not conform to the API metadata. For example, the fields do not match or the format is incorrect.|[Resolution](#section_e2s_n2m_zdb)|
|`SDK.TimeoutError`|`*The request timed out 4 times\(3 for retry\), perhaps we should have the threshold raised a little?*`|The request times out and all retry attempts fail.| -   In scenarios such as cross-region calls or low network quality, we recommend that you increase the timeout or the maximum number of retries.

-   If the problem persists and the network quality is confirmed to be good, we recommend that you open a ticket.


 |
|`SDK.AsyncFunctionNotEnabled`|`Async function is not enabled in client, please invoke ‘client.EnableAsync()’ function`|An asynchronous API is called when the asynchronous request switch is not enabled.|Enable the asynchronous feature. For more information, see [Asynchronous calls](reseller.en-US/Go SDK/User manual/Asynchronous calls.md#).|

## Resolution {#section_e2s_n2m_zdb .section}

Ignore SDK deserialization failure and obtain the response attributes by extracting the original HTTP response.

**Note:** Alibaba Go SDK has dependency on JMESPath. Therefore, when the response content is in the JSON format, you can directly use JMESPath to parse the response.

```
response, err := client.CreateInstance(request)
    if err ! = nil {
        // Handle exceptions
        if clientErr, isClientErr := err.(*errors.ClientError); isClientErr {
            if clientErr.ErrorCode() == errors.JsonUnmarshalErrorCode {
                // You can respectively obtain the data in the original response through our encapsulate methods.
                // The http status code in the response, type: int
                statusCode := response.GetHttpStatus()
                // The http head in the response, type: map[string][]string
                responseHeader := response.GetHttpHeaders()
                // The body content in the response, type: []byte
                responseBytes := response.GetHttpContentBytes()
                // The body content in the response, type: string, equivalent to string(response.GetHttpContentBytes())`
                responseContent := response.GetHttpContentString()
                // You can also directly obtain the original response, type: net/http.Response
                httpResponse := response.GetOriginHttpResponse()
            }
        
    
```

