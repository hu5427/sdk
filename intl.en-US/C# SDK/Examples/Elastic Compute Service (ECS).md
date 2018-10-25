# Elastic Compute Service \(ECS\) {#concept_zxw_xjk_zdb .concept}

The example introduces how to use Alibaba Cloud .NET SDK to call the CreateInstance API to create an ECS instance.

Elastic Compute Service \(ECS\) is a type of computing service that features elastic processing capabilities. ECS has a simpler and more efficient management mode than physical servers. You can create instances, change the operating system, and add or release any quantity of ECS instances at any time to fit your business needs.

To create an ECS instance, obtain the following information first:

-   Image ID

    Obtain the ID of the image to use by calling the DescribeImages API.

-   Instance Type

    Select the instance type to use. For more information, see [Instance type families](../../../../reseller.en-US/Product Introduction/Instance type families.md#).


## Code example {#section_lq5_3kk_zdb .section}

**Note:** Running the code in this example will create an ECS instance and generate fees.

```
using System;
using Aliyun.Acs.Core;
using Aliyun.Acs.Core.Profile;
using Aliyun.Acs.Core.Exceptions;
using Aliyun.Acs.Ecs.Model.V20140526;

class Sample
{
    static void Main(string[] args)
    {
        // Create a client instance
        IClientProfile clientProfile = DefaultProfile.GetProfile("<your-region-id>", "<your-access-key-id>", "<your-access-key-secret>");
        DefaultAcsClient client = new DefaultAcsClient(clientProfile);
        try
        {
            // Create an API request and set parameters
            CreateInstanceRequest request = new CreateInstanceRequest();
            request.ImageId = "_32_23c472_20120822172155_aliguest.vhd";
            request.InstanceType = "ecs.t1.small";
			
            // Initiate the request and print the handling result
            CreateInstanceResponse response = client.GetAcsResponse(request);
            Console.WriteLine("InstanceId: {0}", response.InstanceId);
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

