# 获取ECS实例配置 {#concept_ksg_t1j_52b .concept}

 本示例介绍如何使用阿里云Java SDK调用DescribeInstancesRequest接口获取ECS实例的配置信息。

## 示例代码 {#section_tlp_hbj_52b .section}

```

import com.aliyuncs.DefaultAcsClient;
import com.aliyuncs.IAcsClient;
import com.aliyuncs.ecs.model.v20140526.DescribeInstancesRequest;
import com.aliyuncs.ecs.model.v20140526.DescribeInstancesResponse;
import com.aliyuncs.exceptions.ClientException;
import com.aliyuncs.exceptions.ServerException;
import com.aliyuncs.profile.DefaultProfile;

public class ECSOperate {

	private static final String ACCESS_KEY = "";
	private static final String ACCESS_SECRET = "";
	private static IAcsClient client = null;

	static {
		// 创建profile
		IClientProfile profile = DefaultProfile.getProfile("cn-shenzhen", ACCESS_KEY, ACCESS_SECRET);
		// 创建Acs client
		client = new DefaultAcsClient(profile);
	}
	
	public static String describeInstanceAttribute(FormatType formatType) {
		//构造请求
		DescribeInstanceAttributeRequest request = new DescribeInstanceAttributeRequest();
		request.setInstanceId(实例ID);
		request.setAcceptFormat(formatType);
		try {
			//获取返回内容
			return client.doAction(request).getHttpContentString();
			//获取返回对象
//			return client.getAcsResponse(request);
		} catch (ServerException e) {
			System.out.println("服务器异常 " + e);
			e.printStackTrace();
			return null;
		} catch (ClientException e) {
			System.out.println("参数配置异常 " + e);
			e.printStackTrace();
			return null;
		}
	}
	
	public static void main(String[] args) {
		System.out.println(describeInstanceAttribute
 (FormatType.JSON));
	}
}
```

