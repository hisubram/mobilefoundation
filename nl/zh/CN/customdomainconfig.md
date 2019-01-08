---

copyright:
  years: 2016, 2018
lastupdated:  "2018-11-19"

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock:  .codeblock}

# 为 Mobile Foundation 服务器配置定制域
{: #configcustomdomain}

{{site.data.keyword.mobilefoundation_short}} 创建 {{site.data.keyword.mfserver_short_notm}}，可通过域名基于 {{site.data.keyword.Bluemix_notm}} **区域**的 URL 对其进行访问。您还可以配置自己的定制域。
{: shortdesc}

该 URL 或路径是使用基于 {{site.data.keyword.Bluemix_notm}} `区域`的缺省域名进行创建的。

  |域|区域|    
  |:----- | :----- |    
  |`mybluemix.net` |美国南部|    
  |`eu-gb.mybluemix.net` |英国|
  |`au-syd.mybluemix.net` |悉尼|   
  |`eu-de.mybluemix.net` |法兰克福|   
  {: caption="表 1. 基于 {{site.data.keyword.Bluemix_notm}} 中“区域”的应用程序域名" caption-side="top"}

要能够使用您自己的域，您将需要执行以下步骤来配置定制域：

1.	通过选择其中一个受支持套餐创建 {{site.data.keyword.mobilefoundation_short}} 服务实例来创建 {{site.data.keyword.mfserver_short_notm}} 实例。

+ 将要使用的定制域添加到 {{site.data.keyword.Bluemix_notm}} `组织`。转至**管理组织 > 域 > 添加域**以添加您自己的域。

+ 为服务器设置路径以使用定制域。

+ 转至您的域所对应的 DNS 提供者，添加 CNAME 条目，这会将流量从您的域路由到正在运行服务器的缺省 {{site.data.keyword.Bluemix_notm}} 路径。

+ 如果要为定制域配置 `https`，请将域的 SSL 证书上传到 {{site.data.keyword.Bluemix_notm}} 中。要执行此操作，请转至**管理组织 > 域**，选择要为其配置 SSL 证书的定制域，单击**上传证书**以上传域的 SSL 证书。请参阅[此帖子 ![外部链接图标](../../icons/launch-glyph.svg "外部链接图标")](https://developer.ibm.com/bluemix/2014/09/28/ssl-certificates-bluemix-custom-domains/){: new_window}，以获取更多信息。
