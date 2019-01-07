# Elastic Compute Service {#concept_zxw_xjk_zdb .concept}

The example shows how to create an ECS instance by calling the CreateInstanceRequest API.

Elastic Compute Service \(ECS\) is a type of computing service that features elastic processing capabilities. ECS has a simpler and more efficient management mode than physical servers. You can create instances, change the operating system, and add or release any quantity of ECS instances at any time to fit your business needs.

To create an ECS instance, obtain the following information:

-   Image ID

    Obtain the ID of the image to use by calling the DescribeImages API.

-   Instance type

    Select the instance type to use. For more information, see [Instance type families](../../../../../reseller.en-US/Product Introduction/Instance type families.md#).


## Code example {#section_lq5_3kk_zdb .section}

**Note:** Running the code in this example will create an ECS instance and generate fees.

```
package main

import (
    "github.com/aliyun/alibaba-cloud-sdk-go/services/ecs"
    "github.com/aliyun/alibaba-cloud-sdk-go/sdk/utils"
    "fmt"
)

func main() { 
    // Create a client instance
    client, err := ecs.NewClientWithAccessKey(
        "<your-region-id>", // your region ID
        "<your-access-key-id>", // your AccessKey ID
        "<your-access-key-secret>"// your AccessKey ID
    if err ! = nil {
        // Handle exceptions
        panic(err)
    }
    // Create a request and set parameters
    request := ecs.CreateCreateInstanceRequest()
    request.ImageId = "alinux_17_01_64_20G_cloudinit_20171222.vhd"
    request.InstanceName = "MyInstance"
    request.SecurityGroupId = "<your-security-group-id>"
    request.InstanceType = "ecs.t1.small"
    request.ClientToken = utils.GetUUIDV4() 
    response, err := client.CreateInstance(request)
    if err ! = nil {
        // exception
        panic(err)
    }
    fmt.Printf("success(%d)! instanceId = %s\n", response.GetHttpStatus(), response.InstanceId)
}
```

