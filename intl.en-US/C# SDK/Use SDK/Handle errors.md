# Handle errors {#concept_uzp_mjk_zdb .concept}

Alibaba Cloud .NET SDK returns corresponding exceptions when an error occurs on the service side or the SDK side. These exceptions contain detailed error information, including the error code \(Error Code\) and error message \(Error Message\).

You do not need to handle the exceptions returned by the Alibaba Cloud .NET SDK. You only need to resolve the errors returned by the service.

-   `ServerException` is the exception returned by the corresponding Alibaba Cloud service.
-   `ClientException` is the exception returned by the Alibaba Cloud .NET SDK.

For example, when the following error occurs, you have to modify the ID of the AccessKey according to the error message.

```
InvalidAccessKeyId.NotFound : Specified access key is not found.
```

If you want to handle the client errors returned by the Alibaba Cloud .NET SDK, refer to the following example to write your codes:

```
try 
{
    CreateInstanceResponse response = client.GetAcsResponse(request);
    // Handle the response
    // ...
}
catch (ServerException e)
{
    // You can add your own error handling logic here
    // For example, print the error message
    Console.WriteLine("ErrorCode=" + e.ErrorCode);
    Console.WriteLine("ErrorMessage=" + e.ErrorMessage);
    // If the problem is tricky, you can open a ticket and provide the RequestId to us
    Console.WriteLine("ErrorCode=" + e.RequestId);
}
catch (ClientException e)
{
    // You can add your own error handling logic here
    // For example, print the error message
    Console.Writeline("error code :" + e.errorCode;
    Console.Writeline("error message :" + e.Message;
}
```

