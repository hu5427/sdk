# C++ SDK {#concept_mkk_vpj_zdb .concept}

This document introduces how to install and call Alibaba Cloud C++ SDK.

## Installation {#section_kjb_1qj_zdb .section}

Before installing the C ++ SDK, make sure that the following requirements are met:

-   Install a compiler supporting C++ 11 or later, including Visual Studio 2015 and later or GCC 4.9 and later.

-   Install CMake 3.0 or later.

-   The computer has a memory of 4 GB or larger.


Complete these steps to install C++ SDK:

1.  Run the following command to clone the source codes of C++ SDK.

    ```
    git clone https://github.com/aliyun/aliyun-openapi-cpp-sdk.git
    ```

2.  Install the SDK.
    -   Install C++ SDK on the Windows platform

        Go to the sdk\_build directory of the source code, and use Visual Studio to open the alibabacloud-sdk.sln file to generate the solution. Or run the following command at the command prompt of Visual Studio  to install the C++ SDK:

        ```
        
        msbuild ALL_BUILD.vcxproj
        msbuild INSTALL.vcxpro
        ```

    -   Install C++ SDK on the Linux platform

        1.  Install dependency libraries, including libcurl, libopenssl, libuuid, and libjsoncpp.
            -   Run the following command in the Debian/Ubuntu system to install dependency libraries.

                ```
                sudo dnf install libcurl-devel openssl-devel libuuid-devel libjsoncpp-devel
                ```

            -   Run the following command in the Redhat/Fedora system to install dependency libraries.

                ```
                sudo apt-get install libcurl4-openssl-dev libssl-dev uuid-dev libjsoncpp-dev
                ```

        2.  Run the following command to compile and install  the C++ SDK.

            ```
            makesudo 
            make install
            ```


## Initialize C++ SDK {#section_oxp_djm_zdb .section}

Before using the Alibaba Cloud C++ SDK service client to call a service, you must use `AlibabaCloud::InitializeSdk` to initialize the SDK first and use `AlibabaCloud::ShutdownSdk` to release resources at last.

```
#include <alibabacloud/core/AlibabaCloud.h>
int main(int argc, char** argv)

    AlibabaCloud::InitializeSdk();
    
    AlibabaCloud::ShutdownSdk();
    return 0;

```

## Set up credentials {#section_vjb_1qj_zdb .section}

When using the Alibaba Cloud C++ SDK to access Alibaba Cloud services, you need an Alibaba Cloud account for authentication.

C++ SDK supports the following authentication methods.

|Authentication|Description|
|:-------------|:----------|
|AccessKey|Use the AccessKeyID/Secret to do the authentication|
|StsToken|Use the STS Token to do the authentication|
|RamRoleArn|Use the AssumeRole of the RAM account to do the authentication|
|EcsRamRole|Use the RAM role of an ECS instance to do the authentication|

This document uses AccessKey as an example to illustrate how to set up credentials. To ensure the security of your account, we recommend using your RAM account instead of the primary account. The primary account has full access to all of your cloud services, while the RAM account has limited access granted by the primary account to the cloud services. Firstly, create an AccessKey as described in ,

and then set up your credentials when initializing the client as follows:

**Note:** Do not disclose any code containing your AccessKey \(do not commit the code to public GitHub projects\). Otherwise, your Alibaba Cloud account may be compromised.

```
// Create a client instance
    ClientConfiguration configuration("<your-region-id>");
    EcsClient client("<your-access-key-id>", "<your-access-key-secret>", configuration);
```

## Configure the service client {#section_bkb_1qj_zdb .section}

The Alibaba Cloud C++ SDK provides corresponding Alibaba Cloud APIs for the client to use.

The client service complies with the following naming convention: `AlibabaCloud::Service::ServiceClient`. For example, use `AlibabaCloud::Ecs::EcsClient` to build an ECS client.

You can use `ClientConfiguration` to control some running functions of the service client, as the following code example shows.

```
// Configure an ecs instance
ClientConfiguration configuration;
configuration.setRegionId("cn-hangzhou");
configuration.setEndpoint("ecs-cn-hangzhou.aliyuncs.com");
EcsClient client("<your-access-key-id>", "<your-access-key-secret>", configuration);
```

where:

-   regionId  is the ID of the region where the service is located.
-   regionId is the service endpoint. \(Not recommended\)
-   proxy is the network proxy.

## Use CMake to build a program {#section_g3j_yjm_zdb .section}

CMake is an automatic cross-platform and open-source compiling system. It uses a file named `CMakeLists.txt` to describe the dependency relation between the compiling process and the application, and finally generates the project file that suits your platform most.

Follow these steps to use CMake to build a program:

1.  Create a CMake project:
    1.  Run the following command to create a project directory.

        ```
        mkdir my_example
        ```

    2.  Create a `CMakeLists.txt` file under the created project directory.

        This file is used for describing your project name, executable files, source files and link libraries. For more information, see [Official site of CMake](https://cmake.org/).

        ```
        # Set the lowest Cmake version required by the project
        cmake_minimum_required(VERSION 3.0)
        # Set the project name
        project(my-example)
        # Set C++ 11 as the default version
        set(CMAKE_CXX_STANDARD 11)
        # Set the SDK library installation catalogue
        link_directories(/usr/local/lib64)
        # Set the application name and the original file
        add_executable(my-example
             main.cc)
        # Dynamic link SDK library file
        add_definitions(-DALIBABACLOUD_SHARED)
        # Specify the SDK library file to link
        target_link_libraries(my-example
                     alibabacloud-sdk-core
                     alibabacloud-sdk-ecs)
        ```

2.  Use CMake to compile a program:
    1.  Run the following command to create a compiling directory under the project folder.

        ```
        mkdir build
        ```

    2.  Go to the created compiling directory and run the `camke` command.

        ```
        cmake -DBUILD_SHARED_LIBS=ON -DCMAKE_INSTALL_PREFIX=/usr/local ..
        ```

        The following are compiling options of the Alibaba Cloud C++ SDK:

        -   BUILD\_SHARED\_LIBS: The file output type of the Alibaba Cloud SDK library. valid value: ON: Dynamic library. This is the default value. OFF: Static library.

**Note:** Before dynamically linking an Alibaba Cloud SDK, you must define the preprocessor symbol `ALIBABACLOUD_SHARED`.

        -   BUILD\_TESTS: Whether to compile the unit test module. Valid value: ON: Compile the unit test module. This is the default value. OFF: Do not compile the unit test module

        -   TARGET\_OUTPUT\_NAME\_PREFIX: The prefix of the output file name. The default value is alibabacloud-sdk-.

    3.  After Makefile is generated, run the make command to compile the application.

