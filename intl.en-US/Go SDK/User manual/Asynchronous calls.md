# Asynchronous calls {#concept_itr_k1m_zdb .concept}

Because of the concurrent characteristic of the Go language, we recommend that you control the concurrent requests of SDK on the application layer. To facilitate your usage, Alibaba Cloud Go SDK provides the concurrent calling method that can be directly used. The related concurrency control is achieved within SDK.

## Enable the concurrent feature for SDK Client {#section_s4x_n1m_zdb .section}

In the following example, the `Credential` object is used to set up user credentials.

```
// The maximum number of concurrent connections
poolSize := 10

// The maximum number of cacheable requests
maxTaskQueueSize := 10000

// Enable the asynchronous feature when creating a client
config := sdk.NewConfig().
    WithEnableAsync(true).
    WithGoRoutinePoolSize(poolSize).            // Optional, the default value is 5
    WithMaxTaskQueueSize(maxTaskQueueSize) // Optional, the default value is 1,000
	
// Use the custom configuration to initiate the client
ecsClient, err := ecs.NewClientWithOptions("<your-region-id>", config, credential)

// Or enable the asynchronous feature after initiating the client
client.EnableAsync(poolSize, maxTaskQueueSize)
```

## Initiate asynchronous calls {#section_szq_r1m_zdb .section}

You can make asynchronous calls using the following two methods:

-   Use channel as the return value

    ```
    // this will not block
    responseChannel, errChannel := client.FooWithChan(request)
    // this will block
    response := <-responseChannel
    err = <-errChanne
    ```

-   Use callbacks

    ```
    // this will not block
    blocker := client.FooWithCallback(request, func(response *FooResponse, err error) {
            // handle the response and err
          }
        )
    // The blocker is (chan int) and is used for controlling synchronization. The action succeeds when the return value is one, and fails when the return value is zero.
    // When <-blocker returns zero and the action fails, the error is transmitted to callback through err.
    // This will block
    result := <-blocker
    ```


