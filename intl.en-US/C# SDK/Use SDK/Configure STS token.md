# Configure STS token {#concept_ixf_gbk_zdb .concept}

Using your Alibaba Cloud AccessKey directly to develop applications will have potential security risk. To enhance your account security, you can use the Security Token Service \(STS\) token issued for a subaccount to access Alibaba Cloud services.

## Introduction to STS token {#section_npt_pbk_zdb .section}

Security Token Service is a cloud service that provides short-term access control for Alibaba Cloud accounts \(or RAM users\). Through STS, you can issue an access credentials with custom time limit and access right to federated users \(users managed by your local account system\). Federation users can use STS short-term access credentials to call the Alibaba Cloud APIs directly, or log on to the Alibaba Cloud console to manage the authorized resources.

Using the STS token as an access credential has the following advantages:

-   Using STS token will reduce the risks of a compromised AccessKey ID and AccessKey Secret, particularly reducing risks for your mobile devices.

-   STS token has flexible permission control. You can control the access permission in a finer granularity for products including SLB and ECS, according to the RAM role.


**Note:** Make sure that the product you are calling supports STS.

## Configure the STS token {#section_dh2_pck_zdb .section}

To configure the STS Token, the following two methods are available:

-   Method 1: Use STS token directly

    You can directly specify the STS token. However, you must update the token periodically if you directly specify the token.

    ```
    using System;
    using Aliyun.Acs.Core;
    using Aliyun.Acs.Core.Auth;
    using Aliyun.Acs.Core.Profile;
    using Aliyun.Acs.Core.Exceptions;
    using Aliyun.Acs.Ecs.Model.V20140526;
    
    class SimpleSTSTokenSample
    {
        static void Main(string[] args)
        {
            BasicSessionCredentials credentials = new BasicSessionCredentials(
                "<your-access-key-id>",
                "<your-access-key-secret>",
                "<your-session-token>"
    			
            // Create a client instance
            IClientProfile profile = DefaultProfile.GetProfile("<your-region-id>");
            DefaultAcsClient client = new DefaultAcsClient(profile, credentials);
            try
            {
                // Create an API request and set parameters
                DescribeInstancesRequest request = new DescribeInstancesRequest();
                request.PageSize = 10;
    			
                // Initiate the request and print the handling result
                DescribeInstancesResponse response = client.GetAcsResponse(request);
                Console.WriteLine("TotalCount: {0}", response.TotalCount);
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

    Where:

    -   region-id is the ID of the region that you are using. You can obtain the region ID by calling the DescribeRegions API.
    -   sts-access-key-id, sts-access-key-secret, and sts-session-token are credentials returned by calling the AssumeRole API.
-   Method 2: Use SDK to manage STS tokens

    You can specify the RAM role to allow Alibaba Cloud .NET SDK to create and maintain STS tokens.

    ```
    using System;
    using Aliyun.Acs.Core;
    using Aliyun.Acs.Core.Auth;
    using Aliyun.Acs.Core.Profile;
    using Aliyun.Acs.Core.Exceptions;
    using Aliyun.Acs.Ecs.Model.V20140526;
    
    class UseRoleArnSample
    {
        static void Main(string[] args)
        {
            IClientProfile profile = DefaultProfile.GetProfile("<your-region-id>");
            BasicCredentials basicCredentials = new BasicCredentials(
                "<your-access-key-id>",
                "<your-access-key-secret>")
            STSAssumeRoleSessionCredentialsProvider provider = new STSAssumeRoleSessionCredentialsProvider(
                basicCredentials,
                "<your-role-arn>",
                profile);
    			
            // Create a client instance
            DefaultAcsClient client = new DefaultAcsClient(profile, provider);
    		
            try
            {
                // Create an API request and set parameters
                DescribeInstancesRequest request = new DescribeInstancesRequest();
                request.PageSize = 10;
    			
                // Initiate the request and print the handling result
                DescribeInstancesResponse response = client.GetAcsResponse(request);
                Console.WriteLine("TotalCount: {0}", response.TotalCount);
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

    Where:

    -   role-arn is the role resource descriptor. You can obtain it on the role details page from the [RAM console](https://ram.console.aliyun.com/role/list?spm=a2c4g.11186623.2.7.IjY04Z#/role/list).

    -   role-session-name is a temporary role name. You can call the AssumeRole API to create a temporary identity. After the temporary identity is created, you can use the value set for the RoleSessionName parameter as the role-session-name parameter in this method.


