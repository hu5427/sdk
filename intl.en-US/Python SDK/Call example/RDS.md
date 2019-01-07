# RDS {#concept_zxw_xj2k_zdb .concept}

This example shows how to use Alibaba Cloud python? The SDK calls the createdbinstance interface of the RDS to create an RDS instance.

ApsaraDB for RDS \(Relational Database Service\) is a stable and reliable online database service that supports the elastic scaling function. ApsaraDB for RDS is based on the Apsara distributed system and high-performance storage of ephemeral SSD and supports MySQL, SQL Server, and PostgreSQL engines. ApsaraDB for RDS offers a complete set of solutions for backup, recovery, monitoring, migration, disaster recovery, and troubleshooting database operation and maintenance.

## Sample code {#section_lq5_3kk_zdb .section}

**Note:** Running the code in this example will create an ApsaraDB RDS instance and generate fees.

```
From Fig. Client import acsclient
From Fig. Request. v20140815 import maid
# Create acsclient instance
Client = acsclient (
"<Your-access-key-ID> ",
"<Your-access-key-secret> ",
"<Your-region-ID>"
);
Request = createdbinstancerequest. createdbinstancerequest ()
request.setEngine("MySQL");
request.setEngineVersion("5.7");
request.setDBInstanceClass("mysql.n1.micro. 1 ")
Request. Fig ("20 ")
request.setDBInstanceNetType("Intranet");
request.setSecurityIPList("0.0.0.0/0");
Request. set_paytype ("postpaid ")
request.setDBInstanceDescription("MyRds");
Request. set_clienttoken ("<UUID> ")
Response = client. Fig (request)
Print response
```

