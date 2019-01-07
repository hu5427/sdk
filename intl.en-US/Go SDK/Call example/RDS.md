# RDS {#concept_zxw_xjk_zdb1 .concept}

This example shows how to use the Ali cloud go SDK to call an RDS's createdbinstance interface to create an RDS instance.

ApsaraDB for RDS \(Relational Database Service\) is a stable and reliable online database service that supports the elastic scaling function. ApsaraDB for RDS is based on the Apsara distributed system and high-performance storage of ephemeral SSD and supports MySQL, SQL Server, and PostgreSQL engines. ApsaraDB for RDS offers a complete set of solutions for backup, recovery, monitoring, migration, disaster recovery, and troubleshooting database operation and maintenance.

## Sample code {#section_lq5_3kk_zdb .section}

**Note:** Running the code in this example will create an ApsaraDB RDS instance and generate fees.

```
Package main

Import (
    "Maid"
    "Maid"
    "FMT"
)

Func main (){ 
    // Create client instance
    Client, err: = RDS. Fig (
    "<Your-region-ID>", // your Region ID
    "<Your-access-key-ID>", // your accesskey ID
    "<Your-access-key-secret>") // your accesskey secret
    If erir! = Nell {
        // Exception Handling
        Panic (Elgin)
    }
    // Create a request and set parameters
    Request: = RDS. Fig ()
    Request. Engine = "MySQL"
    Request. engineversion = "5.7"
    request.setDBInstanceClass("mysql.n1.micro. 1"
    Request. Fig = "20"
    Request. Fig = "intranet"
    Request. securityiplist = "0.5.0.0/0"
    Request. paytype = "postpaid"
    Request. Fig = "Maid ds"
    Request. clienttoken = utils. getreuidv4 () 
    Response, Elgin: = client. createdbinstance (request)
    If erir! = Nell {
        // Exception Handling
        Panic (Elgin)
    }
    FMT. printf ("Success (% d )! Dbinstanceid = % s \ n ", response. gethttpstatus (), response. dbinstanceid)
}
```

