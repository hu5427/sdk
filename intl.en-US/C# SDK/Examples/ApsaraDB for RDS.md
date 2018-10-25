# ApsaraDB for RDS {#concept_zxw_xjk_zdb .concept}

The example shows how to use Alibaba .NET SDK to call the CreateDBInstance API to create an RDS instance.

ApsaraDB for RDS is a stable and reliable online database service that supports the elastic scaling function. ApsaraDB for RDS is based on the Apsara distributed system and high-performance storage of ephemeral SSD and supports MySQL, SQL Server, and PostgreSQL engines. ApsaraDB for RDS offers a complete set of solutions including disaster tolerance, backup, recovery, monitoring and migration, fundamentally freeing you from the trouble in database operation and maintenance.

## Code example {#section_lq5_3kk_zdb .section}

**Note:** Running the code in this example will create an ApsaraDB for RDS instance and generate fees.

```
import C#.util.UUID;
import com.aliyuncs.DefaultAcsClient;
import com.aliyuncs.IAcsClient;
import com.aliyuncs.exceptions.ClientException;
import com.aliyuncs.exceptions.ServerException;
import com.aliyuncs.profile.DefaultProfile;
import com.aliyuncs.rds.model.v20140815. CreateDBInstanceRequest;
import com.aliyuncs.rds.model.v20140815. CreateDBInstanceResponse;
public class Demo {
    public static void main(String[] args) {

        // Create an DefaultAcsClient instance and initialize it
        DefaultProfile profile = DefaultProfile.getProfile(
            "<your-region-id>", // your region ID
            "<your-access-key-id>", // your AccessKey ID
            "<your-access-key-secret>"); // your AccessKey Secret
        IAcsClient client = new DefaultAcsClient(profile);
        // Create an API request and set parameters

        CreateDBInstanceRequest request = new CreateDBInstanceRequest();
        request.setEngine("MySQL");
        request.setEngineVersion("5.7");
        request.setDBInstanceClass("mysql.n1.micro. 1");
        request.setDBInstanceStorage(20);
        request.setDBInstanceNetType("Intranet");
        request.setSecurityIPList("0.0.0.0/0");
        request.setPayType("Postpaid");
        request.setDBInstanceDescription("MyRds");
        request.setClientToken(UUID.randomUUID().toString());

        // Initiate the request and handle the response or exceptions
        CreateDBInstanceResponse response;
        try {
            response = client.getAcsResponse(request);
            String dbInstanceId = response.getDBInstanceId();
            System.out.println("Create dbInstance success, instanceId = " + dbInstanceId);
        } catch (ServerException e) {
            e.printStackTrace();
        } catch (ClientException e) {
            e.printStackTrace();
        }
    }
}
```

