# Use CommonRequest {#concept_ivk_p1k_zdb .concept}

If an Alibaba Cloud product does not provide an SDK for its APIs, you can use the generic calling method \(CommonRequest\) to call the product APIs. By using the CommonRequest calling method,  you can call any API.

## Characteristics of CommonRequest calling {#section_kwk_51k_zdb .section}

The CommonRequest calling method has the following characteristics:

1.  Light weight: You only need to download the core package, and do not need to download and install all product SDKs.
2.  Easy and convenient: You can use the latest API without updating the SDK.
3.  Fast iteration: Updates are made often and are made available quickly.

## Call an API using CommonRequest {#section_sc1_v1k_zdb .section}

The APIs of Alibaba Cloud products can be classified into two types, RPC and RESTful styles. The method of making a CommonRequest request varies based on the specific API type.

In general, the API required by the Action parameter belongs to the RPC type and the API required by the PathPattern parameter belongs to the RESTful type. In general, all APIs in a product are of the same type. Each API only supports calling of a specific type. Entering wrong codes may call other APIs or receive the error message of `ApiNotFound`.

To make a CommonRequest request, you must obtain the values for the following parameters. You can obtain the values of these parameters from the API documents at the [Document Center](https://help.aliyun.com/). Besides, you can obtain the parameters of an API through [OpenAPI Explorer](https://api.aliyun.com/).

-   Domain: The universal domain name of a product.

-   Version: The version of the API, in the format of YYYY-MM-DD. 

    You can get the API version from the common parameters in the API document of a product.

-   API information: The name of the API to call.
    -   If an API is an RPC API, such as ECS and RDS, you must obtain the value of the Action parameter and specify the API to call in the form of `request.ApiName = "<Action>"`

        For example, if the Action value of the ECS-RunInstances API is RunInstances, you can use `request.ApiName = "RunInstances"` to specify the API to call when making a CommonRequest request.

    -   If an API is a RESTful API, such as Container Service, you must obtain the value of the PathPattern parameter and specify the RESTful path to call in the form of `request.PathPattern = "<PathPattern>"`.

        For example, the PathPattern value of the CS-GetClusterList API is `/clusters`, you can use `request.PathPattern = "/clusters"` to specify the RESTful path when making a CommonRequest request.


## Example: Call an RPC API {#section_vsg_w1k_zdb .section}

The following code shows how to use the CommonRequest calling method to call the DescribeInstanceStatus API:

```
from aliyunsdkcore.client import AcsClient
from aliyunsdkcore.request import CommonRequest
client = AcsClient('{your_access_key_id}', '{your_access_key_secret}', '{your_region_id}')
request = CommonRequest()
request.set_domain('ecs.aliyuncs.com')
request.set_version('2014-05-26')
request.set_action_name('DescribeInstanceStatus')
# or:
# request = CommonRequest(domain='ecs.aliyuncs.com', version='2014-05-26', action_name='DescribeInstanceStatus')
request.add_query_param('PageNumber', '1')
request.add_query_param('PageSize', '30')
response = client.do_action_with_exception(request)
```

## Example: Call a RESTful API {#section_yxp_1bk_zdb .section}

The following code shows how to use the CommonRequest calling method to call the GetClusterList API:

```
from aliyunsdkcore.client import AcsClient
from aliyunsdkcore.request import CommonRequest
client = AcsClient('{your_access_key_id}', '{your_access_key_secret}', '{your_region_id}')
request = CommonRequest()
request.set_domain('cs.aliyuncs.com')
request.set_version('2015-12-15')
request.set_uri_pattern('/clusters')
# or:
# request = CommonRequest(domain='ecs.aliyuncs.com', version='2014-05-26', uri_pattern='/clusters')
response = client.do_action_with_exception(request)
```

