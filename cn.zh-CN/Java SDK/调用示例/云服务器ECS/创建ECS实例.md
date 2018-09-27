# 创建ECS实例 {#concept_zxw_xjk_zdb .concept}

本示例介绍如何使用阿里云Java SDK调用ECS的CreateInstance接口创建一个ECS实例。

在创建ECS实例前，您需要获取以下信息：

-   镜像 ID

    调用DescribeImages接口查看要使用的镜像 ID。

-   实例规格

    查看[实例规格族](../../../../cn.zh-CN/产品简介/实例规格族.md#)选择要创建的ECS实例的规格。


## 示例代码 {#section_lq5_3kk_zdb .section}

**说明：** 运行该示例代码将创建ECS实例，并产生实际费用。

```

import com.aliyuncs.DefaultAcsClient;
import com.aliyuncs.IAcsClient;
import com.aliyuncs.ecs.model.v20140526.CreateInstanceRequest;
import com.aliyuncs.ecs.model.v20140526.CreateInstanceResponse;
import com.aliyuncs.exceptions.ClientException;
import com.aliyuncs.exceptions.ServerException;
import com.aliyuncs.profile.DefaultProfile;
public class Demo {
    public static void main(String[] args) {
        // 创建DefaultAcsClient实例并初始化
        DefaultProfile profile = DefaultProfile.getProfile(
            "<your-region-id>",                     // 您的可用区ID
            "<your-access-key-id>",             // 您的Access Key ID
            "<your-access-key-secret>");        // 您的Access Key Secret
        IAcsClient client = new DefaultAcsClient(profile);
        // 创建API请求并设置参数
        CreateInstanceRequest request = new CreateInstanceRequest();
        request.setImageId("alinux_17_01_64_20G_cloudinit_20171222.vhd");
        request.setInstanceName("MyEcsInstance");
        request.setSecurityGroupId("<your-security-group-id>");
        request.setInstanceType("ecs.t1.small");
        request.setClientToken(UUID.randomUUID().toString());
        // 发起请求并处理应答或异常
        CreateInstanceResponse response;
        try {
            response = client.getAcsResponse(request);
            String instanceId = response.getInstanceId();
            System.out.println("Create instance success, instanceId = " + instanceId);
        } catch (ServerException e) {
            e.printStackTrace();
        } catch (ClientException e) {
            e.printStackTrace();
        }
    }
}
```

