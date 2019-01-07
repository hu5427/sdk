# Query security groups {#concept_f4h_d2k_52b .concept}

This example shows how to use Alibaba Cloud Java SDK to call the DescribeSecurityGroupsRequest API to query created security groups.

## Code example {#section_obd_g2k_52b .section}

```
import com.aliyuncs.DefaultAcsClient;
import com.aliyuncs.IAcsClient;
import com.aliyuncs.ecs.model.v20140526. DescribeSecurityGroupsRequest;
import com.aliyuncs.ecs.model.v20140526. DescribeSecurityGroupsResponse;
import com.aliyuncs.exceptions.ClientException;
import com.aliyuncs.exceptions.ServerException;
import com.aliyuncs.profile.DefaultProfile;

public class ECSOperate {

	private static final String ACCESS_KEY = "";
	private static final String ACCESS_SECRET = "";
	private static IAcsClient client = null;

	static {
		// Create a profile
		IClientProfile profile = DefaultProfile.getProfile("cn-shenzhen", ACCESS_KEY, ACCESS_SECRET);
		// Create a client instance
		client = new DefaultAcsClient(profile);
	}

	public static String describeSecurityGroups(FormatType formatType) {
		// Construct a request
		DescribeSecurityGroupsRequest request = new DescribeSecurityGroupsRequest();
		request.setAcceptFormat(formatType);
		try {
			//Obtain the returned content
			return client.doAction(request).getHttpContentString();
			//Obtain the returned object
			//return client.getAcsResponse(request);
		} catch (ServerException e) {
			System.out.println("Server exception: " + e);
			e.printStackTrace();
			return null;
		} catch (ClientException e) {
			System.out.println("Parameter configuration error: " + e);
			e.printStackTrace();
			return null;
		}
	}
	
	public static void main(String[] args) {
		System.out.println(describeSecurityGroups(FormatType.JSON));
	}
}
```

