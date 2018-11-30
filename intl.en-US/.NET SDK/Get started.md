# Get started {#concept_b43_j4j_zdb .concept}

Welcome to use the Alibaba Cloud Software Development Kit \(SDK\). The Alibaba Cloud .NET SDK allows you to access Alibaba Cloud services such as Elastic Compute Service \(ECS\), Relational Database Service \(RDS\), and CloudMonitor. You can access Alibaba Cloud services without the need to handle API related tasks, such as signing and constructing your requests. This document introduces how to install and use .NET SDK.

## Prerequisites {#section_xpr_q4j_zdb .section}

-   To use Alibaba Cloud .NET SDK, you must have an Alibaba Cloud account and an AccessKey. You can create and view your AccessKey on the [AccessKey console](https://usercenter.console.aliyun.com/?spm=5176.doc52740.2.3.QKZk8w#/manage/ak), or contact your system administrator.
-   To use Alibaba Cloud .NET SDK to call APIs of a product, you must first activate the product on the Alibaba Cloud console if required.
-   Alibaba Cloud .Net SDK applies to .NET Framework 4.0 and later.

## Install Alibaba Cloud .NET SDK {#section_hts_q4j_zdb .section}

To call the SDK of a product, you must install the SDK core library and the SDK of the product. For example, to call ECS SDK, you must obtain and install both the SDK core library and ECS SDK. Install the Alibaba Cloud .NET SDK using one of the following methods:

-   Add DLL reference
    1.  Download the DLL package from [.NET SDK](https://develop.aliyun.com/tools/sdk#/dotnet).
    2.  Right click your project in the Solution Explorer of Visual Studio and click **Reference**.
    3.  In the displayed menu, click **Add Reference**.
    4.  In the displayed dialog box, click **Browse**. Then select the downloaded DLL file and click **Confirm**.
-   Add project reference
    1.  Run the following command to clone the SDK source codes from GitHub.

        ```
        git clone https://github.com/aliyun/aliyun-openapi-net-sdk.git
        ```

        There are many folders prefixed with aliyun-net-openapi-XXX the cloned directory. Each folder contains \\\\\*.csproj file, which is the .NET project file. For example, there is an `aliyun-net-sdk-ecs.csproj` file under the `aliyun-net-openapi-ecs` subfolder.

    2.  In Visual Studio, right click your solution.
    3.  Click**Add** \> **Existing Project**.
    4.  In the displayed dialog box, select the project file, for example, `aliyun-net-sdk-ecs.csproj`, and then click **Open**.
    5.  Right click your project and click**Reference** \> **Add Reference**.

## Use .NET SDK {#section_m4y_npj_zdb .section}

The following code example shows the three main steps to use the Alibaba Cloud .NET SDK:

1.  Create a DefaultAcsClient instance and initialize it.
2.  Create an API request and set parameters.
3.  Initiate the request and handle the response.

    ```
    using Aliyun.Acs.Core;
    using Aliyun.Acs.Core.Profile;
    using Aliyun.Acs.Core.Exceptions;
    using Aliyun.Acs.Ecs.Model.V20140526;
    
    class TestProgram
    {
        static void Main(string[] args)
        {
            // Construct a client for initiating requests
            IClientProfile profile = DefaultProfile.GetProfile(
                "<your-region-id>",
                "<your-access-key-id>",
                "<your-access-key-secret>")
            DefaultAcsClient client = new DefaultAcsClient(profile);
            try
            {
                // Construct a request
                DescribeInstancesRequest request = new DescribeInstancesRequest();
                request.PageSize = 10;
                // Initiate the request and obtain the response
                DescribeInstancesResponse response = client.GetAcsResponse(request);
                System.Console.WriteLine(response.TotalCount);
            }
            catch (ServerException ex)
            {
                System.Console.WriteLine(ex.ToString());
            }
            catch (ClientException ex)
            {
                System.Console.WriteLine(ex.ToString());
            }
        }
    }
    ```


