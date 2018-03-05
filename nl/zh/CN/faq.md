---

copyright:
  years: 2018
lastupdated:  "2018-02-09"

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}


# 常见问题

此常见问题提供了对 {{site.data.keyword.mobilefoundation_long}} 服务相关常见问题的解答。
{: shortdesc}

## 如何得知 {{site.data.keyword.mobilefoundation_short}} 服务具有可用更新？
{: #maintupdates_mf}

{{site.data.keyword.mobilefoundation_short}} 会供应 {{site.data.keyword.mfserver_short_notm}}。当 {{site.data.keyword.mobilefoundation_short}} 服务器有更新时，用户会收到相应的通知。通知将显示在服务实例仪表板中。用户可以选择在自己决定的维护时段来应用 {{site.data.keyword.mobilefoundation_short}} 更新。您可以选择在您方便的时间来更新 {{site.data.keyword.mobilefoundation_short}} 服务器。


当以下某个组件更新后，即会有 {{site.data.keyword.mobilefoundation_short}} 服务更新可用。

* {{site.data.keyword.mfserver_short_notm}}.
* 底层 Liberty 版本。
* 底层 Java Developer Kit 版本。

## 如何将更新应用到 {{site.data.keyword.mobilefoundation_short}} 服务？
{: #apply_update_mf}

单击**重新创建**即可应用 {{site.data.keyword.mobilefoundation_short}} 更新。应用更新后，服务器的版本（如 {{site.data.keyword.mfp_oc_short_notm}} 中所示）将会修改为服务器更新版本。

> **注：**
>  * 用户将无法对其 {{site.data.keyword.mobilefoundation_short}} 服务实例应用自己的修订和更新。
>  * 请参阅 [Developer 套餐中的重新创建服务器](c_using_mfs_p1.html#recreate_mobilefoundation_p1)、[DeveloperPro 套餐中的重新创建服务器](c_using_mfs_p3.html#recreate_mobilefoundation_p3)、[Professional Per Capacity 套餐中的重新创建服务器](c_using_mfs_p4.html#recreate_mobilefoundation_p4)和 [Professional 1 Application 套餐中的重新创建服务器](c_using_mfs_p2.html#recreate_mobilefoundation_p2)，以了解单击**重新创建** 后不同套餐之间的行为差异。
>

## 如何为我的 {{site.data.keyword.mobilefoundation_short}} 服务器实例配置定制域？
{: #configcustomdomain}

{{site.data.keyword.mobilefoundation_short}} 供应 {{site.data.keyword.mfserver_short_notm}}，其可通过域名基于 {{site.data.keyword.Bluemix_notm}} **区域**的 URL 进行访问。您还可以配置自己的定制域。


该 URL 或路径是使用基于 {{site.data.keyword.Bluemix_notm}} `区域`的缺省域名进行创建的。

  |域|  区域|    
  |:----- | :----- |    
  |`mybluemix.net` | 美国南部|    
  |`eu-gb.mybluemix.net` | 英国|
  |`au-syd.mybluemix.net` | 悉尼|   
  |`eu-de.mybluemix.net` | 法兰克福|   
  {: caption="表 1. 基于 {{site.data.keyword.Bluemix_notm}} 中“区域”的应用程序域名" caption-side="top"}

为了可以使用您自己的域，将需要执行以下步骤来配置定制域：

1.	通过选择其中一个受支持套餐创建 {{site.data.keyword.mobilefoundation_short}} 服务实例来创建 {{site.data.keyword.mfserver_short_notm}} 实例。

+ 将要使用的定制域添加到 {{site.data.keyword.Bluemix_notm}} `组织`。转至**管理组织 > 域 > 添加域**以添加您自己的域。

+ 为<!--container group-->服务器设置路径以使用定制域。

+ 转至您的域所对应的 DNS 提供者，添加 CNAME 条目，这会将流量从您的域路由到正在运行<!--container group-->服务器的缺省 {{site.data.keyword.Bluemix_notm}} 路径。

+ 如果要为定制域配置 `https`，请将域的 SSL 证书上传到 {{site.data.keyword.Bluemix_notm}} 中。要执行此操作，请转至**管理组织 > 域**，选择要为其配置 SSL 证书的定制域，单击**上传证书**以上传域的 SSL 证书。请参阅[此帖子 ![外部链接图标](../../icons/launch-glyph.svg "外部链接图标")](https://developer.ibm.com/bluemix/2014/09/28/ssl-certificates-bluemix-custom-domains/){: new_window}，以获取更多信息。
