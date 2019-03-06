---

copyright:
  years: 2018, 2019
lastupdated: "2018-11-22"

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

# Construa gráficos customizados
{: #build_custom_charts}

A visualização Gráficos customizados no console do Mobile Analytics fornece a flexibilidade de construir as suas próprias visualizações ao redor dos dados de analítica capturados e armazenados. Isso ajuda a aprofundar os insights sobre o que é fornecido de imediato ou até mesmo estendê-lo, sem interrupções, ao Business Analytics, criado em torno de dados customizados.

Nesta visualização, é possível escolher qualquer um dos conjuntos de dados de analítica suportados e, em seguida, selecionar um dos tipos de gráficos suportados para plotar o conjunto de dados. É possível ainda remover a visualização definindo filtros a serem aplicados sobre os dados que estiverem sendo plotados.  

Conjuntos de dados suportados:
 * Logs do app
 * Sessões do app
 * Dados customizados
 * Transações de rede
 
Tipos de gráficos suportados:
 * Gráfico de barras
 * Fluxograma
 * Gráfico de linhas
 * Grupo de métricas 
 * Gráfico de pizza
 * Tabela
 
A seleção do conjunto de dados, o tipo de gráfico a ser plotado, a definição das características do gráfico e os filtros de dados a serem aplicados podem ser puxados para uma Definição de gráfico customizado e salvos.  É possível criar e salvar quantas definições de Gráfico customizado forem necessárias. Gráficos customizados salvos aparecem na visualização Gráficos customizados com os dados de analítica relevantes plotados. 

## Criando um gráfico customizado
{: #creating_custom_chart}

Crie um gráfico customizado usando as etapas a seguir:

1.  Clique no botão **Criar gráfico** na guia **Gráficos customizados** por meio do painel do Mobile Analytics.
2.  Na guia **Configurações gerais**, selecione **Título do gráfico**, **Tipo de evento** e o **Tipo de gráfico**.
3.  Ao selecionar o *Tipo de evento* e o *Tipo de gráfico*, a guia **Definição de gráfico** aparecerá. Use a guia *Definição de gráfico* para definir o gráfico para o tipo de gráfico especificado que você selecionou anteriormente. Após você definir o gráfico, será possível configurar os filtros de gráfico e as propriedades do gráfico.
4.  **Filtros de gráfico** são usados para ajustar com precisão o gráfico customizado. Mais de um filtro pode ser definido para um gráfico.
    Por exemplo, após definir um gráfico para visualizar a duração média da sessão do app, se você desejar visualizar esse gráfico somente para um app específico, será possível criar um filtro conforme a seguir:
    * Selecione **Nome do aplicativo** para **Propriedade**.
    * Selecione **Equivale a** para **Operador**.
    * Selecione o nome de seu app para **Valor**.
    * Clique em **Incluir filtro**.
    O filtro de nome do aplicativo é incluído na tabela de filtros para o seu gráfico.
5.  As propriedades de gráfico estão disponíveis para os tipos de gráfico **Tabela**, **Gráfico de barras** e **Gráfico de linhas**. O objetivo das propriedades do gráfico é aprimorar como os dados são apresentados para que a visualização seja mais efetiva.
    Se você criou um gráfico de **Tabela**, as propriedades do gráfico serão configuradas para definir o tamanho da página da tabela, o campo no qual classificar e a ordem de classificação do campo.
    Se você criou um **Gráfico de barras** ou **Gráfico de linhas**, as propriedades do gráfico serão configuradas para rotular linhas de limite para incluir um quadro de referência para qualquer pessoa que estiver monitorando o gráfico.

## Obtendo insights customizados por meio de logs de dados customizados
{: #creating_custom_chart_for_client_logs}    

Se você desejar obter insights customizados mais detalhados, como rastreio de usuários em todo o aplicativo, então, em primeiro lugar, será necessário capturar as informações relevantes da trilha do usuário, como página escolhida, opção selecionada ou botão clicado, como dados customizados e registrá-los. Consulte o tópico sobre [instrumentando o seu app](/docs/services/mobilefoundation?topic=mobilefoundation-instrument_your_app#instrument_your_app), sobre como registrar dados customizados.

Em seguida, crie uma definição de gráfico customizado com Dados customizados como o EventType e escolha um Tipo de gráfico. À medida que você continuar a definir **Propriedades do gráfico** ou **Filtros de gráfico**, você observará os tipos de Dados customizados e os valores que são mostrados nas caixas drop-down.  Faça as seleções relevantes para o tipo de insight que você está procurando.  

A profundidade e a utilidade de insights customizados dependem inteiramente de quão eficientemente ou relevantemente você definiu e capturou dados customizados em seus aplicativos.
{: note}

## Tipos de gráficos
{: #types_of_charts}

### Gráfico de barras
{:  #bar_graph}

O gráfico de barras permite a visualização de dados numéricos sobre um eixo X. Ao definir um gráfico de barras, deve-se escolher o valor para o eixo X primeiro. É possível escolher entre os valores possíveis a seguir.

* **Linha de tempo** - se você desejar ver os seus dados como uma tendência (por exemplo, duração média de sessão do app ao longo do tempo), escolha *Linha de tempo* para o eixo X.
* **Propriedade** - escolha Propriedade se você desejar ver um detalhamento de contagem para a propriedade específica. Se você escolher Propriedade para o eixo X, então, o Total será escolhido implicitamente para o Eixo Y. Por exemplo, escolha *Propriedade* para o Eixo X e o *Nome do aplicativo* para *Propriedade* para ver uma contagem para um tipo de evento especificado, que é dividido por nome do app.

Após você definir um valor para o eixo X, será possível definir um valor para o eixo Y. Se você escolher *Linha de tempo* para o eixo X, será possível escolher os valores possíveis a seguir para o eixo Y.

* **Average** - calcula a média de uma propriedade numérica no tipo de evento fornecido.
* **Total** - uma contagem total de uma propriedade no tipo de evento fornecido.
* **Unique** - uma contagem exclusiva de uma propriedade no tipo de evento fornecido.
Após a definição dos eixos de gráfico, deve-se escolher um valor para Propriedade.

### Gráfico de linhas
{:  #line_graph}

O gráfico de linhas permite a visualização de alguma métrica ao longo do tempo. Esse tipo de gráfico é valioso quando você deseja visualizar a tendência de dados ao longo do tempo. O primeiro valor a ser definido quando você cria um gráfico de linhas é **Measure**, que tem os valores possíveis a seguir.

* **Average** - calcula a média de uma propriedade numérica no tipo de evento fornecido.
* **Total** - uma contagem total de uma propriedade no tipo de evento fornecido.
* **Unique** - uma contagem exclusiva de uma propriedade no tipo de evento fornecido.
Após a definição da medida, deve-se escolher um valor para **Propriedade**.

### Fluxograma
{:  #flow_chart}

O fluxograma permite a visualização do detalhamento de fluxo de uma propriedade para outra. Para um fluxograma, as propriedades a seguir devem ser configuradas.

* **Source** - o valor de um nó de origem no diagrama.
* **Destination** - o valor do nó de destino no diagrama.
* **Property** - um valor da propriedade do nó de origem ou do nó de destino.
Com o fluxograma, é possível ver o detalhamento de densidade de várias origens que fluem para um destino ou o contrário. Por exemplo, se você desejar ver o detalhamento de severidades de log para um app, será possível definir os valores a seguir.

* Selecione *Nome do aplicativo* para **Source**.
* Selecione *Nível de log* para **Destination**.
* Selecione o nome de seu app para **Property**.

### Grupo de métricas
{:  #metric_group}

O grupo de métricas pode ser usado para visualizar uma única métrica que é medida como um valor médio, uma contagem total ou uma contagem exclusiva. Para definir um grupo de métricas, deve-se definir um dos valores possíveis a seguir para **Measure**.

* **Average** - calcula a média de uma propriedade numérica no tipo de evento fornecido.
* **Total** - uma contagem total de uma propriedade no tipo de evento fornecido.
* **Unique** - uma contagem exclusiva de uma propriedade no tipo de evento fornecido.
Após a definição da medida, deve-se escolher um valor para *Propriedade*. Essa métrica é exibida no grupo de métricas.

### Gráficos de pizza
{:  #pie_chart}

O gráfico de pizza pode ser usado para visualizar o detalhamento de contagem de valores para uma propriedade específica. Por exemplo, se você desejar ver um detalhamento de travamento, defina os valores a seguir.

* Selecione *App Session* para **Event Type**.
* Selecione *Pie Chart* para **Chart Type**.
* Selecione *Closed By* para **Property**.
O gráfico de pizza resultante mostra o detalhamento das sessões do aplicativo que foram encerradas pelo usuário em vez de sessões do app, que foram encerradas por um travamento.

### Tabela
{:  #table}

A tabela será útil quando você desejar ver os dados brutos. Construir uma tabela é tão simples quanto incluir colunas para os dados brutos que você deseja ver.
Como nem todas as propriedades são necessárias para tipos de eventos específicos, valores nulos poderão aparecer em sua tabela. Se você desejar evitar que essas linhas apareçam em sua tabela, inclua um filtro *Existe* para uma propriedade específica na guia **Filtros de gráfico**

