# Server Load Balancer \(SLB\) {#concept_swc_2lk_zdb .concept}

This tutorial uses the CreateLoadBalancer API of SLB to show you how to use Alibaba Cloud Python SDK to call SLB APIs.

Alibaba Cloud Server Load Balancer \(SLB\) is a traffic distribution control service that distributes the incoming traffic among multiple Elastic Compute Service \(ECS\) instances according to the configured forwarding rules. It expands the service capabilities of the application and increases the availability of the application.

## Code example {#section_lq5_3kk_zdb .section}

**Note:** Running the code in this example will create an SLB instance and generate fees.

```
from aliyunsdkcore.client import AcsClient
from aliyunsdkcore.acs_exception.exceptions import ClientException
from aliyunsdkcore.acs_exception.exceptions import ServerException
from aliyunsdkslb.request.v20140515 import CreateLoadBalancerRequest
# Create an AcsClient instance
client = AcsClient(
   "<your-access-key-id>",
   "<your-access-key-secret>",
   "<your-region-id>"

# Create a Server Load Balancer instance
request = CreateLoadBalancerRequest.CreateLoadBalancerRequest()
request.set_LoadBalancerName('MyLoadBalancer')
request.set_AddressType('internet')
request.set_InternetChargeType('paybytraffic')
response = client.do_action_with_exception(request)
print response
```

