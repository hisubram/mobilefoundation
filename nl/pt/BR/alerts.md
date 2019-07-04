---

copyright:
  years: 2018, 2019
lastupdated: "2019-06-06"

keywords: mobile analytics, set up alerts, alert definitions

subcollection:  mobilefoundation
---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:tip: .tip}
{:note: .note}
{:pre: .pre}
{:codeblock: .codeblock}
{:screen: .screen}

# Configurar alertas
{: #set_up_alerts}

O Mobile Analytics fornece o recurso Alertas, que permite definir alertas para monitorar seus aplicativos. Será possível configurar limites, que, se excedidos, acionarão alertas para notificar o monitor do console do Mobile Analytics. Os alertas acionados podem ser visualizados no console ou podem ser configurados para serem retransmitidos para um webhook customizado para manipulação apropriada.

Esse recurso fornece os meios para detectar e alertar violações de limite observadas em logs de erro de aplicativo, travamentos de aplicativo e transações de rede. Os limites e alertas reativos trazem agilidade em respostas às condições de desempenho do aplicativo sem ter que monitorar continuamente as visualizações/os gráficos correspondentes.

Esse recurso está em BETA.
{: note}

As seções a seguir detalham a criação, o gerenciamento de alertas e a sua visualização no console do Mobile Analytics.

## Criando uma definição de alerta para Logs do aplicativo (Logs do app)
{: #creating_alert_def}

É possível criar uma definição de alerta que é baseada em Logs do app.  Por exemplo, se você desejar monitorar seus logs de aplicativo a cada 5 minutos para verificar se seu aplicativo de uma versão específica registra erros mais de três vezes, então aqui é como você pode configurar isso.

1.  No console do Mobile Analytics, clique em **Definições** para acessar a página de definições de alerta.
2.  Clique em **Criar alerta**.
3.  Forneça os seguintes valores:
    * **Alert Name**: *Alert for app logs*
    * **Message**: *Error Message Alert*
    * **Query Frequency**: *5 minutes*
    * **Event Type**: *App Logs*
        * **Property**: *Log Level*
            * **Value**: *Error*
            * **Limite**: *Total para instância do aplicativo*<br/>
              Se você escolher a opção *Média para aplicativo*, os logs do app terão a média calculada pelo número de dispositivos. Por exemplo, se você tem dois dispositivos e um envia seis logs de app enquanto o outro envia três, a média é de 4,5 logs de app.
              {: note}
            * **Operator**: *is greater than or equals*
            * Threshold value: *3*
4.  Clique em **Avançar** e forneça o valor a seguir:
    * **Método**: *Somente Analytics Console*<br/>
      Além disso, escolha a opção *Analytics Console e autoteste inicial de rede* se você desejar enviar uma mensagem POST com uma carga útil JSON para sua URL customizada. Os campos a seguir estarão disponíveis se você escolher essa opção.
      * **URL para autoteste inicial da rede**
      * **Cabeçalhos**
      * **Tipo de Autenticação**
      * **POST Request Body**
5. Clique em **Salvar**.  

Agora você criou uma definição de alerta para acionar um alerta no término de cada intervalo de 5 minutos quando o número de logs de app tiver atingido o limite de 3 ou mais logs de erro.

Esse alerta permanece ativo, monitorando pela frequência de configuração até que a definição de alerta seja desativada ou excluída.
{: note}

## Criando uma definição de alerta para travamentos de aplicativo
{: #creating_alert_crashes}

A seguir está um exemplo de configuração de alertas em torno de travamentos de aplicativo.  Esse alerta monitora, a cada 2 minutos, todos os travamentos de aplicativo e aciona um alerta se o número de travamentos observados ultrapassa uma contagem de 5.

1.  No console do Mobile Analytics, clique em **Definições** para acessar a página de definições de alerta.
2.  Clique em **Criar alerta**.
3.  Forneça os seguintes valores:
    * **Alert Name**: *Alert for Application Crashes*
    * **Message**: *App Crash Alert*
    * **Query Frequency**: *2 minutes*
    * **Event Type**: *Application Crashes*
        * **Application Name**: *Any application*
        * **Any application**: *Any version*
    * **Threshold**: *Crash Count*
    * **Operator**: *is greater than or equals*
    * Threshold value: *5*
4.  Clique em **Método de distribuição** ou em **Avançar** e forneça o valor a seguir:
    * **Método**: *Somente Analytics Console*<br/>
      Além disso, escolha a opção *Analytics Console e autoteste inicial de rede* se você desejar enviar uma mensagem POST com uma carga útil JSON para sua URL customizada. Os campos a seguir estarão disponíveis se você escolher essa opção.
      * **URL para autoteste inicial da rede**
      * **Cabeçalhos**
      * **Tipo de Autenticação**
      * **POST Request Body**
5. Clique em **Salvar**.  

Esse alerta permanece ativo, monitorando pela frequência de configuração até que a definição de alerta seja desativada ou excluída.
{: note}

## Gerenciando definições de alerta
{: #managing_alert_def}

Você gerencia as suas definições de alerta por meio da página de **Definições** de alerta.

Para cada definição de alerta, é possível executar as operações a seguir,
* Alterne a caixa de seleção sob a coluna **Ativado** para ativar ou desativar uma definição de alerta específica.
* Se você deseja criar uma cópia de uma definição de alerta e mudar alguns valores, clique no ícone duplicado.
* Clique no ícone de lápis, se desejar editar uma definição de alerta.
* Clique no ícone de lixeira, se desejar excluir uma definição de alerta.

## Visualizando alertas
{: #viewing_alerts}

É possível visualizar os alertas, que foram acionados por meio da página **Logs** de alerta.

1.  No console do Mobile Analytics, clique em **Logs**.
2.  Clique no ícone **+** para qualquer um dos alertas. Essa ação exibe a definição de alerta e a seção **Instâncias de alerta**.
    Se a definição de alerta correspondente não for excluída ou modificada, será possível editar a definição de alerta clicando em **Editar alerta**. Caso contrário, o botão **Editar alerta** estará indisponível e a mensagem a seguir será exibida:
    `This alert definition has since been modified or deleted.`
3.  É possível, opcionalmente, selecionar um alerta e clicar no ícone de lixeira para excluir o alerta.
