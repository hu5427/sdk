# 使用Java SDK {#concept_mkk_vpj_zdb .concept}

本文档介绍如何安装及调用阿里云Java SDK。

## 安装 {#section_kjb_1qj_zdb .section}

阿里云Java SDK支持1.6及以上版本的JDK，您可以通过以下两种安装方式Java SDK：

-   使用Maven\(推荐\)

    如果您使用了Maven管理依赖，您可以通过向`pom.xml`中添加以下代码来安装阿里云Java SDK。

    ```
    <dependency>
        <groupId>com.aliyun</groupId>
        <artifactId>aliyun-java-sdk-core</artifactId>
        <version>3.5.0</version>
    </dependency>
    <dependency>
        <groupId>com.aliyun</groupId>
        <artifactId>aliyun-java-sdk-ecs</artifactId>
        <version>3.0.0</version>
    </dependency>
    ```

-   在集成开发环境（IDE）中导入JAR文件

**说明：** 该安装方式会在下个主版本中被废弃，届时将仅支持通过Maven安装SDK。

    无论您使用Eclipse还是IntelliJ作为集成开发环境，都可以通过导入JAR文件的方式安装阿里云Java SDK。您可以在[阿里云开发工具包（SDK）](https://develop.aliyun.com/tools/sdk?spm=5176.doc52740.2.4.wQVCcn#/java)中下载各云产品的JAR文件。

    -   使用Eclipse安装

        完成以下操作，在Eclipse的项目中安装阿里云Java SDK：

        1.  将下载的`aliyun-java-sdk-xxx.jar`文件复制到您的项目文件夹中。
        2.  在Eclipse中打开您的项目，右键单击该项目，单击**Properties**。
        3.  在弹出的对话框中，单击**Java Build Path** \> **Libraries** \> **Add JARs**添加下载的JAR文件。
        4.  单击**Apply and Close**。
    -   使用IntelliJ安装

        完成以下操作，在IntelliJ的项目中安装阿里云Java SDK：

        1.  将下载的`aliyun-java-sdk-xxx.jar`文件复制到您的项目文件夹中。
        2.  在IntelliJ中打开您的项目，在菜单栏中单击**File** \> **Project Structure**。
        3.  单击**Apply**，然后单击**OK**。

## 设置身份验证凭据 {#section_vjb_1qj_zdb .section}

当使用阿里云SDK访问阿里云服务时，您需要提供阿里云账号进行身份验证。

目前，Java SDK支持以下几种身份验证方式：

|验证方式|说明|
|:---|:-|
|AccessKey|使用AccessKey ID和AccessKey Secret访问|
|StsToken|使用STS Token访问|
|RamRoleArn|使用RAM子账号的AssumeRole方式访问|
|EcsRamRole|在ECS实例上通过EcsRamRole实现免密验证|
|RsaKeyPair|使用RSA公私钥方式（仅日本站支持）|

本文以AccessKey为例说明如何设置身份凭证。为了保证您的账号安全，建议您使用RAM账号来访问阿里云服务。阿里云账号的AccessKey对拥有的资源有完全的权限。RAM账号由阿里云账号授权创建，仅有对特定资源限定的操作权限。参考[创建AccessKey](https://help.aliyun.com/document_detail/66453.html)创建RAM账号的AccessKey。

使用AccessKey作为访问凭据，需要在初始化Client时设置凭证。

**说明：** 确保包含AccessKey的代码不会泄漏（例如提交到外部公开的GitHub项目），否则将会危害您的阿里云账号的信息安全。

```
DefaultProfile profile = DefaultProfile.getProfile(
    "<your-region-id>",          // 地域ID
    "<your-access-key-id>",      // RAM账号的AccessKey ID
    "<your-access-key-secret>"); // RAM账号AccessKey Secret
```

## 发起调用 {#section_bkb_1qj_zdb .section}

本文以ECS为例，介绍如何使用阿里云Java SDK发起请求。

1.  新建一个AcsClient。

    ```
    IAcsClient client = new DefaultAcsClient(profile);
    ```

2.  创建请求。

    请求类的命名规范为`${apiName}Request`，其中$\{apiName\}为API名称，例如DescribeInstances。

    在引入多个产品SDK时，有可能存在Request类同名的情况，请注意按照package区分。

    ```
    DescribeInstancesRequest request = new DescribeInstancesRequest();
    request.setPageSize(10);
    ```

3.  发起调用并处理应答。

    ```
    DescribeInstancesResponse response;
    try {
        response = client.getAcsResponse(request);
        for (DescribeInstancesResponse.Instance instance:response.getInstances()) {
            System.out.println(instance.getPublicIpAddress());
        }
    } catch (ServerException e) {
        e.printStackTrace();
    } catch (ClientException e) {
        e.printStackTrace();
    }
    ```

    正常情况下，应答中的所有字段，都会被反序列化到response中，您可以直接调用`response.getXXX()`来获得应答中的字段。

    ```
    instanceStatus := response.getStatus()
    ```

    如果出现了异常，或您需要原始HTTP应答的情况下，您可以通过以使用`doAction()`来获取原始应答。

    ```
    HttpResponse response = client.doAction(request);
    ```


