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

# Evitar apps corrompidos acessando backend
{: #prevent_tampered_apps_accessing_backend}

A autenticidade do aplicativo ajuda a verificar a validade do aplicativo antes de ativar quaisquer serviços, evitando assim que o aplicativo violado acesse os serviços de back-end.
{: shortdesc}

Para proteger corretamente seu aplicativo, ative a verificação de segurança de autenticidade de aplicativo predefinida do MobileFirst (``appAuthenticity``). Quando ativada, essa verificação valida a autenticidade do aplicativo antes de fornecê-la com quaisquer serviços. Os aplicativos no ambiente de produção devem ter esse recurso ativado.

Para ativar a autenticidade do aplicativo, é possível seguir as instruções na tela no **MobileFirst Operations Console → [seu aplicativo] → Autenticidade** ou revisar as informações abaixo.

* ** Disponibilidade **

    A autenticidade do aplicativo está disponível em todas as plataformas suportadas (iOS, Android, Windows 8.1 Universal, Windows 10 UWP) nos aplicativos Cordova e nativo.

* ** Limitações **

    A autenticidade do aplicativo não suporta **Bitcode** no iOS. Portanto, antes de usar a autenticidade do aplicativo, desative o Bitcode nas propriedades do projeto Xcode.

## Fluxo de autenticidade do aplicativo
{: #appauthenticityflow}

A verificação de segurança de autenticidade do aplicativo é executada durante o registro do aplicativo para o MobileFirst Server, que ocorre na primeira vez em que uma instância do aplicativo tenta se conectar ao servidor. Por padrão, a verificação de autenticidade não é executada novamente.

Depois que a autenticidade do app for ativada e se o cliente precisar introduzir quaisquer mudanças em seu aplicativo, a versão do aplicativo precisará ser submetida a upgrade.

Consulte [Configurando a autenticidade do aplicativo](#configappauthenticity) para saber como customizar esse comportamento.

### Ativando a Autenticidade do Aplicativo
{: #enableappauthenticity}

Para que a autenticidade do aplicativo seja ativada em seu aplicativo:

1. Abra o MobileFirst Operations Console em seu navegador favorito.
2. Selecione seu aplicativo na barra lateral de navegação e clique no item de menu **Autenticidade**.
3. Alterne o botão **Ligar/Desligar** na caixa **Status**.

![Enabling Application Authencity](/images/enable_application_authenticity.png)

O MobileFirst Server valida a autenticidade do aplicativo na primeira tentativa de conexão com o servidor. Para aplicar essa validação também aos recursos protegidos, inclua a verificação de segurança appAuthenticity no escopo de proteção.

### Desativando a Autenticidade do Aplicativo
{: #disableappauthenticity}

Algumas mudanças no aplicativo durante o desenvolvimento podem fazer com que ele falhe na validação de autenticidade. Portanto, é recomendável desativar a autenticidade do aplicativo durante o processo de desenvolvimento. Os aplicativos no ambiente de produção devem ter esse recurso ativado.

Para desativar a autenticidade do aplicativo, alterne de volta o botão **Ligar/Desligar** na caixa **Status**.

## Configurando a Autenticidade do Aplicativo
{: #configappauthenticity}

Por padrão, a Autenticidade do aplicativo é verificada somente durante o registro do cliente. No entanto, assim como qualquer outra verificação de segurança, é possível decidir proteger seu aplicativo ou recursos com a verificação de segurança ``appAuthenticity`` por meio do console, seguindo as instruções em Protegendo recursos.

É possível configurar a verificação de segurança de autenticidade de aplicativo predefinida com a propriedade a seguir:

* ``expirationSec``: é padronizado com 3600 segundos/1 hora. Define a duração até que o token de autenticidade expire.

Depois que uma verificação de autenticidade é concluída, ela não ocorre novamente até que o token tenha expirado com base no valor configurado.

Para configurar a propriedade `` expirationSec `` :

1. Carregue o MobileFirst Operations Console, navegue para **[seu aplicativo] → Segurança → Configurações de verificação de segurança** e clique em **Novo**.
2. Procure o elemento de escopo `` appAuthenticity `` .
3. Configure um novo valor em segundos.

    ![Configuring expiration in number of seconds](/images/configuring_expirationSec.png)

## Segredo do tempo de construção (BTS)
{: #buildtimesecret}

O Build Time Secret (BTS) é uma **ferramenta opcional para aprimorar a validação de autenticidade**, somente para aplicativos iOS. A ferramenta injeta o aplicativo com um segredo determinado no momento da criação, que é posteriormente usado no processo de validação de autenticidade.

A ferramenta BTS pode ser transferida por download por meio do **MobileFirst Operations Console → Centro de download**.

Para usar a ferramenta BTS no Xcode:

1. Na guia **Fases de construção**, clique no botão **+** e crie uma nova **Fase de execução do script**.
2. Copie o caminho da Ferramenta BTS e cole na nova "Fase de execução do script" que você criou.
3. Arraste a fase de execução do script acima da **Fase de compilação de origens**.

A ferramenta deve ser usada ao construir uma versão de produção do aplicativo.

## Resolução de problemas de autenticidade do aplicativo
{: #troubleshooting-app-authenticity}

### Reconfigurar
{: #trblreset}

O algoritmo de autenticidade do aplicativo usa dados e metadados do aplicativo em sua validação. O primeiro dispositivo a se conectar ao servidor após a ativação da autenticidade do aplicativo fornece uma “impressão digital” do aplicativo, contendo alguns desses dados.

É possível reconfigurar essa impressão digital, fornecendo o algoritmo com novos dados. Isso pode ser útil durante o desenvolvimento (por exemplo, depois de mudar o aplicativo em Xcode). Para reconfigurar a impressão digital, use o comando **reset** por meio da CLI mfpadm.

Depois de reconfigurar a impressão digital, a verificação de segurança appAuthenticity continua a funcionar como antes (isso será transparente para o usuário).

### Tipos de Validação
{: #trblvalidationtypes}

O Mobile First Platform Foundation fornece autenticidade de app estática e dinâmica para aplicativos. Esses tipos de validação diferem no algoritmo e atributos usados para gerar valores iniciais de autenticidade do app. Por padrão, quando a autenticidade do aplicativo é ativada, ela usa o algoritmo de validação dinâmica. Ambos os tipos de validação garantem a segurança do aplicativo. A autenticidade do app dinâmico usa validações estritas e verifica a autenticidade do app. Para a autenticidade do app estático, usaremos um algoritmo um pouco mais flexível, que não usará todas as verificações de validação usadas na autenticidade do app dinâmico.

A autenticidade do app dinâmico é configurável por meio do MobileFirst Console. O algoritmo interno cuida da geração de dados de autenticidade do app com base nas opções escolhidas no console. Para autenticidade do app estático, é necessário usar a CLI mfpadm.

Para ativar a autenticidade do app estático e para alternar entre os tipos de validação, use a CLI mfpadm:

```bash
mfpadm --url=  --user=  --passwordfile= --secure=false app version [RUNTIME] [APPNAME] [ENVIRONMENT] [VERSION] set authenticity-validation TYPE
```
{: codeblock}

TYPE pode ser dinâmico ou estático.
