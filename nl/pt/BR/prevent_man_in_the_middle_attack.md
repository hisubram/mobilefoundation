---

copyright:
  years: 2018, 2019
lastupdated: "2019-06-10"

keywords: mobile foundation security, MIM attack, certificate pinning

subcollection:  mobilefoundation
---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:tip: .tip}
{:pre: .pre}
{:codeblock: .codeblock}
{:screen: .screen}

# Prevenir o ataque Man-in-the-middle
{: #prevent_man_in_the_middle_attack}


Ao se comunicar por redes públicas, é essencial enviar e receber informações de forma segura. O protocolo amplamente usado para proteger essas comunicações é SSL/TLS. O SSL/TLS usa certificados digitais para fornecer autenticação e criptografia. Esses protocolos dependem da verificação da cadeia de certificados e são vulneráveis a vários ataques perigosos, incluindo ataques man-in-the-middle, que ocorrem quando uma parte desautorizada é capaz de visualizar e modificar todo o tráfego que passa entre o dispositivo móvel e os sistemas back-end.
{: shortdesc}

Reduza os Ataques à segurança por meio de [Fixação de certificados](#cert_pinning) e evite ataques man-in-the-middle (MitM).

Esse recurso é opcional e está disponível somente para planos Professional.
{: note}

O processo de fixação de certificado consiste nas etapas a seguir:

1. Execute a  [ Configuração de certificado ](#certsetup).
2. Configure seu código do cliente para chamar o método de [API de Fixação de Certificado](#certpinapi) correto antes de fazer uma solicitação segura.

Durante o handshake SSL (primeira solicitação para o servidor), o SDK do cliente Mobile Foundation verifica se a chave pública do certificado do servidor corresponde à chave pública do certificado armazenado no app.

Deve-se usar um certificado que é comprado de uma autoridade de certificação. Certificados auto-assinados  ** não são suportados **.
{: note}

## Fixação de Certificado
{: #cert_pinning}

Os protocolos que dependem da verificação da cadeia de certificados, como SSL/TLS, são vulneráveis a um número de ataques perigosos, incluindo ataques man-in-the-middle, que ocorrem quando uma parte desautorizada é capaz de visualizar e modificar todo o tráfego que passa entre o dispositivo móvel e os sistemas back-end. Para confiar que um certificado é genuíno e válido, ele é assinado digitalmente por um certificado raiz que pertence a uma autoridade de certificação (CA) confiável. Os sistemas operacionais e os navegadores mantêm listas de certificados raiz de CA confiáveis para que possam verificar facilmente os certificados que as CAs emitiram e assinaram.

O IBM Mobile Foundation fornece uma API para ativar a **fixação de certificado**. Ele é suportado nos aplicativos iOS nativo, Android nativo e Cordova MobileFirst de plataforma cruzada.

## Processo de pinalização de certificado
{: #certpinprocess}

Fixação de certificado é o processo de associação de um host à sua chave pública esperada. É possível configurar seu código de cliente para aceitar apenas um certificado específico para seu nome de domínio, em vez de qualquer certificado que corresponda a um certificado raiz de CA confiável reconhecido pelo sistema operacional ou pelo navegador. Uma cópia do certificado é colocada em seu aplicativo cliente. Durante o handshake SSL (primeira solicitação para o servidor), o SDK do cliente MobileFirst verifica se a chave pública do certificado do servidor corresponde à chave pública do certificado armazenado no app.

Também é possível fixar múltiplos certificados com seu aplicativo cliente. Coloque uma cópia de todos os certificados em seu aplicativo cliente. Durante o handshake SSL (primeira solicitação para o servidor), o SDK do cliente MobileFirst verifica se a chave pública do certificado do servidor corresponde à chave pública de um dos certificados que estão armazenados no app.
{: note}

* ** Importante **
    * Alguns sistemas operacionais móveis podem armazenar em cache o resultado da verificação de validação de certificado. Portanto, seu código chama o método de API de fixação de certificado **antes** de fazer uma solicitação segura. Caso contrário, qualquer solicitação subsequente poderá ignorar a validação de certificado e a verificação de fixação.
    * Certifique-se de usar apenas as APIs do Mobile Foundation para todas as comunicações com o host relacionado, mesmo após a fixação de certificado. O uso de APIs de terceiros para interagir com o mesmo host pode levar a um comportamento inesperado, como o armazenamento em cache de um certificado não verificado pelo sistema operacional móvel.
    * Chamar o método de API de fixação de certificado uma segunda vez substitui a operação de fixação anterior.

Se o processo de fixação for bem-sucedido, a chave pública dentro do certificado fornecido será usada para verificar a integridade do certificado do MobileFirst Server durante o handshake de SSL/TLS de solicitação segura. Se o processo de fixação falhar, todas as solicitações SSL/TLS para o servidor serão rejeitadas pelo aplicativo cliente.

## Configuração do certificado
{: #certsetup}

Deve-se usar um certificado que é comprado de uma autoridade de certificação. Certificados auto-assinados  ** não são suportados **. Para compatibilidade com os ambientes suportados, certifique-se de usar um certificado codificado em **DER** (Regras Distintas de Codificação, conforme definido no formato padrão da União Internacional de Telecomunicações X.690).

O certificado deve ser colocado no MobileFirst Server e em seu aplicativo. Coloque o certificado conforme a seguir,

* No MobileFirst Server (WebSphere Application Server, WebSphere Application Server Liberty ou Apache Tomcat), consulte a documentação para seu servidor de aplicativos específico para obter informações sobre como configurar o SSL/TLS e os certificados.
* Na sua aplicação,
    * Para iOS Nativo, inclua o certificado no **pacote configurável** do aplicativo
    * Para Android Nativo, coloque o certificado na pasta **assets**
    * Para Cordova, coloque o certificado na pasta **app-name\www\certificates** (se a pasta ainda não estiver lá, crie-a)

## API de pinning de certificado
{: #certpinapi}

A fixação de certificado consiste no método de API sobrecarregado a seguir, em que um método tem um parâmetro ``certificateFilename``, em que ``certificateFilename`` é o nome do arquivo de certificado e o segundo método tem um parâmetro ``certificateFilenames``, em que ``certificateFilenames`` é uma matriz de nomes dos arquivos de certificado.

### Android
{: #certpinapiandroid}

* Único certificado:

    Sintaxe: pinTrustedCertificatePublicKeyFromFile (String certificateFilename);

    Por exemplo:

    ```java
    WLClient.getInstance ().pinTrustedCertificatePublicKey ("myCertificate.cer");
    ```

* Múltiplos certificados:

    Sintaxe: pinTrustedCertificatePublicKeyFromFile (String [ ] certificateFilename);

    Por exemplo:

    ```java
    String[] certificates={"myCertificate.cer","myCertificate1.cer"};
    WLClient.getInstance().pinTrustedCertificatePublicKey(certificates);
    ```

O método de fixação de certificado levanta uma exceção em dois casos:

* O arquivo não existe
* O arquivo está no formato errado

### iOS
{: #certpinapiios}

* Fixação de um único certificado

    Sintaxe: pinTrustedCertificatePublicKeyFromFile: (NSString *) certificateFilename;

    O método de fixação de certificado levanta uma exceção em dois casos:

    * O arquivo não existe
    * O arquivo está no formato errado

* Múltiplos certificados pinning

    Sintaxe: pinTrustedCertificatePublicKeyFromFiles: (NSArray *) certificateFilenames;

    O método de fixação de certificado levanta uma exceção em dois casos:

    * Nenhum dos arquivos de certificado existe
    * Nenhum dos arquivos de certificado no formato correto

** No Objective-C **:

Exemplo: certificado único:

```objectc
[ [ WLClient sharedInstance]pinTrustedCertificatePublicKeyFromFile:@ "myCertificate.cer" ];
```

Exemplo: Diversos certificados:

```objectc
NSArray *arrayOfCerts = [NSArray arrayWithObjects:@“Cert1”,@“Cert2”,@“Cert3",nil];
[[WLClient sharedInstance]pinTrustedCertificatePublicKeyFromFiles:arrayOfCerts];
```

** Em Swift **:

Exemplo: certificado único:

```swift
WLClient.sharedInstance ().pinTrustedCertificatePublicKeyFromFile ("myCertificate.cer")
```

Exemplo: Diversos certificados:

```swift
let arrayOfCerts : [Any] = ["Cert1", "Cert2”, "Cert3”];
WLClient.sharedInstance().pinTrustedCertificatePublicKey( fromFiles: arrayOfCerts)
```

O método de fixação de certificado criará uma exceção em dois casos:

* O arquivo não existe
* O arquivo está no formato errado

### Cordova
{: #certpinapicordova}

* Single certificate pinning:

    ```javascript
    WL.Client.pinTrustedCertificatePublicKey (' myCertificate.cer') .then (onSuccess, onFailure);
    ```

* Múltiplos certificados pinning:

    ```javascript
    WL.Client.pinTrustedCertificatePublicKey ([ 'Cert1.cer', 'Cert2.cer', ' Cert3.cer' ]) .then (onSuccess, onFailure);
    ```

O método de fixação de certificado retorna uma promessa:

* O método de fixação de certificado chama o método `onSuccess` se a fixação é bem-sucedida.
* O método de fixação de certificado aciona o retorno de chamada `onFailure` nos dois casos a seguir,
  * O arquivo não existe.
  * O arquivo está no formato errado.

Posteriormente, se uma solicitação segura for feita para um servidor cujo certificado não estiver fixado, o retorno de chamada `onFailure` da solicitação específica (por exemplo, `obtainAccessToken` ou `WLResourceRequest`) será chamado.

Saiba mais sobre o método de API de fixação de certificado na [Referência de API](/docs/services/mobilefoundation?topic=mobilefoundation-client_sdks#client_sdks).
{: note}
