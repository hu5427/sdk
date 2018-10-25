# Virtual Private Cloud （VPC\) {#concept_swc_2lk_zdb .concept}

The example shows how to use Alibaba Cloud .NET SDK to call the CreateVpc API to create a VPC.

Virtual Private Cloud \(VPC\) is a private network established in Alibaba Cloud. VPCs are logically isolated from other virtual networks in Alibaba Cloud. You have full control over your VPC, such as specifying the IP address range of your VPC, and configuring route tables and network gateways. You can also use Alibaba Cloud resources such as ECS, RDS, and SLB in your own VPC.

## Code example {#section_lq5_3kk_zdb .section}

```
using System;
using Aliyun.Acs.Core;
using Aliyun.Acs.Core.Profile;
using Aliyun.Acs.Core.Exceptions;
using Aliyun.Acs.Vpc.Model.V20160428;

class Sample
{
    static void Main(string[] args)
    {
        // Create a client instance
        IClientProfile clientProfile = DefaultProfile.GetProfile("<your-region-id>", "<your-access-key-id>", "<your-access-key-secret>");
        DefaultAcsClient client = new DefaultAcsClient(clientProfile);
        try
        {
            // Create and initiate a request
            CreateVpcRequest request = new CreateVpcRequest();
            CreateUserResponse response = client.GetAcsResponse(request);
			
            // Create and initiate DescribeVpcAttributeRequest
            DescribeVpcAttributeRequest describeRequest = new DescribeVpcAttributeRequest();
            describeRequest.VpcId = createResponse.VpcId;
            DescribeVpcAttributeResponse describeResponse = client.GetAcsResponse(describeRequest);
            Console.WriteLine("vpcId: {0}", describeResponse.VpcId);
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

