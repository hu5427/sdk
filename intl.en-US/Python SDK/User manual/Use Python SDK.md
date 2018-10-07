# Use Python SDK {#concept_mkk_vpj_zdb .concept}

This document introduces how to install and call Alibaba Python SDK.

## Installation {#section_kjb_1qj_zdb .section}

Alibaba Cloud Python SDK supports Python 2.6.x, 2.7.x, 3.x and later. You can install Python SDK using the following two methods:

-   Use pip \(recommended\)

    Run the following commands to install Python SDK using pip.

    ```
    
    pip install aliyun-python-sdk-core # Install Alibaba Cloud SDK core library
    pip install aliyun-python-sdk-ecs # Install ECS SDK
    pip install aliyun-python-sdk-rds # Install RDS SDK
    ```

    **Note:**  If you use python3.x, modify`pip install aliyun-python-sdk-core` to `pip install aliyun-python-sdk-core-v3`.

-   Use GitHub

    Run the following commands to clone the Python SDK from GitHub and install the SDK:

    ```
    git clone https://github.com/aliyun/aliyun-openapi-python-sdk.git
    # Install Alibaba Cloud SDK core library
    cd aliyun-python-sdk-core
    python setup.py install
    # Install Alibaba Cloud ECS SDK
    cd aliyun-python-sdk-ecs
    python setup.py install
    ```


## Set up credentials {#section_vjb_1qj_zdb .section}

When using the Alibaba Cloud Python SDK to access Alibaba Cloud services, you need an Alibaba Cloud account for authentication.

Python SDK supports the following authentication methods.

|Authentication|Description|
|:-------------|:----------|
|AccessKey|Use the AccessKeyID/Secret to do the authentication|
|StsToken|Use the STS Token to do the authentication|
|RamRoleArn|Use the AssumeRole of the RAM account to do the authentication|
|EcsRamRole|Use the RAM role of an ECS instance to do the authentication|
|RsaKeyPair|Use the RSA key pair to do the authentication \(supported only on Japan site\)|

This document uses AccessKey as an example to illustrate how to set up credentials. To ensure the security of your account, we recommend using your RAM account instead of the primary account. The primary account has full access to all of your cloud services, while the RAM account has limited access granted by the primary account to the cloud services. Firstly, create an AccessKey as described in ,

and then set up your credentials when initializing the client as follows:

**Note:** Do not disclose any code containing your AccessKey \(do not commit the code to public GitHub projects\). Otherwise, your Alibaba Cloud account may be compromised.

```
client = AcsClient(
   "<access-key-id>", 
   "<access-key-secret>",
   "<region-id>"

```

## Call a service {#section_bkb_1qj_zdb .section}

This document uses ECS as an example to show you how to use Alibaba Cloud Python SDK to send a request.

1.  Import the SDKs of the product.

    ```
    from aliyunsdkcore.client import AcsClient
    from aliyunsdkcore.acs_exception.exceptions import ClientException
    from aliyunsdkcore.acs_exception.exceptions import ServerException
    from aliyunsdkecs.request.v20140526 import DescribeInstancesRequest
    from aliyunsdkecs.request.v20140526 import StopInstanceRequest
    ```

2.  Create an AcsClient.

    ```
    client = AcsClient(
       "<your-access-key-id>", 
       "<your-access-key-secret>",
       "<your-region-id>"
    
    ```

3.  Create a request object.

    ```
    request = DescribeInstancesRequest.DescribeInstancesRequest()
    request.set_PageSize(10)
    ```

4.  Initiate a call and print the response.

    ```
    DescribeInstancesResponse response;
         try:
            response = client.do_action_with_exception(request)
            print response
        except ServerException as e:
            print e
        except ClientException as e:
    ```


