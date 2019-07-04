---

copyright:
  years: 2018, 2019
lastupdated: "2019-06-10"

keywords: troubleshooting techniques

subcollection:  mobilefoundation
---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:tip: .tip}
{:pre: .pre}
{:codeblock: .codeblock}
{:screen: .screen}

# Resolução de problemas gerais
{: #general-troubleshooting}

Para isolar e resolver problemas com os seus produtos do {{site.data.keyword.IBM_notm}}, é possível usar as informações de resolução de problemas. Essas informações contêm instruções para usar os recursos de determinação de problemas que são fornecidos com os seus produtos do {{site.data.keyword.IBM_notm}}, incluindo o {{site.data.keyword.mobilefoundation_short}}.
{: shortdesc}

## Técnicas para resolução de problemas
{: #ts_overview}

*Resolução de problemas* é uma abordagem sistemática para solucionar um problema. O objetivo da resolução de problemas é determinar por que algo não funciona conforme o esperado e como resolver o problema. Determinadas técnicas comuns podem ajudar com a tarefa de resolução de problemas.

A primeira etapa no processo de resolução de problemas é descrever o problema completamente. As descrições do problema ajudam você e o representante de suporte técnico {{site.data.keyword.IBM_notm}} a saberem em que lugar começarem a localizar a causa do problema. Esta etapa inclui fazer perguntas básicas a si mesmo:

- Quais são os sintomas do problema?
- Onde o problema ocorre?
- Quando o problema ocorre?
- Sob que condições o problema ocorre?
- O problema pode ser reproduzido?

As respostas a essas perguntas geralmente levam a uma boa descrição do problema, que pode levar você a uma resolução do problema.

### Quais são os sintomas do problema?

Quando você começa a descrever um problema, a pergunta mais óbvia é **Qual é o problema?** Essa questão pode parecer direta; entretanto, é possível dividi-la em várias questões mais focadas que criam uma imagem mais descritiva do problema. Essas perguntas podem incluir o seguinte,

- Quem, ou o que, está relatando o problema?
- Quais são os códigos de erro e as mensagens?
- Como o sistema falha? Por exemplo, isso é um loop, uma interrupção, um travamento, uma degradação de desempenho ou um resultado incorreto?

### Onde o problema ocorre?

Nem sempre é fácil determinar a origem do problema, mas essa é uma das etapas mais importantes na resolução de um problema. Muitas camadas de tecnologia podem existir entre o relatório e os componentes com falha. Redes, discos e drivers são apenas alguns dos componentes a considerar quando você estiver investigando problemas.

As perguntas a seguir ajudam você a focar em onde o problema ocorre para isolar a camada do problema:

- O problema é específico a uma plataforma ou sistema operacional ou é comum em diversas plataformas ou sistemas operacionais?
- O ambiente e a configuração atuais são suportados?
- Todos os usuários têm o problema?
- (Para instalações em múltiplos sites.) Todos os sites têm o problema?

O fato de uma camada relatar o problema não significa que o problema tem origem nessa camada. Parte da identificação da origem de um problema é entender o ambiente no qual ele existe. Reserve algum tempo para descrever completamente o ambiente do problema, incluindo o sistema operacional e a versão, todos os softwares e versões correspondentes e informações de hardware. Confirme se você está executando em um ambiente que é uma configuração suportada. Muitos problemas podem ser rastreados de volta para níveis incompatíveis de softwares que não se destinam a serem executados juntos ou que não são totalmente testados juntos.

### Quando o problema ocorre?

Desenvolva uma linha de tempo detalhada de eventos que leve a uma falha, principalmente para os casos que são ocorrências únicas. É possível desenvolver mais facilmente uma linha de tempo trabalhando de trás para a frente: comece com o momento em que um erro foi relatado (tão precisamente quanto possível, até mesmo o milissegundo) e trabalhe retroativamente por meio dos logs e informações disponíveis. Geralmente, é necessário observar apenas até o primeiro evento suspeito que você localizar em um log de diagnóstico.

Para desenvolver uma linha de tempo detalhada de eventos, responda estas perguntas:

- O problema acontece apenas em um determinado horário do dia ou da noite?
- Com que frequência o problema ocorre?
- Qual sequência de eventos leva ao momento em que o problema é relatado?
- O problema ocorre após uma mudança de ambiente, como fazer upgrade ou instalar software ou hardware?

Responder esse tipo de pergunta pode fornecer a você um quadro de referência no qual investigar o problema.

### Sob que condições o problema ocorre?

Saber quais sistemas e aplicativos estão em execução no momento em que um problema ocorre é uma parte importante da resolução de problemas. Essas perguntas sobre o seu ambiente podem ajudá-lo a identificar a causa raiz do problema:

- O problema sempre ocorre quando a mesma tarefa está sendo executada?
- Uma determinada sequência de eventos precisa acontecer para que o problema ocorra?
- Algum outro aplicativo falha ao mesmo tempo?

Responder a esses tipos de perguntas pode ajudá-lo a explicar o ambiente no qual o problema ocorre e correlacionar quaisquer dependências. Só porque múltiplos problemas ocorreram ao mesmo tempo, os problemas não estão necessariamente relacionados.

### O problema pode ser reproduzido?

De um ponto de vista de resolução de problemas, o problema ideal é aquele que pode ser reproduzido. Geralmente, quando um problema pode ser reproduzido, você tem um conjunto maior de ferramentas ou procedimentos à sua disposição para ajudá-lo a investigar. Portanto, os problemas que podem ser reproduzidos são frequentemente mais fáceis de depurar e resolver.

No entanto, os problemas que podem ser reproduzidos podem ter uma desvantagem: Se o problema for de impacto comercial significativo, você não deseja que ele ocorra novamente. Se possível, recrie o problema em um ambiente de teste ou de desenvolvimento, que geralmente oferece mais flexibilidade e controle durante sua investigação.

- O Problema pode ser recriado em um sistema de teste?
- Múltiplos usuários ou aplicativos estão encontrando o mesmo tipo de problema?
- O problema pode ser recriado executando um único comando, um conjunto de comandos ou um aplicativo específico?


##  Limitações Conhecidas
{: #knownlimitations_mfp}

* A IU do serviço {{site.data.keyword.mobilefoundation_short}} não usa o padrão específico de código de idioma selecionado pelo usuário para exibir números.

## Obtendo ajuda e suporte para o Mobile Foundation
{: #getting_help_mobilefoundation}

Se você tiver problemas ou perguntas ao usar o {{site.data.keyword.mobilefoundation_short}}, será possível obter ajuda procurando informações ou fazendo perguntas por meio de um fórum. Também é possível abrir um chamado de suporte.

Ao usar os fóruns para fazer uma pergunta, identifique-a para que ela possa ser vista pelas equipes de desenvolvimento do {{site.data.keyword.Bluemix_notm}}.

Se você tiver questões técnicas sobre o desenvolvimento ou implementação de um app com o {{site.data.keyword.mobilefoundation_short}}, poste sua questão no [Stack Overflow ![Ícone de link externo](../../icons/launch-glyph.svg "Ícone de link externo")](http://stackoverflow.com/search?q=ibm-mobilefirst+bluemix){:new_window} e identifique-a com `bluemix` e `ibm-mobilefirst`.

Para questões sobre o serviço e instruções de introdução, use o fórum [IBM developerWorks dW Answers ![Ícone de link externo](../../icons/launch-glyph.svg "Ícone de link externo")](https://developer.ibm.com/answers/topics/mobilefirst/?smartspace=bluemix){:new_window}. Inclua as marcações `bluemix` e `mobilefirst`.

Para obter mais informações sobre como abrir um chamado de suporte IBM ou sobre níveis de suporte e severidades de chamado, consulte [Contatando o suporte](https://cloud.ibm.com/docs/get-support?topic=get-support-getting-customer-support){: new_window}.
