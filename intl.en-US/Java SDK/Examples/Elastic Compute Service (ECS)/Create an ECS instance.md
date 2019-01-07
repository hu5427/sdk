# Create an ECS instance {#concept_zxw_xjk_zdb .concept}

The example shows how to use Alibaba Cloud Java SDK to call the CreateInstance API to create an ECS instance.

Before creating an ECS instance, you must obtain the following information:

-   Image ID

    Obtain the ID of the image to use by calling the DescribeImages API.

-   Specifications

    Select the instance type to use. For more information, see [Instance type families](../../../../../reseller.en-US/Product Introduction/Instance type families.md#).


## Code example {#section_lq5_3kk_zdb .section}

**Note:** Running the codes in this example will create an ECS instance and generate fees.

```

import com.aliyuncs.DefaultAcsClient;
import com.aliyuncs.IAcsClient;
import com.aliyuncs.ecs.model.v20140526.CreateInstanceRequest;
import com.aliyuncs.ecs.model.v20140526.CreateInstanceResponse;
import com.aliyuncs.exceptions.ClientException;
import com.aliyuncs.exceptions.ServerException;
import com.aliyuncs.profile.DefaultProfile;
public class Demo {
    public static void main(String[] args) {
        // Create and initiate DefaultAcsClient
        DefaultProfile profile = DefaultProfile.getProfile(
            "<your-region-id>",                     //Region ID
            "<your-access-key-id>",             // AccessKey ID
            "<your-access-key-secret>");        // AccessKey Secret
        IAcsClient client = new DefaultAcsClient(profile);
        //Call an API and configure API parameters
        CreateInstanceRequest request = new RunInstancesRequest();
        request.setImageId("alinux_17_01_64_20G_cloudinit_20171222.vhd");
        request.setInstanceName("MyEcsInstance");
        request.setSecurityGroupId("<your-security-group-id>");
        request.setInstanceType("ecs.g5.large");
        request.setClientToken(UUID.randomUUID().toString());
        request.setVSwitchId("your-vswitch-id");
        // Handle response
        RunInstancesResponse response;
        try {
            response = client.getAcsResponse(request);
            String instanceId = response.getInstanceId();
            System.out.println("Create instance success, instanceId = " + instanceId);
        } catch (ServerException e) {
            e.printStackTrace();
        } catch (ClientException e) {
            e.printStackTrace();
        }
    }
}
```

