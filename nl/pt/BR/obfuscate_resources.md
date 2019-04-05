---

copyright:
  years: 2018, 2019
lastupdated: "2019-03-26"

keywords: obfuscating resources, security

subcollection:  mobilefoundation
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

# Obfusciar recursos
{: #obfuscate_resources}

Ofuscação é o processo de modificação de um executável para que ele não seja mais útil para um hacker, mas permanece totalmente funcional. A ofuscação de código automatizado torna a engenharia reversa um programa difícil.

Por ofuscação, um aplicativo torna-se muito mais difícil de realizar engenharia reversa, protegido contra roubo de segredo comercial (propriedade intelectual), acesso não autorizado, ignorando o licenciamento ou outros controles e vulnerabilidade.

A ofuscação de código consiste em várias técnicas e técnicas de segurança de aplicativo diferentes:

* Renomear ofuscação - a renomeação altera o nome de métodos e variáveis e torna a origem descompilada mais difícil para o ser humano entender, mas não altera a execução do aplicativo. O método mais preferencial de ofuscação por ofuscadores .NET, iOS, Java e Android.
* Criptografia de Sequência
* Obfuscação do Fluxo de Controle
* Transformação de Padrão de Instrução
* Inserção de Código Dummy
* Anti-Tamper, e assim por diante.

A ofuscação torna muito mais difícil para os invasores revisar o código e analisar o aplicativo. Para ofuscação, várias ferramentas de terceiros estão disponíveis.

* Para a ofuscação de um aplicativo Android, consulte este [blog](https://mobilefirstplatform.ibmcloud.com/blog/2016/09/19/mfp-80-obfuscating-android-code-with-proguard/).

  É necessário criar o arquivo de configuração (proguard-project.txt) para a ofuscação do Android ProGuard com um aplicativo MobileFirst Android.
  {: note}

* Para obter a ofuscação básica do aplicativo Cordova, consulte [Criptografando o aplicativo Cordova](#encryptingcordovapackage).

## Criptografando os recursos da web de seus pacotes Cordova
{: #encryptingcordovapackage}

Para minimizar o risco de visualização e modificação de seus recursos da web por alguém, embora esteja no pacote `.apk` ou `.ipa`, é possível usar o comando da CLI do MobileFirst a seguir:
```bash
mfpdev app webencrypt
```
{: codeblock}
ou o sinalizador `mfpwebencrypt` para criptografar as informações. Esse procedimento não fornece criptografia que é impossível de derrotar, mas fornece um nível básico de ofuscação.

### Pré-requisitos

* Deve-se instalar as ferramentas de desenvolvimento Cordova. Este exemplo usa a CLI do Apache Cordova. Se você usar outras ferramentas de desenvolvimento Cordova, algumas das etapas serão diferentes. Consulte a documentação da ferramenta Cordova para obter instruções.
* Você deve instalar a CLI do MobileFirst.
* Deve-se instalar o plug-in do MobileFirst Cordova.

O melhor momento para concluir esse procedimento será depois que você concluir seu desenvolvimento de app e estiver pronto para implementar o app. Se você executar qualquer um dos comandos a seguir depois de concluir o procedimento de criptografia de recursos da web, o conteúdo que foi criptografado se tornará decriptografado.

* `cordova prepare`
* `cordova build`
* `cordova run`
* `cordova emulate`
* `mfpdev app webupdate`
* `mfpdev app preview`

Caso você execute um dos comandos listados depois de criptografar os recursos da web, deve-se concluir esse procedimento novamente para criptografar os recursos da web.

1. Abra uma janela do terminal e navegue para o diretório raiz do app Cordova que você deseja criptografar.
2. Prepare o app inserindo um dos comandos a seguir:
    * ```bash
      cordova prepare
      ```
      {: codeblock}
    * ```bash
      mfpdev app webupdate
      ```
      {: codeblock}
3. Conclua um dos procedimentos a seguir para criptografar o conteúdo,
    * Insira o seguinte comando:
      ```bash
      mfpdev app webencrypt
      ```
      {: codeblock}

      É possível visualizar informações sobre o comando `mfpdev app webencrypt` inserindo
      ```bash
      mfpdev help app webencrypt
      ```
      {: codeblock}
      {: tip}

    * Também é possível criptografar os recursos da web de seus pacotes Cordova incluindo a sinalização `mfpwebencrypt` no comando `cordova compile` ou `cordova build` ao construir seus pacotes.
       * ```bash
         cordova compile -- --mfpwebencrypt
         ```
         {: codeblock}
         ou
         ```bash
         cordova build -- --mfpwebencrypt
         ```
         {: codeblock}
         As informações do sistema operacional na pasta **www** são substituídas por um arquivo **resources.zip** que contém o conteúdo criptografado.
         Se o seu app for para o sistema operacional Android e o arquivo **resources.zip** for maior que 1 MB, o arquivo **resources.zip** será dividido em arquivos .zip menores de 768 KB denominados **resources.zip.nnn**. A variável nnn é um número de 001 a 999.
4. Teste o aplicativo com os recursos criptografados usando o emulador fornecido com as ferramentas específicas da plataforma. Por exemplo, é possível usar o emulador no Android Studio for Android ou no Xcode for iOS.

Não use os comandos Cordova a seguir para testar o aplicativo depois de criptografá-lo,
* ```bash
  cordova run
  ```
  {: codeblock}
* ```bash
  cordova emulate
  ```
  {: codeblock}
Esses comandos atualizam o conteúdo que foi criptografado na pasta www e salva-o novamente como conteúdo decriptografado. Se você usar esses comandos, lembre-se de concluir o procedimento novamente para criptografá-lo antes de publicar o app.
{: note}
