# Configure STS token {#concept_ixf_gbk_z62db .concept}

Using your Alibaba Cloud AccessKey directly to develop applications will have potential security risk. To enhance your account security, you can use the Security Token Service \(STS\) token issued for a subaccount to access Alibaba Cloud services.

## Introduction to STS token {#section_npt_pbk_zdb .section}

Security Token Service is a cloud service that provides short-term access control for Alibaba Cloud accounts \(or RAM users\). Through STS, you can issue an access credentials with custom time limit and access right to federated users \(users managed by your local account system\). Federated users can use STS short-term access credentials to call the Alibaba Cloud APIs directly, or log on to the Alibaba Cloud console to manage the authorized resources.

Using the STS token as an access credential has the following advantages:

-   Using STS token will reduce the risks of a compromised AccessKey ID and AccessKey Secret, particularly reducing risks for your mobile devices.

-   STS token has flexible permission control. You can control the access permission in a finer granularity for products including SLB and ECS, according to the RAM role.


**Note:** Make sure that the product you are calling supports STS.

## Set up the STS token {#section_dh2_pck_zdb .section}

Two methods are available to set up the STS token:

-   Method 1: Use STS token directly

    You can directly specify the STS token. However, you must update the token periodically if you directly specify the token.

    ```
    from aliyunsdkcore.client import AcsClient
    from aliyunsdkcore.auth.credentials import StsTokenCredential
    sts_token_credential = StsTokenCredential('sts_access_key_id', 'sts_access_key_secret', 'sts_session_token')
    acs_client = AcsClient(region_id='cn-hangzhou', credential=sts_token_credential)
    ```

    where:

    -   region-id is the ID of the region that you are using.  You can obtain the region ID by calling the DescribeRegions API.
    -   sts-access-key-id, sts-access-key-secret, and sts-session-token are credentials returned by calling the AssumeRole API.
-   Method 2: Use SDK to manage STS tokens

    You can create a new NewClientWithRamRoleArn object to allow Alibaba Cloud Go SDK to create and maintain STS tokens.

    ```
    from aliyunsdkcore.client import AcsClient
    from aliyunsdkcore.auth.credentials import RamRoleArnCredential
    
    ram_role_arn_credential = RamRoleArnCredential('access_key_id', 'access_key_secret', 'role_arn', 'role_session_name')
    acs_client = AcsClient(region_id='cn-hangzhou', credential=ram_role_arn_credential)
    ```

    where:

    -   role-arn is the role resource descriptor. You can obtain it on the role details page from the [RAM console](https://ram.console.aliyun.com/role/list?spm=a2c4g.11186623.2.7.IjY04Z#/role/list).

    -   role-session-name is a temporary role name. You can call the AssumeRole API to create a temporary identity. After the temporary identity is created, you can use the value set for the RoleSessionName parameter as the role-session-name parameter in this method.


