---

copyright:
  years: 2018, 2019
lastupdated: "2018-11-19"

---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:tip: .tip}
{:pre: .pre}
{:codeblock: .codeblock}
{:screen: .screen}
{:note: .note}
{:important: .important}
{:deprecated: .deprecated}
{:javascript: .ph data-hd-programlang='javascript'}
{:java: .ph data-hd-programlang='java'}
{:python: .ph data-hd-programlang='python'}
{:swift: .ph data-hd-programlang='swift'}
{:xml: .ph data-hd-programlang='xml'}

# アダプターの保護
{: #protecting_adapters}

## アダプター・リソースの保護
{: #protecting_adapters_resources}

以下の [Java](#protect-java-adapter-resources) セクションと [JavaScript](#protect-javascript-adapter-resources) セクションで概説されているように、アダプターでは、Java メソッドまたは JavaScript リソース・プロシージャーに対して、あるいは Java リソース・クラス全体に対して保護スコープを指定できます。 スコープは、スペースで区切った 1 つ以上のスコープ・エレメントからなるストリング (「scopeElement1 scopeElement2 …」) として定義することも、ヌルとして定義してデフォルトのスコープを適用することもできます。

デフォルトの MobileFirst スコープは `RegisteredClient` です。これは、リソースにアクセスするためにアクセス・トークンを必要とし、そのリソース要求が MobileFirst Server に登録されたアプリケーションから出されたものであることを検証します。 この保護は、[リソース保護を無効](#disabling-resource-protection)にした場合を除き常に適用されます。 そのため、リソースのスコープを設定しない場合でも、リソースは引き続き保護されます。

>**注**: `RegisteredClient` は、予約済みの MobileFirst キーワードです。 カスタム・スコープ・エレメントやセキュリティー検査をこの名前で定義することはしないでください。
{.note}

### Java アダプター・リソースの保護
{: #protect-java-adapter-resources}

保護スコープを JAX-RS メソッドまたはクラスに割り当てるには、`@OAuthSecurity` アノテーションをこのメソッドまたはクラスの宣言に追加し、このアノテーションの `scope` エレメントを希望のスコープに設定します。 以下の `YOUR_SCOPE` は、1 つ以上のスコープ・エレメントからなるストリング (「scopeElement1 scopeElement2 …」) で置き換えてください。

```java
@OAuthSecurity(scope = "YOUR_SCOPE")
```
{: codeblock}

クラス・スコープは、独自の `@OAuthSecurity` アノテーションが設定されているメソッドを除き、クラス内のすべてのメソッドに適用されます。

>**注**: `@OAuthSecurity` アノテーションの enabled エレメントが `false` に設定されている場合、scope エレメントは無視されます。 [Java リソース保護の無効化](#disabling-java-resource-protection)を参照してください。

**例**

以下のコードは、スコープ・エレメントとして `UserAuthentication` および `Pincode` が含まれているスコープを使用して `helloUser` メソッドを保護します。

```java
@GET
@Path("/{username}")
@OAuthSecurity(scope = "UserAuthentication Pincode")
public String helloUser(@PathParam("username") String name){
    ...
}
```
{: codeblock}

以下のコードは、事前定義の `LtpaBasedSSO` セキュリティー検査を使用して `WebSphereResources` クラスを保護します。

```java
@Path("/users")
@OAuthSecurity(scope = "LtpaBasedSSO")
public class WebSphereResources {
    ...
}
```
{: codeblock}

### JavaScript アダプター・リソースの保護
{: #protect-javascript-adapter-resources}

保護スコープを JavaScript プロシージャーに割り当てるには、**adapter.xml** ファイルで、<procedure> エレメントの scope 属性を希望のスコープに設定します。 以下の `PROCEDURE_NANE` はプロシージャーの名前で、`YOUR SCOPE` は 1 つ以上のスコープ・エレメントからなるストリング (「scopeElement1 scopeElement2 …」) で置き換えてください。

```javascript
<procedure name="PROCEDURE_NANE" scope="YOUR_SCOPE">
```
{: codeblock}

>**注**: <procedure> エレメントの `secured` 属性が false に設定されている場合、`scope` 属性は無視されます。 [JavaScript リソース保護の無効化](#disabling-javascript-resource-protection)を参照してください。
{.note}

**例**

以下のコードは、スコープ・エレメントとして `UserAuthentication` および `Pincode` が含まれているスコープを使用して `userName` プロシージャーを保護します。

```javascript
<procedure name="userName" scope="UserAuthentication Pincode">
```
{: codeblock}

## リソース保護の無効化
{: #disabling-resource-protection}

特定の Java アダプター・リソースまたは JavaScript アダプター・リソースに対して、あるいは Java クラス全体に対して [デフォルトの MobileFirst リソース保護](#protecting_adapters_resources)を無効にすることができます。それは、以下の Java セクションと JavaScript セクションで概説されています。 リソース保護が無効になっている場合、MobileFirst セキュリティー・フレームワークでは、リソースにアクセスするためにトークンは必要ありません。

### Java リソース保護の無効化
{: #disabling-java-resource-protection}

Java リソース・メソッドまたはクラスの OAuth 保護を完全に無効にするには、以下のように、`@OAuthSecurity` アノテーションをこのリソースまたはクラスの宣言に追加し、`enabled` エレメントの値を `false` に設定します。

```java
@OAuthSecurity(enabled = false)
```
{: codeblock}

アノテーションの `enabled` エレメントのデフォルト値は `true` です。 `enabled` エレメントが `false` に設定されている場合、`scope` エレメントは無視され、リソースまたはリソース・クラスは保護されません。

>**注**: 無保護クラスのメソッドにスコープを割り当てた場合、リソースのアノテーションの `enabled` エレメントを `false` に設定しない限り、クラスのアノテーションに関係なくそのメソッドは保護されます。
{.note}

**例**

以下のコードは、`helloUser` メソッドのリソース保護を無効にします。

```java
    @GET
    @Path("/{username}")
    @OAuthSecurity(enabled = "false")
    public String helloUser(@PathParam("username") String name){
        ...
    }
```
{: codeblock}

以下のコードは、`MyUnprotectedResources` クラスのリソース保護を無効にします。

```java
    @Path("/users")
    @OAuthSecurity(enabled = "false")
    public class MyUnprotectedResources {
        ...
    }
```
{: codeblock}

### JavaScript リソース保護の無効化
{: #disabling-javascript-resource-protection}

JavaScript アダプター・リソース (プロシージャー) の OAuth 保護を完全に無効にするには、**adapter.xml** ファイルで、<procedure> エレメントの `secured` 属性を `false` に設定します。

```javascript
<procedure name="procedureName" secured="false">
```
{: codeblock}

`secured` 属性が `false` に設定されている場合、`scope` 属性は無視され、リソースは保護されません。

**例**

以下のコードは、`userName` プロシージャーのリソース保護を無効にします。

```javascript
<procedure name="userName" secured="false">
```
{: codeblock}

## 無保護リソース
{: #unprotected-resources}

無保護リソースは、アクセス・トークンを必要としないリソースです。 MobileFirst セキュリティー・フレームワークは、無保護リソースへのアクセスを管理せず、また該当するリソースにアクセスするクライアントの ID を検証および検査しません。 そのため、無保護リソースでは、ダイレクト・アップデート、デバイス・アクセスのブロック、リモート側でのアプリケーションの無効化などの機能はサポートされません。

