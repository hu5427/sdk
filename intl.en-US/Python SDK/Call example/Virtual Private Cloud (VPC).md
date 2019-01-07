# Virtual Private Cloud \(VPC\) {#concept_swc_2lk_zdb .concept}

This tutorial uses the CreateVpc API of VPC to show you how to use Alibaba Cloud Python SDK to call VPC APIs.

Virtual Private Cloud \(VPC\) is a private network established in Alibaba Cloud. VPCs are logically isolated from other virtual networks in Alibaba Cloud. You have full control over your VPC, such as specifying the IP address range of your VPC, and configuring route tables and network gateways. You can also use Alibaba Cloud resources such as ECS, RDS, and SLB in your own VPC.

## Code example {#section_lq5_3kk_zdb .section}

```
import json
from aliyunsdkcore.client import AcsClient
from aliyunsdkcore.acs_exception.exceptions import ClientException
from aliyunsdkcore.acs_exception.exceptions import ServerException
from aliyunsdkvpc.request.v20160428 import CreateVpcRequest
from aliyunsdkvpc.request.v20160428 import DescribeVpcAttributeRequest
# Create an AcsClient instance
client = AcsClient(
   "<your-access-key-id>",
   "<your-access-key-secret>",
   "<your-region-id>"

# Create a VPC
request = CreateVpcRequest.CreateVpcRequest()
response = client.do_action_with_exception(request)
vpc_id = json.loads(response)['VpcId']
print "VPC ID is", vpc_id
# Obtain and print the attributes of the VPC
request = DescribeVpcAttributeRequest.DescribeVpcAttributeRequest()
request.set_VpcId(vpc_id)
response = client.do_action_with_exception(request)
print response
```

