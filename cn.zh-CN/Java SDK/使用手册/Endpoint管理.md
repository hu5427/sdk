# Endpoint管理 {#concept_qnj_hf3_ffb .concept}

Endpoint是阿里云服务的API服务端地址。针对不同的地域，单个服务可能有不同的Endpoint。例如，云服务器（ECS）在华东1（杭州）地域的Endpoint是 `ecs-cn-hangzhou.aliyuncs.com`， 而在日本（东京）地域的Endpoint是`ecs.ap-northeast-1.aliyuncs.com`。阿里云SDK内置了 Endpoint寻址模块，当您调用SDK对一个服务发起请求时，SDK会自动根据您在创建SDK Client时指定的地域ID（Region ID）和产品ID来找到Endpoint。

## 异常处理 {#section_qws_5m3_ffb .section}

如果遇到了`SDK.InvalidRegionID` 或者`SDK.EndpointResolvingError`的错误，建议您参考以下信息解决。

|错误码|错误消息|解决方法|
|:--|:---|:---|
|SDK.InvalidRegionID|can not find endpoint to access|当前SDK版本过低。请将SDK核心库`aliyun-java-sdk-core`升级到4.1.0版本或更高版本。|
|SDK.EndpointResolvingError|No such region <region-id\>. Please check your region ID.|请检查您输入的地域ID是否正确。可以通过DescribeRegionsAPI查找地域ID。

|
|SDK.EndpointResolvingError|No endpoint for product <product-id\>.|解决方法：-   当前SDK版本过低。请将SDK核心库`aliyun-java-sdk-core`升级到4.1.0版本或更高版本，并将使用的产品SDK如aliyun-java-sdk-ecs升级到最新版本。

-   直接设置Endpoint来发送请求，详情参见[直接设置Endpoint]()。


|
|SDK.EndpointResolvingError|No endpoint in the region <region-id\> for product <product-id\>.|在指定地域下找不到该产品的Endpoint，解决方法如下：-   可能该产品在指定地域尚未开放服务，请根据提示信息更换地域ID。
-   将`aliyun-java-sdk-core`升级到最新版本，以便应用最新的内置Endpoint解析配置。阿里云在服务地址发生新增、变更时会发布新的SDK Core版本。
-   直接设置Endpoint来发送请求，详情参见[直接设置Endpoint]()。


|

## 直接设置Endpoint {#section_vdj_l43_ffb .section}

参考以下代码直接为服务请求设置Endpoint。参考各云产品的API文档，查看可用的Endpoint。

**说明：** 请根据实际情况替换代码示例中的Endpoint \(ecs-cn-hangzhou.aliyuncs.com\)。

```
DescribeInstancesRequest request = new DescribeInstancesRequest();
// 对这个请求直接设置Endpoint
request.setEndpoint("ecs-cn-hangzhou.aliyuncs.com");
DescribeInstancesResponse response = this.client.getAcsResponse(request);
```

