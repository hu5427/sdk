# Auto scaling {#concept_zxw_xjk_zdb .concept}

The example shows how to use Alibaba Cloud .NET SDK to call the CreateScalingGroup API to create an auto scaling group and how to specify the I/O optimized instance through the IoOptimized parameter.

Auto scaling is a management service that allows you to automatically adjust elastic computing resources according to your individual needs and policies.

## Code example: Create an auto scaling group {#section_lq5_3kk_zdb .section}

**Note:** Make sure that you have activated the auto scaling service.

```
using System;
using Aliyun.Acs.Core;
using Aliyun.Acs.Core.Profile;
using Aliyun.Acs.Core.Exceptions;
using Aliyun.Acs.Ess.Model.V20140828;

class Sample
{
    static void Main(string[] args)
    {
        // Create a client instance
        IClientProfile clientProfile = DefaultProfile.GetProfile("<your-region-id>", "<your-access-key-id>", "<your-access-key-secret>");
        DefaultAcsClient client = new DefaultAcsClient(clientProfile);
		
        try
        {
            // Create a scaling group
            CreateScalingGroupRequest csgRequest = new CreateScalingGroupRequest();
            csgRequest.MaxSize = 10;
            csgRequest.MinSize = 2;
            CreateScalingGroupResponse csgResponse = client.GetAcsResponse(csgRequest);
            String scalingGroupId = csgResponse.ScalingGroupId;
            Console.WriteLine("ScalingGroupId: {0}", scalingGroupId);
			
            // Create scaling configuration
            CreateScalingConfigurationRequest cscRequest = new CreateScalingConfigurationRequest();
            cscRequest.ScalingGroupId = scalingGroupId;
            cscRequest.ImageId = "centos_7_04_64_20G_alibase_201701015.vhd";
            cscRequest.InstanceType = "ecs.t1.xsmall";
            cscRequest.SecurityGroupId = "G0000000123456789";// You can obtain the SecurityGroupId through the DescribeSecurityGroups API of ECS
            CreateScalingConfigurationResponse cscResponse = client.GetAcsResponse(cscRequest);
            Console.WriteLine("ScalingConfigurationId: {0}", cscResponse.ScalingConfigurationId);
        }
        catch (ServerException e)
        {
            Console.WriteLine(e.ErrorCode);
            Console.WriteLine(e.ErrorMessage);
        }
        catch (ClientException e)
        {
            Console.WriteLine(e.ErrorCode);
            Console.WriteLine(e.ErrorMessage);
        }
    }
}
```

## Code example: Specify the I/O optimized instance {#section_ptm_srl_zdb .section}

```

CreateScalingConfigurationRequest cscRequest = new CreateScalingConfigurationRequest();
cscRequest.ScalingGroupId = scalingGroupId;
cscRequest.ImageId = "centos_7_04_64_20G_alibase_201701015.vhd";
cscRequest.InstanceType = "ecs.t1.xsmall";
cscRequest.IoOptimized = "optimized";
cscRequest.SecurityGroupId = "G0000000123456789";
```

