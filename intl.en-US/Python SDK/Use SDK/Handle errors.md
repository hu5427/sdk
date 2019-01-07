# Handle errors {#concept_uzp_mjk_zdb .concept}

Alibaba Cloud Python SDK returns corresponding exceptions when an error occurs on the service side or the SDK side. These exceptions contain detailed error information, including the error code \(ErrCode\)  and error message \(ErrMsg\).

You do not need to handle these returned errors. You only need to make corresponding modifications based on the error message.

-   `ServerException` is the exception returned by the corresponding Alibaba Cloud service.
-   `ClientException` is the exception returned by the Alibaba Cloud Python SDK.

For example, when the following error occurs, you have to modify the ID of the AccessKey according to the error message.

```
aliyunsdkcore.acs\_exception.exceptions.ServerException: HTTP Status: 404 Error:InvalidAccessKeyId.NotFound Specified access key is not found.
```

If you want to handle the client errors returned by the Alibaba Cloud Python SDK, refer to the following example to write your codes:

```
try:
    response = client.do_action_with_exception(request)
except ServerException as e:
    # You can add your own error handling logic here
    # For example, print the error message
    print e.get_http_status()
    print e.get_error_code()
    print e.get_error_msg()
```

