---

copyright:
  years: 2018
lastupdated: "2018-11-22"

---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:tip: .tip}
{:note: .note}
{:pre: .pre}
{:codeblock: .codeblock}
{:screen: .screen}

# Visualizar insights no console
{: #visualize_insights_on_console}

No MobileFirst Analytics Console, é possível visualizar e configurar os relatórios do Analytics, gerenciar alertas e visualizar logs do cliente. Ative o Console de analítica por meio do Mobile Foundation Operations Console, clicando no **Console de analítica** na navegação esquerda.

O Console de analítica é ativado e o painel padrão aparece. Se um aplicativo cliente já enviou logs e dados de analítica para o servidor, os relatórios relevantes serão criados e exibidos. No painel, é possível revisar os dados de analítica que são coletados. Esses dados de analítica podem estar relacionados a travamentos do aplicativo, sessões do aplicativo e tempo de processamento do servidor. Além disso, é possível criar gráficos customizados e gerenciar alertas.

Além de uma visualização de visão rápida de sua analítica móvel, o recurso de analítica inclui a capacidade de executar uma procura bruta com relação aos logs do cliente, dados de travamento do cliente capturados e quaisquer dados extras que você fornecer explicitamente por meio de chamadas de função da API do cliente que se alimentam no Mobile Analytics.

## Monitorando dados do aplicativo
{: #monitoring_app_data}

O Mobile Analytics fornece monitoramento e analítica para seus aplicativos móveis. É possível registrar logs de aplicativo e monitorar dados com o SDK do Mobile Analytics Client. Os desenvolvedores podem controlar quando enviar esses dados para o Mobile Analytics Service. Quando os dados são entregues para o Mobile Analytics, é possível usar o console do Mobile Analytics para obter insights analíticos sobre seus aplicativos móveis, dispositivos e logs de aplicativo.

Alguns dos insights que podem ser derivados:

**Métricas predefinidas**

Com as métricas predefinidas, é possível responder perguntas como:
* Quantos novos usuários eu tenho?
* Quantas pessoas estão usando ativamente meu aplicativo?
* Com que frequência as pessoas estão usando meu aplicativo?
* Que hora do dia as pessoas estão usando meu aplicativo?
* Quais modelos de dispositivo meus usuários preferem?
* Quando deverei descontinuar o suporte para sistemas operacionais legados?
* Quais aplicativos estão tendo problemas de desempenho?

**Eventos customizados**

Incluindo seus próprios eventos customizados, é possível responder perguntas como:
* Quais recursos são mais e menos usados?
* Onde os usuários estão entrando e saindo do meu app?
* Quais atividades os usuários estão visualizando mais?
* Os usuários estão concluindo fluxos de trabalho no app (por exemplo, funis de conversão)?

## Relatórios que podem ser visualizados: usuários
{: #reports_visualized_users}

Esse relatório exibe um gráfico mostrando o número de usuários ativos que usaram o app dentro de um intervalo de data especificado. O relatório também mostra o número de novos usuários que são usuários exclusivos que estão usando o app pela primeira vez para o intervalo de data especificado.
Os gráficos podem ser filtrados pelo nome do app, pelo sistema operacional ou pelas versões do sistema operacional.

## Relatórios que podem ser visualizados: sessões
{: #reports_visualized_sessions}

Esse relatório exibe um gráfico mostrando Sessões de app para o intervalo de data especificado. Uma sessão é registrada quando um app é trazido para o primeiro plano de um dispositivo. Os gráficos podem ser filtrados pelo nome do app, pelo sistema operacional ou pelas versões do sistema operacional.

## Relatórios que podem ser visualizados: solicitações de rede
{: #reports_visualized_network_requests}

Visualize os dados da solicitação de rede para seus aplicativos no console do Mobile Analytics.

Os dados estão disponíveis para as medidas a seguir:

**Tempo de roundtrip** - define o período de tempo, medido em milissegundos, que leva para que seu app faça solicitações de rede.
**Contagem de solicitações** - exibe com que frequência um app faz solicitações de rede. Os dados também são exibidos como uma média.
Os gráficos podem ser filtrados pelo nome do app, pelo sistema operacional ou pelas versões do sistema operacional.

## Relatórios que podem ser visualizados: travamentos
{: #reports_visualized_crashes}

É possível visualizar informações sobre seus travamentos de aplicativo no console do Mobile Analytics para monitorar melhor e solucionar problemas de seus aplicativos.

Na página Travamentos, a tabela **Visão geral de travamento** mostra as colunas de dados a seguir:

**Aplicativo**: nome do aplicativo<br/>
**Travamentos**: o número total de travamentos para esse app<br/>
**Total de usos**: o número total de vezes que um usuário abre e fecha esse app<br/>
**Taxa de travamento**: porcentagem de travamentos por uso<br/>
É possível ver rapidamente as informações sobre seus travamentos de aplicativo na tabela Travamentos. O gráfico de barras Travamentos mostra um histograma de travamentos ao longo do tempo.<br/>

É possível exibir dados de travamento de duas maneiras:

1.  Exibir taxa de travamento: taxa de travamento ao longo do tempo
2.  Exibir total de travamentos: total de travamentos ao longo do tempo

## Relatórios que podem ser visualizados: resolução de problemas
{: #reports_visualized_troubleshooting}

A página **Resolução de problemas** no console do Mobile Analytics oferece uma visualização granular de seus travamentos de app, usando a tabela **Resumo de travamentos**.

A tabela **Resumo de travamento** é classificável e inclui as colunas de dados a seguir:

* Travamentos
* Dispositivos
* Último travamento
* Aplicativo
* OS
* Mensagem

Clique no ícone + próximo a qualquer entrada para exibir a
tabela
**Detalhes do travamento**, que inclui as
colunas a seguir:

* Horário do travamento
* Versão de Aplicativo
* Versão do Sistema Operacional
* Modelo de dispositivo
* ID do dispositivo
* Download: link para fazer download dos logs que levaram ao travamento

Expanda qualquer entrada na tabela **Detalhes do travamento** para obter mais detalhes, incluindo um rastreio de pilha.

Os dados para a tabela **Resumo de travamento** são preenchidos consultando os logs do app de nível fatal. Se seu aplicativo não coletar logs de aplicativo fatais, nenhum dado estará disponível.
{: note}


## Relatórios que podem ser visualizados: feedback do usuário
{: #reports_visualized_userfeedback}

O Feedback do usuário fornece análise de feedback no app usando o Mobile Analytics.
Com esse recurso do Mobile Analytics -
* Os **Usuários e testadores** podem registrar e enviar feedback e relatórios de erros por meio do aplicativo, à medida que eles executam e usam o aplicativo.
* Os **Proprietários do app** obtêm um sentido mais profundo da experiência do usuário do aplicativo com esse feedback do usuário rico em contexto.
* Os **Desenvolvedores** recebem contextos de aplicativos precisos para diagnosticar e corrigir erros ou diferenças de recursos.

### Ativando o feedback do usuário
{: #enable_user_feedback}

Conclua as etapas a seguir para ativar seu aplicativo móvel para capturar o feedback do usuário.

#### Instrumente seu app
{: #instrument_app}

* Instrumente seu app móvel para entrar no modo de feedback. Chame a API `Analytics.triggerFeedbackMode();` para chamar o modo de feedback. <!--For more information, refer to the documentation [here](instrument_an_app.html)-->.
* A API pode ser chamada em qualquer evento de aplicativo, tais como botões, ações de menu ou gestos.

#### Receber feedback do usuário
{: #receive_feedback}

* Os usuários e testadores de seu app podem alternar sobre o modo de feedback acionando a ação do aplicativo que é instrumentada para feedback.
* De dentro do modo de feedback, o feedback contextual rico juntamente com uma captura de tela pode ser reunido e enviado para o Mobile Analytics.

#### Analisar o feedback do usuário
{: #analyze_feedback}

* O Mobile Analytics recebe e consolida o feedback contextual rico enviado de aplicativos remotos.
* Efetue logon no console do Mobile Analytics e selecione a opção **Feedback do usuário** para visualizar o feedback.
* Um proprietário do app pode revisar o feedback, incluir comentários e identificar o feedback com um status de revisão. Os comentários podem geralmente ser ações planejadas, como links para problemas de Git que são criados para trabalhar no feedback, ou os comentários podem ser uma instrução que justifique o motivo pelo qual nenhuma ação é necessária no feedback.
* O status de revisão pode ser usado para gerenciar eficientemente o feedback, categorizando-o sob uma das diferentes opções de status de revisão.

