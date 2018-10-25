# Server Load Balancer \(SLB\) {#concept_swc_2lk_zdb .concept}

The example shows how to use Alibaba Cloud .NET SDK to call the CreateLoadBalancer API to create an SLB instance.

Server Load Balancer \(SLB\) is a traffic distribution control service that distributes the incoming traffic among multiple Elastic Compute Service \(ECS\) instances according to the configured forwarding rules. It expands the service capabilities of the application and increases the availability of the application.

## Code example {#section_lq5_3kk_zdb .section}

**Note:** Running the code in this example will create an SLB instance and generate fees.

```
using System;
using Aliyun.Acs.Core;
using Aliyun.Acs.Core.Profile;
using Aliyun.Acs.Core.Exceptions;
using Aliyun.Acs.Slb.Model.V20140515;

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
            CreateLoadBalancerRequest request = new CreateLoadBalancerRequest();
            request.LoadBalancerName = "my-sample-slb";
            request.AddressType = "internet";
            request.InternetChargeType = "paybytraffic
			
            // Initiate the request and print the handling result
            CreateLoadBalancerResponse response = client.GetAcsResponse(request);
            Console.WriteLine("LoadBalancerId: {0}", response.LoadBalancerId);
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

