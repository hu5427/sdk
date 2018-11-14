# 使用C++ SDK {#concept_mkk_vpj_zdb .concept}

本文档介绍如何安装及调用阿里云C++ SDK。

## 安装 {#section_kjb_1qj_zdb .section}

在安装C++ SDK前，确保你的环境满足以下条件：

-   已安装支持C++ 11或更高版本的编译器，包括（Visual Studio 2015及以上版本或 GCC 4.9及以上版本）。

-   已安装CMake 3.0或以上版本。

-   计算机拥有4 GB或更大的内存。


完成以下操作，安装C++ SDK：

1.  您可以通过访问[GitHub](https://github.com/aliyun/aliyun-openapi-cpp-sdk/archive/master.zip)下载阿里云C++ SDK的源代码，也可以执行以下命令，使用git获取C++ SDK的源代码。

    ```
    git clone https://github.com/aliyun/aliyun-openapi-cpp-sdk.git
    ```

2.  安装SDK。
    -   Windows平台

        进入源代码的sdk\_build目录，使用Visual Studio打开alibabacloud-sdk.sln文件，生成解决方案。您也可以在Visual Studio的开发人员命令提示符中执行以下命令，安装C++ SDK。

        ```
        
        msbuild ALL_BUILD.vcxproj
        msbuild INSTALL.vcxpro
        ```

    -   Linux平台

        1.  在Linux平台中安装第三方库，包括libcurl、libopenssl、libuuid和libjsoncpp。
            -   在基于Redhat / Fedora的系统上执行以下命令安装第三方库。

                ```
                sudo dnf install libcurl-devel openssl-devel libuuid-devel libjsoncpp-devel
                ```

            -   在基于Debian / Ubuntu的系统上执行以下命令安装第三方库。

                ```
                sudo apt-get install libcurl4-openssl-dev libssl-dev uuid-dev libjsoncpp-dev
                ```

        2.  执行以下命令，编译并安装C++ SDK。

            ```
            makesudo 
            make install
            ```


## 初始化SDK {#section_oxp_djm_zdb .section}

在使用阿里云 C++ SDK 服务客户端发起请求前，您必须先使用`AlibabaCloud::InitializeSdk`接口进行初始化，最后还需使用 `AlibabaCloud::ShutdownSdk`接口释放资源。

```
#include <alibabacloud/core/AlibabaCloud.h>
int main(int argc, char** argv)
{
    AlibabaCloud::InitializeSdk();
    // ......
    AlibabaCloud::ShutdownSdk();
    return 0;
}
```

## 设置身份验证凭据 {#section_vjb_1qj_zdb .section}

当使用阿里云SDK访问阿里云服务时，您需要提供阿里云账号进行身份验证。

目前，C++ SDK支持以下几种身份验证方式：

|验证方式|说明|
|:---|:-|
|AccessKey|使用AccessKey ID和AccessKey Secret访问|
|StsToken|使用STS Token访问|
|RamRoleArn|使用RAM子账号的AssumeRole方式访问|
|EcsRamRole|在ECS实例上通过EcsRamRole实现免密验证|

本文以AccessKey为例说明如何设置身份凭证。为了保证您的账号安全，建议您使用RAM账号来访问阿里云服务。阿里云账号的AccessKey对拥有的资源有完全的权限。RAM账号由阿里云账号授权创建，仅有对特定资源限定的操作权限。参考[创建AccessKey](https://www.alibabacloud.com/help/doc-detail/66453.htm)创建RAM账号的AccessKey。

使用AccessKey作为访问凭据，需要在初始化Client时设置凭证。

**说明：** 确保包含AccessKey的代码不会泄漏（例如提交到外部公开的GitHub项目），否则将会危害您的阿里云账号的信息安全。

```
// 创建客户端实例
    ClientConfiguration configuration("<your-region-id>");
    EcsClient client("<your-access-key-id>", "<your-access-key-secret>", configuration);
```

## 配置服务客户端 {#section_bkb_1qj_zdb .section}

阿里云C++ SDK服务客户端类为您提供了该类所代表的阿里云的服务接口。

服务客户端通常遵循的名称约定为`AlibabaCloud::Service::ServiceClient`。例如，使用`AlibabaCloud::Ecs::EcsClient`类构建云服务器ECS的客户端。

您可以使用客户端配置类`ClientConfiguration`来控制部分服务客户端运行时功能。

```
// 配置 ecs 实例
ClientConfiguration configuration;
configuration.setRegionId("cn-hangzhou");
configuration.setEndpoint("ecs-cn-hangzhou.aliyuncs.com");
EcsClient client("<your-access-key-id>", "<your-access-key-secret>", configuration);
```

其中：

-   regionId是地域ID。
-   endpoint是服务节点。\(不建议使用\)
-   proxy是网络代理设置。

## 使用CMake构建程序 {#section_g3j_yjm_zdb .section}

CMake是一个开源的跨平台自动化建构系统，它使用一个名为`CMakeLists.txt`的文件来描述构建过程和应用程序依赖关系，并最终生成适合您当前构建平台的项目文件。

完成以下操作，构建一个程序：

1.  创建CMake项目
    1.  执行以下命令，创建项目文件目录。

        ```
        mkdir my_example
        ```

    2.  进入该目录并创建`CMakeLists.txt`文件。

        该文件用于描述您的项目名称，可执行文件，源文件和链接库。关于更多CMake选项功的功能介绍，请参见[CMake官方网站](https://cmake.org/)。

        ```
        # 为项目设置cmake所需的最低版本
        cmake_minimum_required(VERSION 3.0)
        # 为整个项目设置名称
        project(my-example)
        # 设置默认为 C++ 11
        set(CMAKE_CXX_STANDARD 11)
        # 设置SDK库安装目录
        link_directories(/usr/local/lib64)
        # 设置应用程序名及原文件
        add_executable(my-example
             main.cc)
        # 动态链接 SDK 库文件
        add_definitions(-DALIBABACLOUD_SHARED)
        # 指定要链接的 SDK 库文件
        target_link_libraries(my-example
                     alibabacloud-sdk-core
                     alibabacloud-sdk-ecs)
        ```

2.  使用CMake进行编译
    1.  执行以下命令，在项目文件夹下创建编译目录。

        ```
        mkdir build
        ```

    2.  进入该目录并执行`camke`命令。

        ```
        cmake -DBUILD_SHARED_LIBS=ON -DCMAKE_INSTALL_PREFIX=/usr/local ..
        ```

        其中，阿里云C++ SDK部分的编译选项如下:

        -   BUILD\_SHARED\_LIBS：指定阿里云SDK库文件输出类型，取值：ON（默认值）|OFF，如果为ON将输出为共享库，否则为静态库。

**说明：** 要动态链接阿里云SDK前，您需要定义预处理器符号`ALIBABACLOUD_SHARED`。

        -   BUILD\_TESTS：指定是否编译单元测试模块，取值：ON（默认值）|OFF。如果为ON将编译单元测试模块，否则不编译。

        -   TARGET\_OUTPUT\_NAME\_PREFIX：指定输出文件名前缀。默认值是alibabacloud-sdk-。

    3.  在生成Makefile文件后，执行make命令编译应用程序。

