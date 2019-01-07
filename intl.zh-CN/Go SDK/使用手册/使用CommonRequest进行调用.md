# 使用CommonRequest进行调用 {#concept_ivk_p1k_zdb .concept}

当您要调用的某个产品的API没有提供SDK时，可以采用泛用型的API调用方式（CommonRequest）。使用CommonRequest调用方式可实现任意Open API接口的调用。

## CommonRequest调用特点 {#section_kwk_51k_zdb .section}

CommonRequest的特点如下：

1.  轻量：只需Core包即可发起调用，无需下载安装各产品的SDK。
2.  简便：无需更新SDK即可调用最新发布的API。
3.  快速迭代。

## 使用CommonRequest {#section_sc1_v1k_zdb .section}

阿里云产品的API有RPC和RESTful两种风格，不同风格的API的CommonRequest的调用方法也不同。

通常API参数中包含Action参数的是RPC风格，包含PathPattern参数的是RESTful风格。一般情况下，每个产品内，所有API的调用风格是统一的。每个API仅支持特定的一种风格调用，传入错误的标识，可能会调用到其他API，或收到`ApiNotFound`的错误信息。

发起一次CommonRequest请求，您需要获取以下几个参数的值。您可以在[文档中心](https://www.alibabacloud.com/help)各产品的API文档中获取以下参数的值。此外，部分产品也可以通过[OpenAPI Explorer](https://api.aliyun.com/)来获取API的参数信息。

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
package main

import (
    "github.com/aliyun/alibaba-cloud-sdk-go/sdk/requests"
    "github.com/aliyun/alibaba-cloud-sdk-go/sdk"
    "fmt"
)

func main() {
    client, err := sdk.NewClientWithAccessKey("cn-hangzhou", "{your_access_key_id}", "{your_access_key_id}")
    if err != nil {
        panic(err)
    }
	
    request := requests.NewCommonRequest()
    request.Domain = "ecs.aliyuncs.com"
    request.Version = "2014-05-26"
    // 因为是RPC接口，因此需指定ApiName(Action)
    request.ApiName = "DescribeInstanceStatus"
    request.QueryParams["PageNumber"] = "1"
    request.QueryParams["PageSize"] = "30"
    response, err := client.ProcessCommonRequest(request)
    if err != nil {
        panic(err)
    }
    fmt.Print(response.GetHttpContentString())
}
```

## 示例：调用RESTful风格的API {#section_yxp_1bk_zdb .section}

以下代码展示了如何使用CommonRequest的方式调用容器服务的查看所有集群实例接口：

```
package main

import (
    "github.com/aliyun/alibaba-cloud-sdk-go/sdk/requests"
    "github.com/aliyun/alibaba-cloud-sdk-go/sdk"
    "fmt"
)

func main() {
    client, err := sdk.NewClientWithAccessKey("cn-hangzhou", "{your_access_key_id}", "{your_access_key_id}")
    if err != nil {
        panic(err)
    }
	
    request := requests.NewCommonRequest()
    request.Domain = "cs.aliyuncs.com"
    request.Version = "2015-12-15"
    // 因为是Restful接口，因此需指定PathPattern
    request.PathPattern = "/clusters"
    response, err := client.ProcessCommonRequest(request)
    if err != nil {
        panic(err)
    }
    fmt.Print(response.GetHttpContentString())
}
```

