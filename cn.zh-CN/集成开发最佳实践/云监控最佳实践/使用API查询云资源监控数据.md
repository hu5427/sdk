# 使用API查询云资源监控数据 {#concept_221096 .concept}

大型企业内部通常有自建的运维监控系统，上云过程中会面临如何将云资源监控数据与已有系统集成的问题。本文介绍了如何通过云监控接口查询各产品监控数据，从而将阿里云的监控数据与已有系统进行集成。

## 产品架构 {#section_6fg_2j6_kab .section}

具体架构如下图所示：

![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/188516/155745284045717_zh-CN.png)

## 相关API {#section_2zi_82s_8dg .section}

在使用API查询监控数据之前，我们需要了解关于查询指标类监控数据的接口：

|API名称|说明|
|-----|--|
|[DescribeProjectMeta](https://help.aliyun.com/document_detail/114916.html)|查询产品列表：查询云监控支持哪些产品的监控项。|
|[DescribeMetricMetaList](https://help.aliyun.com/document_detail/98846.html)|查询监控项列表：查询对应产品可以获取哪些监控项。|
|[DescribeMetricList](https://help.aliyun.com/document_detail/51936.html)|查询监控数据：查询一段时间内指定产品实例的监控数据。|
|[DescribeMetricLast](https://help.aliyun.com/document_detail/51939.html)|查询监控数据：查询指定监控对象的最近一次时间点的监控数据。|

## 典型案例 {#section_kyv_7hk_rv5 .section}

-   以下代码示例展示了如何使用云监控的DescribeProjectMeta接口查询可以获取哪些云产品的监控项。

    ``` {#codeblock_hrn_n78_imi .Java}
    import com.aliyuncs.DefaultAcsClient;
    import com.aliyuncs.IAcsClient;
    import com.aliyuncs.exceptions.ClientException;
    import com.aliyuncs.exceptions.ServerException;
    import com.aliyuncs.profile.DefaultProfile;
    import com.google.gson.Gson;
    import java.util.*;
    import com.aliyuncs.cms.model.v20190101.*;
    
    public class CmsDemo {
    
        public static void main(String[] args) {
            DefaultProfile profile = DefaultProfile.getProfile("cn-hangzhou", "<accessKeyId>", "<accessKeySecret>");
            IAcsClient client = new DefaultAcsClient(profile);
    
            DescribeProjectMetaRequest request = new DescribeProjectMetaRequest();
    
            try {
                DescribeProjectMetaResponse response = client.getAcsResponse(request);
                System.out.println(new Gson().toJson(response));
            } catch (ServerException e) {
                e.printStackTrace();
            } catch (ClientException e) {
                System.out.println("ErrCode:" + e.getErrCode());
                System.out.println("ErrMsg:" + e.getErrMsg());
                System.out.println("RequestId:" + e.getRequestId());
            }
    
        }
    }
    ```

-   以下代码示例展示了如何使用云监控的DescribeMetricMetaList接口查询负载均衡产品可以获取哪些监控项。

    ``` {#codeblock_d1q_smb_2n7 .Java}
    import com.aliyuncs.DefaultAcsClient;
    import com.aliyuncs.IAcsClient;
    import com.aliyuncs.exceptions.ClientException;
    import com.aliyuncs.exceptions.ServerException;
    import com.aliyuncs.profile.DefaultProfile;
    import com.google.gson.Gson;
    import java.util.*;
    import com.aliyuncs.cms.model.v20190101.*;
    
    public class CmsDemo {
    
        public static void main(String[] args) {
            DefaultProfile profile = DefaultProfile.getProfile("cn-hangzhou", "<accessKeyId>", "<accessKeySecret>");
            IAcsClient client = new DefaultAcsClient(profile);
    
            DescribeMetricMetaListRequest request = new DescribeMetricMetaListRequest();
            request.setNamespace("acs_slb_dashboard");
    
            try {
                DescribeMetricMetaListResponse response = client.getAcsResponse(request);
                System.out.println(new Gson().toJson(response));
            } catch (ServerException e) {
                e.printStackTrace();
            } catch (ClientException e) {
                System.out.println("ErrCode:" + e.getErrCode());
                System.out.println("ErrMsg:" + e.getErrMsg());
                System.out.println("RequestId:" + e.getRequestId());
            }
    
        }
    }
    ```

-   以下代码示例展示了如何使用云监控的DescribeMetricList接口查询指定时间段内账号下所有负载均衡实例的最大连接数使用率数值。

    ``` {#codeblock_ydb_35a_cmr}
    import com.aliyuncs.DefaultAcsClient;
    import com.aliyuncs.IAcsClient;
    import com.aliyuncs.exceptions.ClientException;
    import com.aliyuncs.exceptions.ServerException;
    import com.aliyuncs.profile.DefaultProfile;
    import com.google.gson.Gson;
    import java.util.*;
    import com.aliyuncs.cms.model.v20190101.*;
    
    public class CmsDemo {
    
        public static void main(String[] args) {
            DefaultProfile profile = DefaultProfile.getProfile("cn-hangzhou", "<accessKeyId>", "<accessKeySecret>");
            IAcsClient client = new DefaultAcsClient(profile);
    
            DescribeMetricListRequest request = new DescribeMetricListRequest();
            request.setEndTime("2019-05-09 10:00:00");
            request.setStartTime("2019-05-09 00:00:00");
            request.setNamespace("acs_slb_dashboard");
            request.setMetricName("InstanceMaxConnectionUtilization");
    
            try {
                DescribeMetricListResponse response = client.getAcsResponse(request);
                System.out.println(new Gson().toJson(response));
            } catch (ServerException e) {
                e.printStackTrace();
            } catch (ClientException e) {
                System.out.println("ErrCode:" + e.getErrCode());
                System.out.println("ErrMsg:" + e.getErrMsg());
                System.out.println("RequestId:" + e.getRequestId());
            }
    
        }
    }
    ```

-   以下代码示例展示了如何使用云监控的DescribeMetricLast接口查询在当前最新时间点账号下所有负载均衡实例的最大连接数使用率数值。

    ``` {#codeblock_khy_rze_njx .Java}
    import com.aliyuncs.DefaultAcsClient;
    import com.aliyuncs.IAcsClient;
    import com.aliyuncs.exceptions.ClientException;
    import com.aliyuncs.exceptions.ServerException;
    import com.aliyuncs.profile.DefaultProfile;
    import com.google.gson.Gson;
    import java.util.*;
    import com.aliyuncs.cms.model.v20190101.*;
    
    public class CmsDemo {
    
        public static void main(String[] args) {
            DefaultProfile profile = DefaultProfile.getProfile("cn-hangzhou", "<accessKeyId>", "<accessSecret>");
            IAcsClient client = new DefaultAcsClient(profile);
    
            DescribeMetricLastRequest request = new accessKeySecretDescribeMetricLastRequest();
            request.setNamespace("acs_slb_dashboard");
            request.setMetricName("InstanceMaxConnectionUtilization");
    
            try {
                DescribeMetricLastResponse response = client.getAcsResponse(request);
                System.out.println(new Gson().toJson(response));
            } catch (ServerException e) {
                e.printStackTrace();
            } catch (ClientException e) {
                System.out.println("ErrCode:" + e.getErrCode());
                System.out.println("ErrMsg:" + e.getErrMsg());
                System.out.println("RequestId:" + e.getRequestId());
            }
    
        }
    }
    ```


**说明：** 

-   DescribeMetricList和DescribeMetricLast接口支持批量获取用户下所有实例的某个指标的数据。如果想获取多个指标，可以多个线程获取多个指标，也可以单线程循环获取多个指标。
-   DescribeMetricList接口支持的最大QPS是20，DescribeMetricLast接口的最大QPS是30。
-   DescribeMetricLast接口适用于需要定时全量拉取所有最新数据的情况。时间窗口自动往前滑动，每个周期都取一条最新数据。
-   监控数据会有一定的延迟，且各产品的监控数据的延迟情况不太一样，所以建议您使用QueryMetricLast查询最新数据时，时间窗口放宽到5-10分钟。
-   秒级精度的数据保存7天，分钟级精度的数据保存31天。
-   如果您需要查询云账号所有实例的数据，则不需要指定Dimensions。

## 相关文档 {#section_ian_245_xfu .section}

[安装Alibaba Cloud SDK for Java](../../../../cn.zh-CN/Java SDK/安装Alibaba Cloud SDK for Java.md#)

[云监控API参考](../../../../cn.zh-CN/API参考/API概览.md#)

