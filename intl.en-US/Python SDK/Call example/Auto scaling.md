# Auto scaling {#concept_zxw_xjk_zdb .concept}

This tutorial introduces how to use the CreateScalingGroup API of Alibaba Cloud Python SDK to create an auto scaling group and how to specify the I/O optimized instance through the IoOptimized parameter.

Auto scaling is a management service that allows users to automatically adjust elastic computing resources according to their individual needs and policies.

## Code example: Create an auto scaling group {#section_lq5_3kk_zdb .section}

**Note:** Make sure that you have activated the auto scaling service.

```
import json

from aliyunsdkcore.client import AcsClient
from aliyunsdkcore.acs_exception.exceptions import ClientException
from aliyunsdkcore.acs_exception.exceptions import ServerException
from aliyunsdkess.request.v20140828 import CreateScalingGroupRequest
from aliyunsdkess.request.v20140828 import CreateScalingConfigurationRequest

# Create an AcsClient instance
client = AcsClient(
   "<your-access-key-id>",
   "<your-access-key-secret>",
   "<your-region-id>"


# Create a scaling group
request = CreateScalingGroupRequest.CreateScalingGroupRequest()
request.set_MaxSize(10)
request.set_MinSize(2)
response = client.do_action_with_exception(request)
scaling_group_id = json.loads(response)['ScalingGroupId']
print "ScalingGroupId is", scaling_group_id

# Create scaling configuration
request = CreateScalingConfigurationRequest.CreateScalingConfigurationRequest()
request.set_ScalingGroupId(scaling_group_id)
# You can obtain the ImageId through the DescribeImages API of ECS
request.set_ImageId('centos_7_04_64_20G_alibase_201701015.vhd')
request.set_InstanceType('ecs.t1.xsmall')

# You can obtain the SecurityGroupId through the DescribeSecurityGroups API of ECS
request.set_SecurityGroupId('sg-bp14z29vpgy2t7spfxw8')
response = client.do_action_with_exception(request)
print response
```

## Code example: specify the I/O optimized instance {#section_ptm_srl_zdb .section}

```
# Create scaling configuration
request = CreateScalingConfigurationRequest.CreateScalingConfigurationRequest()
request.set_ScalingGroupId(scaling_group_id)

# You can obtain the ImageId through the DescribeImages API of ECS
request.set_ImageId('centos_7_04_64_20G_alibase_201701015.vhd')
request.set_InstanceType('ecs.t1.xsmall')
request.set_IoOptimized('optimized')

# You can obtain the SecurityGroupId through the DescribeSecurityGroups API of ECS
request.set_SecurityGroupId('sg-bp14z29vpgy2t7spfxw8')
response = client.do_action_with_exception(request)
print response
```

