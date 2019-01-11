---

copyright:
  years: 2018
lastupdated:  "2018-11-19"

---

{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:tip: .tip}
{:pre: .pre}

#	Utilitários de segurança
{: #security_utilities}

## Visão geral
A API do lado do cliente do Mobile Foundation fornece alguns utilitários de segurança para ajudar a proteger os dados de seu usuário. Recursos como o JSONStore são ótimos se você deseja proteger objetos JSON. No entanto, não é sugerido armazenar blobs binários em uma coleção JSONStore.

Em vez disso, armazene dados binários no sistema de arquivos e armazene os caminhos de arquivo e outros metadados dentro de uma coleção JSONStore. Se desejar proteger arquivos como imagens, será possível codificá-los como sequências base64, criptografá-los e gravar a saída no disco. Para decriptografar os dados, é possível consultar os metadados em uma coleção do JSONStore. Leia os dados criptografados no disco e decriptografe-os usando os metadados que foram armazenados. Esses metadados podem incluir a chave, o salt, o Vetor de inicialização (IV), o tipo de arquivo, o caminho para o arquivo e outros.

Em um alto nível, a API SecurityUtils fornece as APIs a seguir:

* Geração de chave - em vez de passar uma senha diretamente para a função de criptografia, essa função de geração de chave usa Password-Based Key Derivation Function v2 (PBKDF2) para gerar uma chave forte de 256 bits para a API de criptografia. Ela toma um parâmetro para o número de iterações. Quanto maior o número, mais tempo leva para um invasor forçar a chave. Use um valor de pelo menos 10.000. O salt deve ser exclusivo e ajuda a assegurar que os invasores tenham mais dificuldade em usar as informações de hash existentes para atacar a sua senha. Use um comprimento de 32 bytes.
* Criptografia - a entrada é criptografada usando o Padrão de Criptografia Avançado (AES). A API toma uma chave que é gerada com a API de geração de chave. Internamente, ela gera um IV seguro, que é usado para incluir a aleatorização para a primeira cifra de bloco. O texto é criptografado. Se você deseja criptografar uma imagem ou outro formato binário, transforme seu binário em texto base64 usando essas APIs. Essa função de criptografia retorna um objeto com as partes a seguir:
    * ct (texto cifrado, que também é chamado de texto criptografado)
    * IV
    * v (versão, que permite que a API se desenvolva enquanto ainda está sendo compatível com uma versão anterior)
* Decriptografia - toma a saída da API de criptografia como entrada e decriptografa o texto cifrado ou criptografado em texto sem formatação.
* Sequência aleatória remota - obtém uma sequência hexadecimal aleatória entrando em contato com um gerador aleatório no MobileFirst Server. O valor padrão é 20 bytes, mas é possível mudar o número até 64 bytes.
* Sequência aleatória local - obtém uma sequência hexadecimal aleatória gerando uma localmente, diferentemente da API de sequência aleatória remota, que requer acesso à rede. O valor padrão é 32 bytes e não há valor máximo. O tempo de operação é proporcional ao número de bytes.
* Codificar base64 - toma uma sequência e aplica a codificação base64. Incorrer em uma codificação base64 pela natureza do algoritmo significa que o tamanho dos dados é aumentado por aproximadamente 1,37 vezes o tamanho original.
* Decodificar base64 - toma uma sequência codificada em base64 e aplica a decodificação base64.

## Instalar
Assegure-se de importar os arquivos a seguir para usar as APIs de utilitários de segurança do JSONStore.

### iOS

```objc
#import "WLSecurityUtils.h"
```

### Android

```java
import com.worklight.wlclient.api.SecurityUtils
```

### JavaScript
Nenhuma configuração é necessária.

## Exemplos
### iOS
#### Criptografia e decriptografia

```objc
// User provided password, hardcoded only for simplicity.
NSString* password = @"HelloPassword";

// Random salt with recommended length.
NSString* salt = [WLSecurityUtils generateRandomStringWithBytes:32];

// Recomended number of iterations.
int iterations = 10000;

// Populated with an error if one occurs.
NSError* error = nil;

// Call that generates the key.
NSString* key = [WLSecurityUtils generateKeyWithPassword:password
                                 andSalt:salt
                                 andIterations:iterations
                                 error:&error];

// Text that is encrypted.
NSString* originalString = @"My secret text";
NSDictionary* dict = [WLSecurityUtils encryptText:originalString
                                      withKey:key
                                      error:&error];

// Should return: 'My secret text'.
NSString* decryptedString = [WLSecurityUtils decryptWithKey:key
                                             andDictionary:dict
                                             error:&error];
```
{: codeblock}

#### Codificar e decodificar base64

```objc
// Input string.
NSString* originalString = @"Hello world!";

// Encode to base64.
NSData* originalStringData = [originalString dataUsingEncoding:NSUTF8StringEncoding];
NSString* encodedString = [WLSecurityUtils base64StringFromData:originalStringData length:originalString.length];

// Should return: 'Hello world!'.
NSString* decodedString = [[NSString alloc] initWithData:[WLSecurityUtils base64DataFromString:encodedString] encoding:NSUTF8StringEncoding];
```
{: codeblock}

#### Obter aleatório remoto

```objc
[WLSecurityUtils getRandomStringFromServerWithBytes:32
                 timeout:1000
                 completionHandler:^(NSURLResponse *response, NSData *data, NSError *connectionError) {

  // You might want to see the response and the connection error before moving forward.

  // Get the secure random string.
  NSString* secureRandom = [[NSString alloc] initWithData:data encoding:NSUTF8StringEncoding];
}];
```
{: codeblock}

### Android
#### Criptografia e decriptografia

```java
String password = "HelloPassword";
String salt = SecurityUtils.getRandomString(32);
int iterations = 10000;

String key = SecurityUtils.generateKey(password, salt, iterations);

String originalText = "Hello World!";

JSONObject encryptedObject = SecurityUtils.encrypt(key, originalText);

// Deciphered text will be the same as the original text.
String decipheredText = SecurityUtils.decrypt(key, encryptedObject);
```
{: codeblock}

#### Codificar e decodificar base64

```java
import android.util.Base64;

String originalText = "Hello World";
byte[] base64Encoded = Base64.encode(text.getBytes("UTF-8"), Base64.DEFAULT);

String encodedText = new String(base64Encoded, "UTF-8");

byte[] base64Decoded = Base64.decode(text.getBytes("UTF-8"), Base64.DEFAULT);

// Decoded text will be the same as the original text.
String decodedText = new String(base64Decoded, "UTF-8");
```
{: codeblock}

#### Obter aleatório remoto

```java
Context context; // This is the current Activity's context.
int byteLength = 32;

// Listener calls the callback functions after it gets the response from the server.
WLRequestListener listener = new WLRequestListener(){
  @Override
  public void onSuccess(WLResponse wlResponse) {
    // Implement the success handler.
  }

  @Override public void onFailure(WLFailResponse wlFailResponse) {
    // Implement the failure handler.
    }
};

SecurityUtils.getRandomStringFromServer(byteLength, context, listener);
```
{: codeblock}

#### Obter aleatório local

```java
int byteLength = 32;
String randomString = SecurityUtils.getRandomString(byteLength);
```
{: codeblock}

### JavaScript
#### Criptografia e decriptografia

```javascript
// Keep the key in a variable so that it can be passed to the encrypt and decrypt API.
var key;

// Generate a key.
WL.SecurityUtils.keygen({
  password: 'HelloPassword',
  salt: Math.random().toString(),
  iterations: 10000
})

.then(function (res) {

  // Update the key variable.
  key = res;

  // Encrypt text.
  return WL.SecurityUtils.encrypt({
    key: key,
    text: 'My secret text'
  });
})

.then(function (res) {

  // Append the key to the result object from encrypt.
  res.key = key;

  // Decrypt.
  return WL.SecurityUtils.decrypt(res);
})

.then(function (res) {

  // Remove the key from memory.
  key = null;

  //res => 'My secret text'
})

.fail(function (err) {
  // Handle failure in any of the previously called APIs.
});
```
{: codeblock}

#### Codificar e decodificar base64

```javascript
WL.SecurityUtils.base64Encode('Hello World!')
.then(function (res) {
  return WL.SecurityUtils.base64Decode(res);
})
.then(function (res) {
  //res => 'Hello World!'
})
.fail(function (err) {
  // Handle failure.
});
```
{: codeblock}

#### Obter aleatório remoto

```javascript
WL.SecurityUtils.remoteRandomString(32)
.then(function (res) {
  // res => deba58e9601d24380dce7dda85534c43f0b52c342ceb860390e15a638baecc7b
})
.fail(function (err) {
  // Handle failure.
});
```
{: codeblock}

#### Obter aleatório local

```javascript
WL.SecurityUtils.localRandomString(32)
.then(function (res) {
  // res => 40617812588cf3ddc1d1ad0320a907a7b62ec0abee0cc8c0dc2de0e24392843c
})
.fail(function (err) {
  // Handle failure.
});
```
{: codeblock}
