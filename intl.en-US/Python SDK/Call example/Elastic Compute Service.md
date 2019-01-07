# Elastic Compute Service {#concept_zxw_xjk_zdb .concept}

The example shows how to create an ECS instance by calling the CreateInstance API of Alibaba Cloud Python SDK.

Elastic Compute Service \(ECS\) is a type of computing service that features elastic processing capabilities. ECS has a simpler and more efficient management mode than physical servers. You can create instances, change the operating system, and add or release any quantity of ECS instances at any time to fit your business needs.

To create an ECS instance, obtain the following information:

-   Image ID

    Obtain the ID of the image to use by calling the DescribeImages API.

-   Instance type

    Select the instance type to use. For more information, see [Instance type families](../../../../../reseller.en-US/Product Introduction/Instance type families.md#).


## Code example {#section_lq5_3kk_zdb .section}

**Note:** Running the code in this example will create an ECS instance and generate fees.

```
from aliyunsdkcore.client import AcsClient
from aliyunsdkcore.acs_exception.exceptions import ClientException
from aliyunsdkcore.acs_exception.exceptions import ServerException
from aliyunsdkecs.request.v20140526 import CreateInstanceRequest
# Create an AcsClient instance
client = AcsClient(
   "<your-access-key-id>",
   "<your-access-key-secret>",
   "<your-region-id>"

# Create a request and set parameters
request = CreateInstanceRequest.CreateInstanceRequest()
request.set_ImageId("alinux_17_01_64_20G_cloudinit_20171222.vhd")
request.set_InstanceName("MyInstance")
request.set_SecurityGroupId("<your-security-group-id>")
request.set_InstanceType("ecs.t1.small")
request.set_ClientToken("<uuid>")
# Initiate an API request and print the response
response = client.do_action_with_exception(request)
print response
```

