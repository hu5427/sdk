# 云服务器ECS {#concept_zxw_xjk_zdb .concept}

本示例介绍如何使用阿里云.NET SDK调用ECS的CreateInstance接口创建一个ECS实例。

云服务器（Elastic Compute Service，简称ECS）是阿里云提供的一种基础云计算服务。使用云服务器ECS就像使用水、电、煤气等资源一样便捷、高效。您无需提前采购硬件设备，而是根据业务需要，随时创建所需数量的云服务器实例。如果不再需要云服务器，也可以方便的释放资源，节省费用。

在创建ECS实例前，您需要获取以下信息：

-   镜像 ID

    调用DescribeImages接口查看要使用的镜像 ID。

-   实例规格

    查看[实例规格族](../../../../intl.zh-CN/产品简介/实例规格族.md#)选择要创建的ECS实例的规格。


## 示例代码 {#section_lq5_3kk_zdb .section}

**说明：** 运行该示例代码将创建ECS实例，并产生实际费用。

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
        // 创建客户端实例
        IClientProfile clientProfile = DefaultProfile.GetProfile("<your-region-id>", "<your-access-key-id>", "<your-access-key-secret>");
        DefaultAcsClient client = new DefaultAcsClient(clientProfile);
        try
        {
            // 创建API请求并设置参数
            CreateInstanceRequest request = new CreateInstanceRequest();
            request.ImageId = "_32_23c472_20120822172155_aliguest.vhd";
            request.InstanceType = "ecs.t1.small";
			
            // 请求并打印处理结果
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

