# Virtual Private Cloud \(VPC\) {#concept_swc_2lk_zdb .concept}

This tutorial uses the CreateVpc API of VPC to show you how to use Alibaba Cloud Go SDK to call VPC APIs.

Virtual Private Cloud \(VPC\) is a private network established in Alibaba Cloud. VPCs are logically isolated from other virtual networks in Alibaba Cloud. You have full control over your VPC, such as specifying the IP address range of your VPC, and configuring route tables and network gateways. You can also use Alibaba Cloud resources such as ECS, RDS, and SLB in your own VPC.

## Code example {#section_lq5_3kk_zdb .section}

```
package main

import (
    "github.com/aliyun/alibaba-cloud-sdk-go/services/vpc"
    "github.com/aliyun/alibaba-cloud-sdk-go/sdk/utils"
    "fmt"


func main() { 
    // Create a client instance
    client, err := vpc.NewClientWithAccessKey(
        "<your-region-id>", // your region ID
        "<your-access-key-id>", // your AccessKey ID
        "<your-access-key-secret>") // your AccessKey Secret
    if err ! = nil {
        // Handle exceptions
        panic(err)
    
    // Create a request and set parameters
    request := vpc.CreateCreateVpcRequest()
    request.VpcName = "MyVpc"
    request.CidrBlock = "192.168.0.0/16"
    request.ClientToken = utils.GetUUIDV4()
    response, err := client.CreateVpc(request)
    if err ! = nil {
        // Handle exceptions
        panic(err)
    
    fmt.Printf("success(%d)! vpcId = %s\n", response.GetHttpStatus(), response.VpcId)

```

