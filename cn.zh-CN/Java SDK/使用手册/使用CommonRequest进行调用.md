# 使用CommonRequest进行调用 {#concept_ivk_p1k_zdb .concept}

当您要调用的某个产品的API没有提供SDK时，可以采用泛用型的API调用方式（CommonRequest）。使用CommonRequest调用方式可实现任意Open API接口的调用。

## CommonRequest调用特点 {#section_kwk_51k_zdb .section}

CommonRequest有如下特点：

1.  轻量：只需Core包即可发起调用，无需下载安装各产品的SDK。
2.  简便：无需更新SDK即可调用最新发布的API。
3.  快速迭代。

## 使用CommonRequest {#section_sc1_v1k_zdb .section}

阿里云产品的API有RPC和RESTful两种风格，不同风格的API的CommonRequest的调用方法也不同。

通常API参数中包含Action参数的是RPC风格，包含PathPattern参数的是RESTful风格。一般情况下，每个产品内，所有API的调用风格是统一的。每个API仅支持特定的一种风格调用，传入错误的标识，可能会调用到其他API，或收到`ApiNotFound`的错误信息。

发起一次CommonRequest请求，您需要获取以下几个参数的值。您可以在[文档中心](https://help.aliyun.com/)各产品的API文档中获取以下参数的值。此外，部分产品也可以通过[OpenAPI Explorer](https://api.aliyun.com/)来获取API的参数信息。

-   域名\(domain\)：该产品的服务地址。

-   API版本\(version\)：该API的版本号，格式为YYYY-MM-DD。

    您可以在各产品的API文档的公共参数部分获取API版本。

-   接口信息：要调用的接口名称。
    -   当调用的API为RPC风格时（大部分阿里云产品API为RPC风格）如ECS和RDS，需要获取Action参数，使用`request.ApiName = "<Action>"`的方式来指定API名称。

        例如RunInstances接口，在发起CommonRequest请求时，需要使用`request.ApiName = "RunInstances"`来指定API名称。

    -   当调用的API为RESTful风格时如容器服务, 需要获取PathPattern参数，使用`request.PathPattern = "<PathPattern>"`的方式来指定RESTful路径。

        例如容器服务的查看所有集群实例的API的PathPattern为`/clusters`，在发起CommonRequest请求时，要使用`request.PathPattern = "/clusters"`指定RESTful路径。


## 示例：调用RPC风格的API {#section_vsg_w1k_zdb .section}

以下代码展示了如何使用CommonRequest的方式调用ECS的DescribeInstanceStatus接口：

```
import com.aliyuncs.CommonRequest;
import com.aliyuncs.CommonResponse;
import com.aliyuncs.DefaultAcsClient;
import com.aliyuncs.IAcsClient;
import com.aliyuncs.exceptions.ClientException;
import com.aliyuncs.exceptions.ServerException;
import com.aliyuncs.profile.DefaultProfile;
public class Sample {
    public static void main(String[] args) {
        // 创建DefaultAcsClient实例并初始化
        DefaultProfile profile = DefaultProfile.getProfile(
            "<your-region-id>",          // 您的地域ID
            "<your-access-key-id>",      // 您的AccessKey ID
            "<your-access-key-secret>"); // 您的AccessKey Secret
        IAcsClient client = new DefaultAcsClient(profile);
        // 创建API请求并设置参数
        CommonRequest request = new CommonRequest();
        request.setDomain("ecs.aliyuncs.com");
        request.setVersion("2014-05-26");
        request.setAction("DescribeInstanceStatus");
        request.putQueryParameter("PageNumber", "1");
        request.putQueryParameter("PageSize", "30");
        try {
            CommonResponse response = client.getCommonResponse(request);
            System.out.println(response.getData());
        } catch (ServerException e) {
            // TODO Auto-generated catch block
            e.printStackTrace();
        } catch (ClientException e) {
            // TODO Auto-generated catch block
            e.printStackTrace();
        }
    }
}
```

## 示例：调用RESTful风格的API {#section_yxp_1bk_zdb .section}

以下代码展示了如何使用CommonRequest的方式调用容器服务的查看所有集群实例接口：

```
import com.aliyuncs.CommonRequest;
import com.aliyuncs.CommonResponse;
import com.aliyuncs.DefaultAcsClient;
import com.aliyuncs.IAcsClient;
import com.aliyuncs.exceptions.ClientException;
import com.aliyuncs.exceptions.ServerException;
import com.aliyuncs.profile.DefaultProfile;
public class Sample {
    public static void main(String[] args) {
        // 创建DefaultAcsClient实例并初始化
        DefaultProfile profile = DefaultProfile.getProfile(
            "<your-region-id>",          // 您的地域ID
            "<your-access-key-id>",      // 您的AccessKey ID
            "<your-access-key-secret>"); // 您的AccessKey Secret
        IAcsClient client = new DefaultAcsClient(profile);
        // 创建API请求并设置参数
        CommonRequest request = new CommonRequest();
        request.setDomain("cs.aliyuncs.com");
        request.setVersion("2015-12-15");
        request.setUriPattern("/clusters");
        try {
            CommonResponse response = client.getCommonResponse(request);
            System.out.println(response.getData());
        } catch (ServerException e) {
            // TODO Auto-generated catch block
            e.printStackTrace();
        } catch (ClientException e) {
            // TODO Auto-generated catch block
            e.printStackTrace();
        }
    }
}
```

