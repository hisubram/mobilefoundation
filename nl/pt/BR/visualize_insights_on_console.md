---

copyright:
  years: 2018, 2019
lastupdated: "2019-02-01"

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

Para visualizar insights dos dados de analítica capturados e enviados de seu aplicativo, deve-se iniciar o console do Mobile Analytics clicando na opção **Console de analítica** da navegação à esquerda do console do Mobile Foundation Operations.

O Mobile Analytics Console pode ser executado em dois modos:
  - **Modo Demo LIGADO** que é puramente para propósitos de demonstração mostrando as diferentes visualizações de analítica (gráficos e tabelas) usando feeds de dados simulados.
  - **Modo Demo DESLIGADO** que mostra as várias visualizações de analítica com base nos feeds de dados em tempo real provenientes de seus aplicativos [instrumentados para o Mobile Analytics](/docs/services/mobilefoundation?topic=mobilefoundation-instrument_your_app#instrument_your_app).
  
Todas as visualizações de analítica podem ser removidas aplicando filtros em torno do *nome do aplicativo*, *versão*, *S.O. do dispositivo* e *período de tempo*, permitindo, assim, que você obtenha insights por meio de diferentes perspectivas.

Para visualizar insights para o seu aplicativo, assegure-se de que:
  - O seu aplicativo esteja instrumentado de forma apropriada para capturar e enviar os dados de analítica relevantes para o serviço do Mobile Analytics.
  - Você desligou o modo Demo no console do Analytics
  - Você aplica os filtros corretos. Por exemplo, assegure-se de selecionar um período de tempo em que o seu aplicativo tenha sido implementado no campo e esteja ativo com usuários.

O console do Mobile Analytics fornece diferentes tipos de análise de seu uso de aplicativo móvel e desempenho, conforme categorizado na área de janela de navegação esquerda do console do Analytics. As seções a seguir detalham as diferentes visualizações de analítica: 


## Usuários
{: #reports_visualized_users}
Esta visualização o ajuda a obter insigts sobre 'Padrões de migração do usuário', como o número de usuários ativos que usaram o app dentro de um intervalo de data especificado e uma comparação do número de novos usuários versus usuários existentes que retornam para usar o seu app.
Os gráficos nesta visualização podem ser filtrados no *nome do app*, no *sistema operacional* ou na *versão do sistema operacional*.

## Sessões
{: #reports_visualized_sessions}
Esta visualização o ajuda a obter insights sobre os 'Padrões de uso' de seu aplicativo em termos de *Sessões do app* para o intervalo de data especificado. Uma sessão é registrada quando um app é trazido para o primeiro plano de um dispositivo.  Você obterá insights sobre as horas do dia em que o seu aplicativo é mais e menos usado e isso poderá levar a insights úteis sobre os seus negócios. Os gráficos nesta visualização podem ser filtrados no *nome do app*, no *sistema operacional* ou na *versão do sistema operacional*.

## Solicitações de rede
{: #reports_visualized_network_requests}
Esta visualização o ajuda a obter insights sobre a experiência de seu aplicativo, pois ele faz chamadas de API para os sistemas back-end. Esta visualização tem tabelas e gráficos que fornecem uma visão geral sobre quais são as funções mais usadas de seus sistemas back-end e qual tem sido o seu tempo de resposta e estabilidade e se é necessário considerar o rebalanceamento de seus sistemas de suporte de back-end.

Esta visualização contém gráficos que plotam com relação a um determinado intervalo de dados o Tempo Médio de RoundTrip de chamadas de API de saída de seu aplicativo, o número de solicitações feitas por chamada de API, o número de solicitações bem-sucedidas versus as que falharam agrupadas pelos códigos de resposta. Os gráficos nesta visualização podem ser filtrados no nome do app, no sistema operacional ou nas versões do sistema operacional.

## Travamentos
{: #reports_visualized_crashes}
Esta visualização o ajuda com insights sobre quão estável o seu aplicativo esteve durante um período de tempo selecionado e o ajuda a decidir se o design/a implementação de seu aplicativo devem ser corrigidos. Ela fornece gráficos que contrastam o número de travamentos com relação ao número total de usos e a taxa de travamento geral. Os gráficos nesta visualização podem ser filtrados no *nome do app*, no *sistema operacional* ou na *versão do sistema operacional*.


## Resolução de problemas
{: #reports_visualized_troubleshooting}
Esta visualização fornece todas as informações necessárias que um desenvolvedor de aplicativos pode precisar para solucionar problemas de um aplicativo. Esta visualização fornece uma análise mais detalhada dos travamentos do seu aplicativo em termos dos dispositivos afetados, o S.O. do host, o tempo específico do travamento, o rastreio de pilha no momento do travamento e também os logs de travamento que podem ser transferidos por download para obter uma análise mais detalhada.  

Os logs de travamento são reunidos procurando os logs do app que foram registrados no nível FATAL. O SDK do Analytics Client para Android e iOS nativo manipula exceções não capturadas e registra detalhes sobre eles como mensagens de log de nível FATAL. No entanto, no caso do Cordova, quaisquer travamentos na camada do JavaScript precisam ser manipulados pelo desenvolvedor e os logs de travamento enviados para o serviço do Mobile Analytics a serem visualizados e analisados no console do Mobile Analytics.
{: note}


## Feedback do usuário
{: #reports_visualized_userfeedback}
Esta visualização fornece insights sobre a experiência interativa real dos seus usuários enquanto eles usam o app e sobre como eles se sentem em relação a isso.

* **Proprietários de app** podem obter uma visualização detalhada e detalhada de contexto de erros e outros feedbacks enviados por **Usuários e testadores** conforme registrado ao executar o aplicativo
* **Desenvolvedores** podem receber contextos de aplicativos precisos para diagnosticar e corrigir erros ou diferenças de recurso.
* **Proprietários de app** e **Desenvolvedores** podem usar esta visualização para também gerenciar ações no feedback recebido, como gravação de comentários ou links para problemas criados em sistemas de rastreamento de erro. Um status de revisão geral também pode ser configurado para cada feedback para ajudar a resumir as ações tomadas no feedback do usuário.

## Gráficos customizados
Esta visualização estende o Mobile Analytics para casos customizados em que os **Proprietários de app** e **Desenvolvedores** gostariam de construir a sua própria analítica específica do aplicativo. Usando esse recurso, é possível construir as suas próprias visualizações de analítica (gráficos, tabelas etc.) ao redor de dados de analítica padrão que são capturados pelo Client SDK e também dados customizados ou dados específicos do aplicativo que são registrados. Consulte [aqui](/docs/services/mobilefoundation?topic=mobilefoundation-build_custom_charts#build_custom_charts) para obter mais informações sobre esse recurso de analítica estendido.

