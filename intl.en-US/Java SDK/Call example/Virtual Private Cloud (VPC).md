# Virtual Private Cloud \(VPC\) {#concept_swc_2lk_z11db .concept}

This tutorial uses the CreateVpc API of VPC to show you how to use Alibaba Cloud Java SDK to call VPC APIs.

Virtual Private Cloud \(VPC\) is a private network established in Alibaba Cloud. VPCs are logically isolated from other virtual networks in Alibaba Cloud. You have full control over your VPC, such as specifying the IP address range of your VPC, and configuring route tables and network gateways. You can also use Alibaba Cloud resources such as ECS, RDS, and SLB in your own VPC.

## Code example {#section_lq5_3kk_zdb .section}

```
import java.util.UUID;
import com.aliyuncs.DefaultAcsClient;
import com.aliyuncs.IAcsClient;
import com.aliyuncs.exceptions.ClientException;
import com.aliyuncs.exceptions.ServerException;
import com.aliyuncs.profile.DefaultProfile;
import com.aliyuncs.vpc.model.v20160428. CreateVpcRequest;
import com.aliyuncs.vpc.model.v20160428. CreateVpcResponse;
public class Demo {
    public static void main(String[] args) {
        //Create a DefaultAcsClient instance and initialize it.
        DefaultProfile profile = DefaultProfile.getProfile(
            "<Your-region-ID>", // your region ID
            "<Your-access-key-ID>", // your AccessKey ID
            "<your-access-key-secret>"); // your AccessKey Secret
        IAcsClient client = new DefaultAcsClient(profile);
        // Create a request and set parameters
        CreateVpcRequest request = new CreateVpcRequest();
        request.setVpcName("MyVpc");
        request.setCidrBlock("192.168.0.0/16");
        request.setClientToken(UUID.randomUUID().toString());
        // Initiate a request and handle the response or exceptions
        CreateVpcResponse response;
        try {
            response = client.getAcsResponse(request);
            String vpcId = response.getVpcId();
            System.out.println("Create vpc success, vpcId = " + vpcId);
        } catch (ServerException e) {
            e.printStackTrace();
        } catch (ClientException e) {
            e.printStackTrace();
        }
    

```

