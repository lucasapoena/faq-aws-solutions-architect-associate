# 🛡️ IAM — Identity and Access Management

IAM é o serviço da AWS que controla **identidade e acesso**.  
É como o **controle de crachás e permissões em uma empresa**: cada pessoa tem seu crachá (usuário), pode estar em setores (grupos) e tem regras de acesso (políticas).

## 👤 Usuários
- Representam pessoas ou aplicações que acessam a AWS.  
- Cada usuário tem credenciais próprias.  
- ⚠️ Nunca usar o **usuário root** para tarefas do dia a dia.

## 👥 Grupos
- Conjunto de usuários com permissões comuns.  
- Ex.: Grupo **Marketing** pode ler/gravar no S3; Grupo **Financeiro** só pode ler.  
- 👉 Facilita a administração: ao invés de configurar usuário por usuário, basta gerenciar o grupo.

## 📜 Políticas (Policies)
- Conjuntos de regras em formato JSON que dizem **quem pode fazer o quê**.  
- Estrutura básica:
  ```json
  {
    "Effect": "Allow",
    "Action": "s3:ListBucket",
    "Resource": "*"
  }

* **Allow** → permite ações; **Deny** → bloqueia (e sempre tem prioridade).

## 🔐 MFA (Multi-Factor Authentication)

* Exige duas formas de autenticação: senha + código de app/celular.
* 👉 Como um **cofre que precisa da chave e da digital** para abrir.
* Aumenta muito a segurança.

## ⏳ STS (Security Token Service)

* Gera **credenciais temporárias** (válidas por minutos ou horas).
* 👉 Equivale a um **crachá de visitante**: dá acesso temporário e depois expira.
* Útil para acessos de terceiros ou integrações.

## 📌 Boas Práticas

* Nunca usar credenciais fixas sem necessidade.
* Usar MFA em todos os acessos.
* Preferir **roles + STS** em vez de usuários com acesso direto.
* Aplicar **princípio do menor privilégio**: dar só as permissões necessárias.

## 📚 Resumo

* **Usuário** = crachá individual
* **Grupo** = setor da empresa
* **Política** = regra de acesso
* **MFA** = tranca dupla
* **STS** = crachá temporário
* **Deny** > Allow: uma negação explícita sempre vence