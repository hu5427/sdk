# 使用Go SDK {#concept_mkk_vpj_zdb .concept}

本文档介绍如何安装及调用阿里云Go SDK。

## 安装 {#section_kjb_1qj_zdb .section}

阿里云Go SDK支持Go 1.7及以上版本，您可以通过以下方式安装Go SDK：

-   使用Glide \(推荐\)

    ```
    glide get github.com/aliyun/alibaba-cloud-sdk-go
    ```

-   使用govendor

    执行以下命令，安装Go SDK：

    ```
    go get -u github.com/aliyun/alibaba-cloud-sdk-go/sdk
    ```


## 设置身份验证凭据 {#section_vjb_1qj_zdb .section}

当使用阿里云SDK访问阿里云服务时，您需要提供阿里云账号进行身份验证。

目前，Go SDK支持以下几种身份验证方式：

|验证方式|说明|
|:---|:-|
|AccessKey|使用AccessKey ID和AccessKey Secret访问|
|StsToken|使用STS Token访问|
|RamRoleArn|使用RAM子账号的AssumeRole方式访问|
|EcsRamRole|在ECS实例上通过EcsRamRole实现免密验证|
|RsaKeyPair|使用RSA公私钥方式（仅日本站支持）|

本文以AccessKey为例说明如何设置身份凭证。为了保证您的账号安全，建议您使用RAM账号来访问阿里云服务。阿里云账号的AccessKey对拥有的资源有完全的权限。RAM账号由阿里云账号授权创建，仅有对特定资源限定的操作权限。参考[创建AccessKey](https://www.alibabacloud.com/help/doc-detail/66453.htm)创建RAM账号的AccessKey。

使用AccessKey作为访问凭据，需要在初始化Client时设置凭证。

**说明：** 确保包含AccessKey的代码不会泄漏（例如提交到外部公开的GitHub项目），否则将会危害您的阿里云账号的信息安全。

```
// 创建ecsClient实例
ecsClient, err := ecs.NewClientWithAccessKey(
        "<your-region-id>",                 // 您的可用区ID
        "<your-access-key-id>",             // 您的Access Key ID
        "<your-access-key-secret>")        // 您的Access Key Secret
```

如果要使用自定义的Config，或使用AccessKey以外的凭据，您需要先创建一个`Credential`对象来保存凭据，并在初始化Client时提交此`Credential`对象。

```
// 自定义config
config := NewConfig()
// 创建credential对象
credential := &credentials.BaseCredential{
    AccessKeyId:     "<your-access-key-id>",
    AccessKeySecret: "<your-access-key-secret>",
}
// 初始化client
client, err := NewClientWithOptions("cn-hangzhou", config, credential)
```

## 发起调用 {#section_bkb_1qj_zdb .section}

本文以ECS为例，介绍如何使用阿里云Go SDK发起请求。

1.  导入云产品SDK。

    ```
    import github.com/aliyun/alibaba-cloud-sdk-go/services/ecs
    ```

2.  新建一个ecsClient，该client中包含ECS的所有API。

    ```
    ecsClient := ecs.NewClientWithAccessKey(
        "<your-region-id>",                 
        "<your-access-key-id>",                
    	"<your-access-key-secret>")
    ```

3.  使用`xxx.CreateXXXRequest`方法创建一个请求。

    该方法的命名规范为`${service}.Create${apiName}Request`，其中：

    -   $\{service\}为产品名称\(小写\)，例如 ecs、oss、slb等
    -   $\{apiName\}为API名称，例如DescribeInstances
    ```
    
    request := ecs.CreateDescribeInstanceAttributeRequest()
    request.InstanceId = ""
    ```

4.  发起调用。

    ```
    response, err := ecsClient.DescribeInstanceAttribute(request)
    ```

5.  处理应答。

    正常情况下，应答中的所有字段，都会被反序列化到response中，您可以直接调用`response.getXXX()`来获得应答中的字段。

    ```
    instanceStatus := response.Status
    ```

    如果出现了异常`(err != nil)`，或您需要原始HTTP应答，您可以通过以下几种方式获得原始数据：

    ```
    // 获取原始http应答，返回类型为 *http.Response
    httpResponse := response.GetOriginHttpResponse()
    
    // 获取原始http应答的statusCode，返回类型为 int
    statusCode := response.GetHttpStatus()
    
    // 获取原始http应答的header，返回类型为 map[string][]string
    headers := response.GetHttpHeaders()
    
    // 获取原始http应答的content，返回类型为[]byte
    contentBytes := response.GetHttpContentBytes()
    
    // 获取原始http应答的content, 并按照UTF-8转码为string，返回类型为string
    contentString := response.GetHttpContentString()
    ```


