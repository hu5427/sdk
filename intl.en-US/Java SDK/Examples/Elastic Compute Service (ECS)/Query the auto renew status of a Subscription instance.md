# Query the auto renew status of a Subscription instance {#concept_svm_qzj_52b .concept}

This example introduces how to use Alibaba Cloud Java SDK to call the DescribeInstanceAutoRenewAttribute API to query the auto renew status of a Subscription instance.

## Code example {#section_djd_szj_52b .section}

```
import com.aliyuncs.DefaultAcsClient;
import com.aliyuncs.IAcsClient;
import com.aliyuncs.ecs.model.v20140526.DescribeInstanceAutoRenewAttributeRequest;
import com.aliyuncs.ecs.model.v20140526.DescribeInstanceAutoRenewAttributeResponse;
import com.aliyuncs.exceptions.ClientException;
import com.aliyuncs.exceptions.ServerException;
import com.aliyuncs.profile.DefaultProfile;

public class ECSOperate {
	private static final String ACCESS_KEY = "";
	private static final String ACCESS_SECRET = "";
	private static IAcsClient client = null;

	static {
		//Create profile
		IClientProfile profile = DefaultProfile.getProfile("cn-shenzhen", ACCESS_KEY, ACCESS_SECRET);
		//Create client
		client = new DefaultAcsClient(profile);
	}

	public static String describeInstanceAutoRenewAttribute(FormatType formatType) {
		//Create request
		DescribeInstanceAutoRenewAttributeRequest request = new DescribeInstanceAutoRenewAttributeRequest();
		request.setInstanceId("Instance ID");
		request.setAcceptFormat(formatType);
                request.setRegionId("<region-id>");
		try {
			// Get response
			return client.doAction(request).getHttpContentString();
			//Get object
			//return client.getAcsResponse(request);
		} catch (ServerException e) {
			System.out.println("Server Error " + e);
			e.printStackTrace();
			return null;
		} catch (ClientException e) {
			System.out.println("Invalid Parameter" + e);
			e.printStackTrace();
			return null;
		}
	}
	public static void main(String[] args) {
	System.out.println(describeInstanceAutoRenewAttribute(FormatType.JSON));
	}
}
```

