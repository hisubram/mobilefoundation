---

copyright:
  years: 2018, 2019
lastupdated:  "2018-12-20"

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen:  .screen}
{:codeblock:  .codeblock}
{:tip: .tip}
{:note: .note}

# Migrar o plano de serviço do Mobile Foundation

As instâncias do Mobile Foundation criadas usando os planos descontinuados precisam ser atualizadas para os novos planos. A atualização do plano também pode ser necessária com base no uso da instância.
{: shortdesc}

## Cenário de amostra: migrar do Plano Professional Per Device para o plano Professional 1 Application

1. No painel do IBM Cloud, selecione a instância do IBM Mobile Foundation que você deseja migrar.
2. Selecione **Plano** na navegação esquerda.
   ![Plano do Mobile Foundation existente](images/existing-plan.png)
3. Nos planos de precificação listados, selecione Professional 1 Application.
   ![Novo plano do Mobile Foundation](images/new-plan.png)
4. Clique no botão **Salvar** e confirme a migração do plano.
     A migração para o Professional 1 Application está agora concluída e todos os dados existentes são retidos. O faturamento foi mudado e não há tempo de inatividade.
5. Após a migração do plano, a instância do Mobile Foundation precisa ser recriada por meio do painel de serviço para que a configuração correta entre em vigor. Essa atualização requer um tempo de inatividade curto. Você precisará planejar-se para o tempo de inatividade. Selecione **Gerenciar** na navegação esquerda e clique em **Recriar**.

Se você está em um dos planos descontinuados, deve-se migrar para um novo plano.
{: note}

## Migrações de plano suportadas

* O plano *Developer* (descontinuado) pode ser atualizado somente para o novo plano *Developer*.
* O plano *Developer Pro* (descontinuado) pode ser atualizado somente para o plano *Professional Per Device* ou *Professional 1 Application* .
* O plano *Professional Per Capacity* (descontinuado) pode ser atualizado somente para o plano *Professional Per Device* ou *Professional 1 Application*.
* O plano *Professional Per Device* pode ser atualizado somente para o plano *Professional 1 Application*.
* O plano *Professional 1 Application* pode ser atualizado somente para o plano *Professional Per Device*.
* A atualização do plano não é suportada para o novo plano *Developer*.
