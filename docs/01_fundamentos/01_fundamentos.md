# 🌱 Fundamentos da AWS

## 🖥️ Console, CLI e CloudShell

* **AWS Console**: é como um painel web, onde você clica em menus e botões.
  👉 Pense nele como o **aplicativo do banco no celular**: você faz tudo pela interface gráfica.

* **AWS CLI (Command Line Interface)**: é um terminal para interagir com a AWS via comandos.
  👉 Comparando: se o console é o app do banco, a CLI é como **falar direto com o caixa do banco** pedindo para executar ações — mais rápido, mas exige conhecer os comandos.

* **AWS CloudShell**: é um **terminal já pronto dentro da AWS**, sem precisar instalar nada no seu computador.
  👉 Imagine um **computador emprestado pela AWS** já configurado para acessar sua conta.



## 💸 Free Tier (antigo e novo modelo)

A AWS oferece um **nível gratuito (Free Tier)** para você praticar.

* **Antigo Free Tier (até 15/07/2025):**

  * 12 meses gratuitos de alguns serviços (ex: 750 horas por mês de EC2 t2.micro).
  * Alguns serviços sempre gratuitos (ex: 1 milhão de execuções de Lambda/mês).

* **Novo Free Tier (após 15/07/2025):**

  * Conta recebe **US\$ 100 de créditos** (podendo chegar a US\$ 200 com atividades extras).
  * Existe a opção de **plano sem cobrança**: quando acaba o crédito, a conta simplesmente para.

👉 **Analogia**: é como um **cartão pré-pago de celular**. Você tem um saldo inicial (créditos da AWS). Se acabar, não há cobrança inesperada — apenas para de funcionar.

⚠️ Atenção: sempre desligue (de-provisionar) os recursos após usar, para não gerar custos.

📑 [Documentação - Free Tier](https://aws.amazon.com/pt/free)



## 🛡️ IAM
Os detalhes completos estão em [`docs/resumos-por-servico/iam.md`](../resumos-por-servico/iam.md).  
> IAM (Identity and Access Management) controla **quem pode fazer o quê** dentro da AWS.




## 🏗️ AWS Well-Architected Framework
Os detalhes completos estão em [`docs/resumos-por-servico/well-architected.md`](../resumos-por-servico/well-architected.md).  
> Conjunto de boas práticas da AWS que guiam a construção de sistemas seguros, eficientes e econômicos.



## 📌 Resumo Final

* Console = interface gráfica (fácil, mas manual).
* CLI = comandos rápidos (poderoso, exige prática).
* CloudShell = terminal pronto no navegador.
* Free Tier = saldo gratuito para aprender, mas com limites.
* IAM = controle de identidade e acesso (usuários, grupos, políticas, MFA).
* STS = credenciais temporárias (crachá de visitante).
* Well-Architected = guia de boas práticas para criar sistemas robustos.