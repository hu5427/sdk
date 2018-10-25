# Content Delivery Network \(CDN\) {#concept_sfl_3rm_zdb .concept}

This example introduces how to use Alibaba Cloud .NET SDK to call the AddCdnDomainRequest API to add a CDN domain name.

Alibaba Cloud Content Delivery Network \(CDN\), is a distributed network that is built on and overlays the bearer network, and is composed of edge node server clusters distributed across different regions.

Note the following before calling the API:

-   Before creating a CDN domain name, you must have activated the CDN service.

-   The CDN domain name must have been filed.

-   If the origin site content is not on the Alibaba Cloud platform, it must be reviewed. The review will be completed before the next workday.


```
using System;
using Aliyun.Acs.Core;
using Aliyun.Acs.Core.Profile;
using Aliyun.Acs.Core.Exceptions;
using Aliyun.Acs.Cdn.Model.V20141111;

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
            AddCdnDomainRequest request = new AddCdnDomainRequest();
            request.CdnType = "web";
            request.DomainName = "test.com";
            request.Sources = "test.com";
            request.SourceType = "domain";
			
            //Initiate the request
            AddCdnDomainResponse response = client.GetAcsResponse(request);
            Console.WriteLine("Success");
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

