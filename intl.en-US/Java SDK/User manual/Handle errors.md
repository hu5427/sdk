# Handle errors {#concept_uzp_mjk_zdb .concept}

Alibaba Cloud Java SDK returns corresponding exceptions when an error occurs on the service side or the SDK side. These exceptions contain detailed error information,  including the error code \(ErrCode\) and error message \(ErrMsg\).

You do not need to handle the exceptions returned by the Alibaba Cloud Java SDK.  You only need to resolve the errors returned by the service.

-   `ServerException` is the exception returned by the corresponding Alibaba Cloud service.
-   `ClientException` is the exception returned by the Alibaba Cloud Java SDK.

For example, when the following error occurs, you have to modify the ID of the AccessKey according to the error message.

```
com.aliyuncs.exceptions.ClientException: InvalidAccessKeyId.NotFound : Specified access key is not found.
```

If you want to handle the client errors returned by the Alibaba Cloud Java SDK, refer to the following example to write your codes:

```
try {
    FooResponse response = client.getAcsResponse(request);
    // Handle the response
    
}catch (ServerException e){
    // You can add your own error handling logic here
    // For example, print the error message
    System.out.println("ErrorCode=" + e.getErrCode());
    System.out.println("ErrorMessage=" + e.getErrMsg());
    // If the problem is tricky, you can open a ticket and provide the RequestId to us
    System.out.println("ResponseId=" + e.getRequestId());
}catch (ClientException e){
    // You can add your own error handling logic here
    // For example, print the error message
    System.out.println("ErrorCode=" + e.getErrCode());
    System.out.println("ErrorMessage=" + e.getErrMsg());

```

