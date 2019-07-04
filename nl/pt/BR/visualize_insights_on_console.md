---

copyright:
  years: 2018, 2019
lastupdated: "2019-06-10"

keywords: mobile analytics, charts, visualize data, analytics console

subcollection:  mobilefoundation
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

Para visualizar insights dos dados de analítica que são capturados e enviados do seu aplicativo, deve-se ativar o console do Mobile Analytics clicando na opção **Analytics Console** na navegação do Mobile Foundation Operations Console.

O Mobile Analytics Console pode ser executado em dois modos:
  - **Modo demo ATIVADO**, que é puramente para propósitos de demonstração e mostra as diferentes visualizações de analítica (gráficos e tabelas) usando feeds de dados simulados.
  - **Modo demo DESATIVADO**, que mostra as várias visualizações de analítica com base nos feeds de dados em tempo real provenientes de seus aplicativos que são [instrumentados para o Mobile Analytics](/docs/services/mobilefoundation?topic=mobilefoundation-instrument_your_app#instrument_your_app).

Todas as visualizações de analítica podem ser removidas aplicando filtros em torno do *nome do aplicativo*, *versão*, *S.O. do dispositivo* e *período*, permitindo assim que você obtenha insights de diferentes perspectivas.

Para visualizar insights para seu aplicativo, assegure-se do seguinte.
  - Seu aplicativo é instrumentado para capturar e enviar os dados de analítica relevantes para o serviço Mobile Analytics.
  - Você DESATIVOU o modo Demo no console do Analytics
  - Você aplica os filtros corretos. Por exemplo, assegure-se de selecionar um período quando seu aplicativo for implementado no campo e estiver ativo com os usuários.

O console do Mobile Analytics fornece tipos diferentes de análise de seu uso de aplicativo móvel e desempenho, conforme categorizado na área de janela de navegação do console do Analytics. As seções a seguir detalham as diferentes visualizações de analítica:


## Usuários
{: #reports_visualized_users}
Essa visualização ajuda a obter insights sobre *Padrões de integração do usuário*, como o número de usuários ativos que usaram o app em um intervalo de data especificado e uma comparação do número de novos usuários versus usuários existentes que retornam para usar seu app.
Os gráficos nesta visualização podem ser filtrados no *nome do app*, no *sistema operacional* ou na *versão do sistema operacional*.

## Sessões
{: #reports_visualized_sessions}
Essa visualização ajuda a obter insights sobre os *Padrões de uso* do seu aplicativo em termos de *Sessões do app* para o intervalo de data especificado. Uma sessão é registrada quando um app é trazido para o primeiro plano de um dispositivo.  Você fica insights sobre quais horários do dia em que seu aplicativo é mais e menos usado e essas informações podem levar a insights úteis de negócios. Os gráficos nesta visualização podem ser filtrados no *nome do app*, no *sistema operacional* ou na *versão do sistema operacional*.

## Solicitações de rede
{: #reports_visualized_network_requests}
Esta visualização o ajuda a obter insights sobre a experiência de seu aplicativo, pois ele faz chamadas de API para os sistemas back-end.  Essa visualização tem tabelas e gráficos que mostram quais são as funções mais usadas de seus sistemas back-end e qual é o tempo de resposta e a estabilidade e se deve-se considerar o rebalanceamento de seus sistemas de suporte back-end.

Essa visualização contém gráficos que são criados com relação a um intervalo de dados que é selecionado, o Tempo médio de roundTrip de chamadas da API de saída de seu aplicativo, o número de solicitações feitas por chamada da API, o número de solicitações bem-sucedidas versus aquelas com falha agrupadas pelos códigos de resposta.  Os gráficos nesta visualização podem ser filtrados no nome do app, no sistema operacional ou nas versões do sistema operacional.

## Travamentos
{: #reports_visualized_crashes}
Essa visualização o ajuda com insights sobre o quão estável seu aplicativo estava entre um período selecionado e ajuda a decidir se seu design ou implementação do aplicativo deve ser corrigido.  Ela fornece gráficos que contrastam o número de travamentos com relação ao número total de usos e a taxa de travamento geral.  Os gráficos nesta visualização podem ser filtrados no *nome do app*, no *sistema operacional* ou na *versão do sistema operacional*.


## Resolução de problemas
{: #reports_visualized_troubleshooting}
Esta visualização fornece todas as informações necessárias que um desenvolvedor de aplicativos pode precisar para solucionar problemas de um aplicativo.  Essa visualização fornece uma análise mais detalhada dos travamentos de seu aplicativo em termos dos dispositivos que são afetados, do S.O. do host, do horário específico do travamento, do rastreio de pilha no momento do travamento e também de logs de travamento que podem ser transferidos por download para uma análise mais detalhada.  

Os logs de travamento são reunidos consultando os logs de app que são registrados no nível FATAL.  O SDK do Analytics Client para Android e iOS nativo manipula exceções não capturadas e registra detalhes sobre eles como mensagens de log de nível FATAL.  No entanto, se Cordova, quaisquer travamentos na camada JavaScript precisam ser manipulados pelo desenvolvedor e os logs de travamento que são enviados para o serviço Mobile Analytics precisam ser visualizados e analisados no console do Mobile Analytics.
{: note}


## Feedback do usuário
{: #reports_visualized_userfeedback}
Esta visualização fornece insights sobre a experiência interativa real dos seus usuários enquanto eles usam o app e sobre como eles se sentem em relação a isso.

* **Proprietários de app** podem obter uma visualização detalhada e rica em contexto de erros e outros feedbacks enviados por **Usuários e testadores** conforme registrado enquanto você executa o aplicativo
* **Desenvolvedores** podem receber contextos de aplicativos precisos para diagnosticar e corrigir erros ou diferenças de recurso.
* **Proprietários de app** e **Desenvolvedores** podem usar esta visualização para também gerenciar ações no feedback recebido, como gravação de comentários ou links para problemas criados em sistemas de rastreamento de erro.  Um status de revisão geral também pode ser configurado para cada feedback para ajudar a resumir as ações tomadas no feedback do usuário.

## Gráficos customizados
Essa visualização amplia o Mobile Analytics para casos customizados nos quais os **Proprietários de app** e os **Desenvolvedores** gostariam de construir suas próprias analíticas específicas do aplicativo.   Usando esse recurso, é possível construir suas próprias visualizações analíticas (gráficos, tabelas e assim por diante) em torno de dados de analítica padrão que são capturados pelo Client SDK e também dados customizados ou dados específicos do aplicativo que são registrados em log.  Para obter mais informações sobre esse recurso de analítica estendida, consulte [aqui](/docs/services/mobilefoundation?topic=mobilefoundation-build_custom_charts#build_custom_charts).
