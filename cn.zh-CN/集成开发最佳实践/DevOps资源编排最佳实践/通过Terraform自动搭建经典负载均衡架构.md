# 通过Terraform自动搭建经典负载均衡架构 {#concept_222063 .concept}

本文介绍了如何基于Terraform来实现一个经典的云基础架构自动化编排的创建。该架构包含云服务器ECS，负载均衡SLB，数据库RDS，承载计算资源的网络资源专有网VPC、交换机VSwitch和安全组，以及持久化存储OSS。

## 背景信息 {#section_9qr_xzz_6j7 .section}

[HashiCorp Terraform](https://www.terraform.io/?spm=a2c4g.11186623.2.11.55e267a38YzsUB)是一个IT基础架构自动化编排工具，可以用代码来管理维护IT资源。Terraform的命令行接口提供了一种简单机制，用于将配置文件部署到阿里云或其他任意支持的云上，并对其进行版本控制。它编写了描述云资源拓扑的配置文件中的基础结构，例如虚拟机、存储账户和网络接口。

Terraform是一个高度可扩展的工具，通过Provider来支持新的基础架构。Terraform能够让您在阿里云上轻松使用[简单模板语言](https://www.terraform.io/docs/configuration/syntax.html)来定义、预览和部署云基础结构。您可以使用Terraform来创建、修改、删除ECS、VPC、RDS、SLB等多种资源。相比其他开源资源管理工具，Terraform具有如下优势：

-   将基础结构部署到多个云

    Terraform适用于多云方案，将相类似的基础结构部署到阿里云、其他云提供商或者本地数据中心。开发人员能够使用相同的工具和相似的配置文件同时管理不同云提供商的资源。

-   自动化管理基础结构

    Terraform能够创建配置文件的模板，以可重复、可预测的方式定义、预配和配置ECS资源，减少因人为因素导致的部署和管理错误。能够多次部署同一模板，创建相同的开发、测试和生产环境。

-   基础架构即代码（Infrastructure as Code）

    可以用代码来管理维护资源。允许保存基础设施状态，从而使您能够跟踪对系统（基础设施即代码）中不同组件所做的更改，并与其他人共享这些配置。

-   降低开发成本

    您通过按需创建开发和部署环境来降低成本。并且，您可以在系统更改之前进行评估。


## 产品架构 {#section_91p_v73_91x .section}

通过Terraform自动搭建经典负载均衡架构的示意图如下：

![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/189347/155745317945967_zh-CN.png)

## 相关资源 {#section_ybx_7ys_qgs .section}

通过以下云资源的配置信息搭建经典架构：

|资源名称|说明|
|----|--|
|[alicloud\_instance](https://www.terraform.io/docs/providers/alicloud/r/instance.html)|创建ECS实例。|
|[alicloud\_vpc](https://www.terraform.io/docs/providers/alicloud/r/vpc.html)|新建专有网络。|
|[alicloud\_vswitch](https://www.terraform.io/docs/providers/alicloud/r/vswitch.html)|新建交换机。|
|[alicloud\_slb](https://www.terraform.io/docs/providers/alicloud/r/slb.html)|创建LoadBalancer。|
|[alicloud\_slb\_listener](https://www.terraform.io/docs/providers/alicloud/r/slb_listener.html)|创建负载均衡监听。|
|[alicloud\_slb\_attachment](https://www.terraform.io/docs/providers/alicloud/r/slb_attachment.html)|挂载ECS实例。|
|[alicloud\_security\_group](https://www.terraform.io/docs/providers/alicloud/r/security_group.html)|创建安全组。|
|[alicloud\_security\_group\_rule](https://www.terraform.io/docs/providers/alicloud/r/security_group_rule.html)|创建安全组规则。|
|[alicloud\_db\_instance](https://www.terraform.io/docs/providers/alicloud/r/db_instance.html)|创建数据库实例。|
|[alicloud\_db\_database](https://www.terraform.io/docs/providers/alicloud/r/db_database.html)|创建数据库。|
|[alicloud\_db\_account](https://www.terraform.io/docs/providers/alicloud/r/db_account.html)|创建数据库账号。|
|[alicloud\_oss\_bucket](https://www.terraform.io/docs/providers/alicloud/r/oss_bucket.html)|创建OSS bucket。|

## 典型案例 {#section_1km_qmn_su2 .section}

以下示例定义了一个用于一键部署经典负载均衡架构的Terraform模板，Terraform将基于该模板首先创建出资源所需要的网络环境VPC、VSwitch、SLB以及安全组，然后在该网络环境中创建ECS，RDS等计算资源，最后在SLB中创建监听并将ECS实例挂载到SLB上，成为其后端服务器。

其中每个资源的定义都包含了count字段，用于设置所要创建的资源的个数。模板中的DataSource为创建Resource提供了动态获取属性和参数的能力，您无需关心这些参数的具体值。如通过指定镜像（image）的name\_regex来动态获取符合特定条件的镜像ID，通过指定cpu\_core\_count和memory\_size来动态获取符合CPU和内存空间大小的实例规格。

**说明：** 以下代码是部分模板示例，获取完整模板请访问[terraform-alicloud-classic-load-balance](https://github.com/terraform-alicloud-modules/terraform-alicloud-classic-load-balance)。

``` {#codeblock_3g7_3fs_4us}
// Images data source to get image_id
data "alicloud_images" "default" {
  most_recent = true
  owners      = "system"
  name_regex  = "${var.image_name_regex}"
}

// Instance_types data source to get instance_type
data "alicloud_instance_types" "default" {
  cpu_core_count = "${var.cpu_core_count}"
  memory_size    = "${var.memory_size}"
}

// Zones data source to get availability_zone
data "alicloud_zones" "default" {
  available_instance_type     = "${data.alicloud_instance_types.default.instance_types.0.id}"
  available_resource_creation = "Rds"
}

// If you do not specify vpc_id, the module will create a new VPC
resource "alicloud_vpc" "vpc" {
  count      = "${var.vpc_id == "" ? 1 : 0}"
  cidr_block = "${var.vpc_cidr}"
  name       = "${var.vpc_name == "" ? var.resource_group_name : var.vpc_name}"
}

// Create VSwitches
resource "alicloud_vswitch" "vswitches" {
  count             = "${length(var.vswitch_ids) > 0 ? 0 : length(var.vswitch_cidrs)}"
  vpc_id            = "${var.vpc_id == "" ? join("", alicloud_vpc.vpc.*.id) : var.vpc_id}"
  cidr_block        = "${element(var.vswitch_cidrs, count.index)}"
  availability_zone = "${lookup(data.alicloud_zones.default.zones[count.index], "id")}"
  name              = "${var.vswitch_name_prefix == "" ? format("%s-%s", var.resource_group_name, format(var.number_format, count.index+1)) : format("%s-%s", var.vswitch_name_prefix, format(var.number_format, count.index+1))}"
}

// Security Group Resource for Module
resource "alicloud_security_group" "default" {
  vpc_id = "${var.vpc_id == "" ? join("", alicloud_vpc.vpc.*.id) : var.vpc_id}"
  name   = "${var.group_name == "" ? var.resource_group_name : var.group_name}"
}

// ECS Instance Resource for Web Tier
resource "alicloud_instance" "web" {
  count = "${var.number_of_web_instances}"

  image_id        = "${var.image_id == "" ? data.alicloud_images.default.images.0.id : var.image_id }"
  instance_type   = "${var.instance_type == "" ? data.alicloud_instance_types.default.instance_types.0.id : var.instance_type}"
  security_groups = ["${ alicloud_security_group.default.id }"]

  instance_name = "${var.number_of_web_instances < 2 ? var.web_instance_name : format("%s-%s", var.web_instance_name, format(var.number_format, count.index+1))}"
  host_name     = "${var.number_of_web_instances < 2 ? var.web_host_name : format("%s-%s", var.web_host_name, format(var.number_format, count.index+1))}"

  internet_charge_type       = "${var.internet_charge_type}"
  internet_max_bandwidth_out = "${var.internet_max_bandwidth_out}"

  instance_charge_type = "${var.instance_charge_type}"
  system_disk_category = "${var.system_category}"
  system_disk_size     = "${var.system_size}"

  password = "${var.ecs_password}"

  vswitch_id = "${length(var.vswitch_ids) > 0 ? element(split(",", join(",", var.vswitch_ids)), count.index%length(split(",", join(",", var.vswitch_ids)))) : length(var.vswitch_cidrs) < 1 ? "" : element(split(",", join(",", alicloud_vswitch.vswitches.*.id)), count.index%length(split(",", join(",", alicloud_vswitch.vswitches.*.id))))}"

  period      = "${var.period}"
  period_unit = "${var.period_unit}"
}

// ECS Instance Resource for app Tier
resource "alicloud_instance" "app" {
  count = "${var.number_of_app_instances}"

  image_id        = "${var.image_id == "" ? data.alicloud_images.default.images.0.id : var.image_id }"
  instance_type   = "${var.instance_type == "" ? data.alicloud_instance_types.default.instance_types.0.id : var.instance_type}"
  security_groups = ["${alicloud_security_group.default.id}"]

  instance_name = "${var.number_of_app_instances < 2 ? var.app_instance_name : format("%s-%s", var.app_instance_name, format(var.number_format, count.index+1))}"
  host_name     = "${var.number_of_app_instances < 2 ? var.app_host_name : format("%s-%s", var.app_host_name, format(var.number_format, count.index+1))}"

  internet_charge_type       = "${var.internet_charge_type}"
  internet_max_bandwidth_out = "${var.internet_max_bandwidth_out}"

  instance_charge_type = "${var.instance_charge_type}"
  system_disk_category = "${var.system_category}"
  system_disk_size     = "${var.system_size}"

  password = "${var.ecs_password}"

  vswitch_id = "${length(var.vswitch_ids) > 0 ? element(split(",", join(",", var.vswitch_ids)), count.index%length(split(",", join(",", var.vswitch_ids)))) : length(var.vswitch_cidrs) < 1 ? "" : element(split(",", join(",", alicloud_vswitch.vswitches.*.id)), count.index%length(split(",", join(",", alicloud_vswitch.vswitches.*.id))))}"

  period      = "${var.period}"
  period_unit = "${var.period_unit}"
}

// SLB Instance Resource for intranet
resource "alicloud_slb" "intranet" {
  internet = false
  name     = "${var.slb_intranet_name == "" ? var.resource_group_name : var.slb_intranet_name}"
}

resource "alicloud_slb_attachment" "intranet" {
  load_balancer_id = "${alicloud_slb.intranet.id}"
  instance_ids     = ["${alicloud_instance.web.*.id}", "${alicloud_instance.app.*.id}"]
}

// SLB Instance Resource for internet
resource "alicloud_slb" "internet" {
  internet  = true
  bandwidth = "${var.slb_max_bandwidth}"
  name      = "${var.slb_internet_name == "" ? var.resource_group_name : var.slb_internet_name}"
}

resource "alicloud_slb_attachment" "internet" {
  load_balancer_id = "${alicloud_slb.internet.id}"
  instance_ids     = ["${alicloud_instance.web.*.id}"]
}

// RDS Resource
resource "alicloud_db_instance" "default" {
  count            = "${var.number_of_rds_instances}"
  instance_name    = "${var.number_of_rds_instances < 2 ? var.rds_name_prefix : format("%s-%s", var.rds_name_prefix, format(var.number_format, count.index+1))}"
  engine           = "${var.engine}"
  engine_version   = "${var.engine_version}"
  instance_type    = "${var.db_instance_type}"
  instance_storage = "${var.storage}"
  vswitch_id       = "${length(var.vswitch_ids) > 0 ? element(split(",", join(",", var.vswitch_ids)), count.index%length(split(",", join(",", var.vswitch_ids)))) : length(var.vswitch_cidrs) < 1 ? "" : element(split(",", join(",", alicloud_vswitch.vswitches.*.id)), count.index%length(split(",", join(",", alicloud_vswitch.vswitches.*.id))))}"
  security_ips     = ["${alicloud_instance.app.*.private_ip}"]
}

resource "alicloud_db_account" "default" {
  count       = "${var.number_of_rds_instances}"
  instance_id = "${element(alicloud_db_instance.default.*.id, count.index)}"
  name        = "${var.rds_account_name_prefix}${count.index}"
  password    = "${var.rds_account_password}"
}

resource "alicloud_db_database" "default" {
  count       = "${var.number_of_rds_instances}"
  instance_id = "${element(alicloud_db_instance.default.*.id, count.index)}"
  name        = "${var.rds_database_name_prefix}_${count.index}"
}

// OSS Resource
resource "alicloud_oss_bucket" "default" {
  bucket = "${var.bucket_name == "" ? var.resource_group_name : var.bucket_name}"
  acl    = "${var.bucket_acl}"
}
```

## 运行Terraform模板 {#section_opm_a1z_ve5 .section}

参考以下步骤，运行该模板：

**说明：** 目前阿里云已经提供了丰富多样的[Terraform Modules](https://www.terraform.io/docs/configuration/modules.html)，并在[Terraform Registry](https://registry.terraform.io/browse?provider=alicloud)完成了注册和验证，您也可以通过[classic-load-balance Module](https://registry.terraform.io/modules/aliyun/classic-load-balance/alicloud/2.1.0)的方式来运行。

1.  通过环境变量设置AccessKey ID、AccessKey Secret和地域。

    您可以登录[访问控制管理控制台](https://usercenter.console.aliyun.com/#/manage/ak)创建并查看AccessKey，或者联系系统管理员获取AccessKey。

    ``` {#codeblock_gln_ize_coz}
    $ export ALICLOUD_ACCESS_KEY=XXXXX
    $ export ALICLOUD_SECRET_KEY=XXXXX
    $ export ALICLOUD_REGION=regionID 
    ```

2.  下载完整模板。

    ``` {#codeblock_7tk_iep_m11}
    $ git clone https://github.com/terraform-alicloud-modules/terraform-alicloud-classic-load-balance.git
    ```

3.  运行以下的Terraform命令，完成资源的创建。
    1.  进入模板根目录：

        ``` {#codeblock_3fp_ulx_1q8}
         $ cd terraform-alicloud-classic-load-balance
        ```

    2.  预览要创建的资源：

        ``` {#codeblock_7za_c21_60l}
        $ terraform plan
        ```

    3.  创建资源：

        ``` {#codeblock_lno_hc5_hls}
        $ terraform apply
        ```

    4.  释放已创建的资源：

        ``` {#codeblock_cxe_gwf_zzj}
        $ terraform destroy
        ```


