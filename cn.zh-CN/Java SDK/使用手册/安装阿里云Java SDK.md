# 安装阿里云Java SDK {#concept_mkk_vpj_zdb .concept}

阿里云Java SDK支持1.6及以上版本的JDK。您可以通过直接修改Maven项目文件`pom.xml`安装阿里云Java SDK。

## 使用Maven \(推荐\) {#section_hvs_sls_gfb .section}

如果您使用Maven管理Java项目，可以通过在`pom.xml`文件中添加Maven依赖安装阿里云SDK。您可以在Maven库中查找各云产品的Maven依赖信息。

执行以下代码安装阿里云SDK核心库和ECS Java SDK.

```
<dependency>
    <groupId>com.aliyun</groupId>
    <artifactId>aliyun-java-sdk-core</artifactId>
    <version>3.7.0</version>
</dependency>
<dependency>
    <groupId>com.aliyun</groupId>
    <artifactId>aliyun-java-sdk-ecs</artifactId>
    <version>4.11.0</version>
</dependency>
```

## 在集成开发环境（IDE）中导入JAR文件 {#section_kjb_1qj_zdb .section}

无论您使用Eclipse还是IntelliJ作为集成开发环境，都可以通过导入JAR文件的方式安装阿里云Java SDK。您可以在[阿里云开发工具包（SDK）](https://develop.aliyun.com/tools/sdk?spm=5176.doc52740.2.4.wQVCcn#/java)中下载各云产品的JAR文件。

**说明：** 该安装方式将来会被废弃，届时将仅支持通过Maven安装SDK。

-   使用Eclipse安装

    完成以下操作，在Eclipse的项目中安装阿里云Java SDK：

    1.  将下载的aliyun-java-sdk-xxx.jar文件复制到您的项目文件夹中。
    2.  在Eclipse中打开您的项目，右键单击该项目，单击**Properties**。
    3.  在弹出的对话框中，单击**Java Build Path** \> **Libraries** \> **Add JARs**添加下载的JAR文件。
    4.  单击**Apply and Close**。
-   使用IntelliJ安装

    完成以下操作，在IntelliJ的项目中安装阿里云Java SDK：

    1.  将下载的aliyun-java-sdk-xxx.jar文件复制到您的项目文件夹中。
    2.  在IntelliJ中打开您的项目，在菜单栏中单击**File** \> **Project Structure**。
    3.  单击**Apply**，然后单击**OK**。

