# Handle errors {#concept_k1s_tzl_zdb .concept}

Alibaba Cloud Go SDK returns corresponding exceptions when an error occurs on the service side or the SDK side. These exceptions contain detailed error information, including `ClientError` and `ServerError` under `sdk.errors`. These errors conform to the Go error API, so you can handle errors returned by Alibaba Cloud Go SDK in the same way as handling Go errors.

## ClientError {#section_tmh_xzl_zdb .section}

When an error occurs to any call in SDK and cannot be handled automatically, SDK returns a `ClientError`.

You can add the following codes to determine and obtain the error in `ClientError`:

```
response, err := ecsClient.DescribeInstances(request)
if clientError, ok := err.(*errors.ClientError); ok{
    // Obtain the error code
    clientError.ErrorCode()
    // Obtain the error description
    clientError.Message()
    // Obtain the original error (may be nil)
    clientError.OriginError()

```

## ServerError {#section_ebz_c1m_zdb .section}

When the server side returns an error message, SDK wraps the response into a `ServerError` and returns it.

**Note:** In this case, you can still obtain the original HTTP response from the response.

You can determine and get the error information in servererror and response using the following code:

```
response, err := ecsClient.DescribeInstances(request)    
if serverError, ok := err.(*errors.ServerError); ok{
    // Obtain the error code
    serverError.ErrorCode()
    // Obtain the error description
    serverError.Message()
    // Obtain the original http response
    response.GetOriginHttpResponse()

```

