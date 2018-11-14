# Handle errors {#concept_atc_flm_zdb .concept}

The Ali cloud C ++ SDK does not support exceptions, however, you can still use exceptions in your code for error handling, each service client class interface returns a result object that contains both the result and the error object.

```
auto outcome = client.describeInstances(request);
if (! outcome.isSuccess())

    Error e = outcome.error();
    std::cout << "host: " << e.host() << std::endl;
    std::cout << "requestId: " << e.requestId() << std::endl;
    std::cout << "errorCode: " << e.errorCode() << std::endl;
    std::cout << "errorMessage: " << e.errorMessage() << std::endl;

```

