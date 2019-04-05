---

copyright:
  years: 2018, 2019
lastupdated: "2018-11-29"

keywords: device management

subcollection:  mobilefoundation
---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:tip: .tip}
{:note: .note}
{:important: .important}
{:pre: .pre}
{:codeblock: .codeblock}
{:screen: .screen}

# Gerenciar dispositivos
{: #manage_devices}

O acesso ao dispositivo e o estado do dispositivo podem ser gerenciados por meio do Mobile Foundation Operations Console. Um dispositivo é identificado exclusivamente com um identificador chamado ID do dispositivo, designado pelo SDK do cliente do Mobile Foundation. Também é possível configurar um nome de exibição para seu dispositivo. O ID do dispositivo e os campos de nome de exibição do dispositivo são indexados para procura.

Os desenvolvedores de aplicativos podem usar o método `setDeviceDisplayName` da classe `WLClient` para configurar o nome de exibição do dispositivo. Veja [aqui](https://mobilefirstplatform.ibmcloud.com/tutorials/en/foundation/8.0/api/client-side-api/javascript/client/) para a documentação do `WLClient`. Os desenvolvedores de adaptador Java também podem configurar o nome de exibição do dispositivo usando o método `setDeviceDisplayName` da classe `com.ibm.mfp.server.registration.external.model MobileDeviceData`.
{: tip}

O servidor Mobile Foundation mantém as informações de status para cada dispositivo que acessa o servidor.
Os valores de status possíveis são:
* Ativo
* Perdido
* Roubado
* Expirado
* Desativado

O status padrão para um dispositivo é **Ativo**, o que indica que o acesso por meio desse dispositivo não está bloqueado. É possível mudar o status para **Perdido**, **Deturpado** ou **Desativado** para bloquear o acesso a seus recursos de aplicativo por meio do dispositivo. É possível sempre restaurar o status **Ativo** para permitir o acesso novamente.

O status **Expirado** é um status especial que é configurado pelo servidor Mobile Foundation depois que um dispositivo atingiu uma duração de inatividade pré-configurada. Esse status é usado para rastreamento de licença e não afeta os direitos de acesso do dispositivo. Quando um dispositivo com um status **Expirado** se reconecta ao servidor, seu status é restaurado para **Ativo** e o dispositivo tem acesso concedido ao servidor.

## Bloquear dispositivos
{: #block_devices}

1. Acesse a página **Dispositivos** no console do Mobile Foundation Operations.
2. Use o campo **Procurar** para procurar um dispositivo, fornecendo o ID do usuário que está associado ao dispositivo ou fornecendo o nome de exibição do dispositivo (se estiver configurado em um dispositivo).
3. Para cada um dos dispositivos retornados no resultado da procura, é possível ver o ID do dispositivo, o nome de exibição, o modelo de dispositivo, o sistema operacional e a lista de IDs de usuários associados ao dispositivo.
4. A coluna de status do dispositivo mostra o status do dispositivo. É possível mudar o status do dispositivo para **Perdido**, **Deturpado** ou **Desativado** para bloquear o acesso por meio do dispositivo para recursos de aplicativos protegidos.

   Mudar o status de volta para **Ativo** restaura os direitos de acesso originais.
   {: note}


## Cancelar registro de dispositivos
{: #unregister_devices}

1. Acesse a página **Dispositivos** no console do Mobile Foundation Operations.
2. Use o campo **Procurar** para procurar um dispositivo, fornecendo o ID do usuário que está associado ao dispositivo ou fornecendo o nome de exibição do dispositivo (se estiver configurado em um dispositivo).
3. Para cada um dos dispositivos retornados no resultado da procura, é possível ver o ID do dispositivo, o nome de exibição, o modelo de dispositivo, o sistema operacional e a lista de IDs de usuários associados ao dispositivo.
4. Selecione *Cancelar registro* na coluna **Ações**.

   Cancelar o registro de um dispositivo exclui os dados de registro de todos os aplicativos do Mobile Foundation que estão instalados no dispositivo. Além disso, o nome de exibição do dispositivo, a lista de usuários que estão associados ao dispositivo e os atributos públicos que o aplicativo registrou para esse dispositivo são excluídos.
   {: note}


Não **Cancele o registro** de um dispositivo, se você pretende **Bloquear** o dispositivo. Cancelar o registro de um dispositivo remove todos os dados relacionados ao dispositivo. Se um usuário tentar acessar o servidor Mobile Foundation usando o mesmo dispositivo, ele será solicitado a registrar o dispositivo e o registro criará um novo ID do dispositivo para o dispositivo no servidor. Isso significa que o dispositivo recupera o acesso ao servidor Mobile Foundation.
{: important}
