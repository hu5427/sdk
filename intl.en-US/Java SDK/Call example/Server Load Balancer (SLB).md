# Server Load Balancer \(SLB\) {#concept_swc_2l65k_zdb .concept}

This tutorial uses the CreateLoadBalancer API of SLB to show you how to use Alibaba Cloud Java SDK to call SLB APIs.

Alibaba Cloud Server Load Balancer \(SLB\) is a traffic distribution control service that distributes the incoming traffic among multiple Elastic Compute Service \(ECS\) instances according to the configured forwarding rules. It expands the service capabilities of the application and increases the availability of the application.

## Code example {#section_lq5_3kk_zdb .section}

**Note:** Running the code in this example will create an SLB instance and generate fees.

```
import java.util.UUID;
import com.aliyuncs.DefaultAcsClient;
import com.aliyuncs.IAcsClient;
import com.aliyuncs.exceptions.ClientException;
import com.aliyuncs.exceptions.ServerException;
import com.aliyuncs.profile.DefaultProfile;
import com.aliyuncs.slb.model.v20140515. CreateLoadBalancerRequest;
import com.aliyuncs.slb.model.v20140515. CreateLoadBalancerResponse;
public class Demo {
    public static void main(String[] args) {
        //Create a DefaultAcsClient instance and initialize it.
        DefaultProfile profile = DefaultProfile.getProfile(
            "<Your-region-ID>", // your region ID
            "<your-access-key-id>", // your AccessKey ID
            "<your-access-key-secret>"); // your AccessKey Secret        
        IAcsClient client = new DefaultAcsClient(profile);
        // Create a request and set parameters
        CreateLoadBalancerRequest request = new CreateLoadBalancerRequest();
        request.setLoadBalancerName("MyLoadBalancer");
        request.setAddressType("internet");
        request.setClientToken(UUID.randomUUID().toString());
        // Initiate a request and handle the response or exceptions
        CreateLoadBalancerResponse response;
        try {
            response = client.getAcsResponse(request);
            String loadBalancerId = response.getLoadBalancerId();
            System.out.println("Create loadBalancer success, loadBalancerId = " + loadBalancerId);
        } catch (ServerException e) {
            e.printStackTrace();
        } catch (ClientException e) {
            e.printStackTrace();
        }
    

```

