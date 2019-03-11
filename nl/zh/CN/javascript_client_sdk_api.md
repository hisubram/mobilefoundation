---

copyright:
  years: 2019
lastupdated: "2018-12-21"

---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:tip: .tip}
{:note: .note}
{:pre: .pre}
{:codeblock: .codeblock}
{:screen: .screen}
{:java: .ph data-hd-programlang='java'}
{:ruby: .ph data-hd-programlang='ruby'}
{:c#: .ph data-hd-programlang='c#'}
{:objectc: .ph data-hd-programlang='Objective C'}
{:python: .ph data-hd-programlang='python'}
{:javascript: .ph data-hd-programlang='javascript'}
{:php: .ph data-hd-programlang='PHP'}
{:swift: .ph data-hd-programlang='swift'}

# Javascript 客户机 SDK API
{: #javascript_client_sdk_api}

## 用于 Cordova / Web 应用程序的 API (Javascript)

下表列出了可以在 JavaScript 应用程序中执行的函数以及相应的 API 方法。

|函数| 描述|
|----------|-------------|
|`WL.Client`，`WL.App`|初始化和重新装入应用程序，使应用程序文本全球化| 
|`WLAuthorizationManager`|获取客户机标识和授权头|
|`WL.Logger`|将日志消息打印到环境的日志中|
|`WL.NativePage`|将当前显示的基于 Web 的屏幕切换至本机编写的页面|
|`WLResourceRequest`|向受保护和不受保护的资源发送请求| 
|`WL.JSONStore`|提供面向文档的轻量级存储系统的客户机端 API| 

## 其他信息
{: #additional-information }
### options 对象
{: #the-optional-object }
`options` 对象包含所有方法的公共属性。此对象用于所有对 {{ site.data.keys.mf_server }} 的异步调用。

有时，会通过仅适用于特定方法的属性对此对象进行扩充。这些额外的属性在特定方法的描述中进行详细说明。

options 对象的公共属性如下所示：

```javascript
options = {
    onSuccess: successHandler(response),
    onFailure: failureHandlder(response),
    invocationContext: invocation-context
};
```
{: code}
每个属性的含义如下所示：

|属性| 描述|
|----------|-------------|
| `onSuccess` |可选。成功完成异步调用时会调用此函数。`onSuccess` 函数的语法为：`success-handler-function(response)`，其中 `response` 是至少包含以下属性的对象：{::nomarkdown}<ul><li><b>invocationContext</b> - 在 <code>options</code> 对象中初始传递到 {{ site.data.keys.mf_server }} 的 <code>invocationContext</code> 对象，或者如果未传递 <code>invocationContext</code> 对象，那么为 <code>undefined</code>。</li><li><b>status</b> - HTTP 响应状态</li></ul>{:/}**注：**对于 `response` 对象包含其额外属性的方法，这些属性在特定方法的描述中进行详细说明。|
| `onFailure` |可选。异步调用失败时要调用的函数。此类失败包括在异步调用期间发生的服务器端错误和客户机端错误，例如服务器连接失败或超时的调用。**注：**对于通过抛出异常来停止执行的客户机端错误，不会调用此函数。onFailure 函数的语法为：`failure-handler-function(response)`，其中 `response` 是包含以下属性的对象：{::nomarkdown}<ul><li><b>invocationContext</b> - 在 <code>options</code> 对象中初始传递到 {{ site.data.keys.mf_server }} 的 <code>invocationContext</code> 对象，或者如果未传递 <code>invocationContext</code> 对象，那么为 <code>undefined</code>。</li><li><b>errorCode</b> - 错误代码字符串。可以返回的所有错误代码在 <b>worklight.js</b> 文件中定义为 <code>WL.ErrorCode</code> 对象中的常量。</li><li><b>errorMsg</b> - {{ site.data.keys.mf_server }} 提供的错误消息。此消息仅供开发者使用，不应该向用户显示。此消息不会翻译为用户的语言。</li><li><b>status</b> - HTTP 响应状态</li></li></ul>{:/}**注：**对于 `response` 对象包含其额外属性的方法，这些属性在特定方法的描述中进行详细说明。|
| `invocationContext` |可选。返回到成功和失败处理程序的对象。`invocationContext` 对象用于在从服务返回时保留调用异步服务的上下文。例如，可以使用相同的成功处理程序依次调用 `invokeProcedure` 方法。成功处理程序需要能够确定正在处理对 invokeProcedure 的哪个调用。一个解决方案是将 `invocationContext` 对象作为整数实现，并针对 `invokeProcedure` 的每个调用，以 1 为增量递增其值。调用成功处理程序时，{{ site.data.keys.product_adj }} 框架会将与 `invokeProcedure` 方法关联的 options 对象的 `invocationContext` 对象传递给该处理程序。`invocationContext` 对象的值可用于确定与所处理结果关联的对 `invokeProcedure` 的调用。| 

## WL.ClientMessages 对象
{: #the-wlclientmessages-object }
您可以查看 `WL.ClientMessages` 对象中存储的系统消息的列表，并启用这些系统消息的翻译。

必须在全局 JavaScript 级别覆盖系统消息，这是因为代码的某些部分仅在应用程序成功初始化后才会运行。
{: note}

`WL.ClientMessages` 对象用于存储 **worklight/messages/messages.json** 文件中定义的系统消息。此文件位于使用 {{ site.data.keys.product }} 生成的项目的环境文件夹中。要启用系统消息的翻译，必须在 `WL.ClientMessages` 对象中覆盖此消息的值，如以下代码示例所示：

```javascript
WL.ClientMessages.invalidUsernamePassword="The custom user name and password are not valid";
```
{: code}


* [JavaScript API Reference](http://mobilefirstplatform.ibmcloud.com/tutorials/en/foundation/8.0/api/client-side-api/javascript/client/#javascript-api-reference)
* [Objective-C API reference (for Cordova)](http://mobilefirstplatform.ibmcloud.com/tutorials/en/foundation/8.0/api/client-side-api/javascript/client/#objective-c-api-reference-for-cordova)
{: note}
