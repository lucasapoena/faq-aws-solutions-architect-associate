# 🛡️ Lab: Criando um Usuário IAM com MFA e Acesso via AWS CLI com STS

## 🎯 Objetivo

Configurar um **usuário IAM** com **MFA** e **acesso programático** (chaves) e, a partir dele, **gerar credenciais temporárias via STS** para uso seguro na **AWS CLI**.

Ao final, você vai:

* Ter um **usuário de laboratório** com permissões mínimas.
* Autenticar com **MFA**.
* Gerar **credenciais temporárias (STS)** e testá-las com a **CLI**.
* Saber **revogar/limpar** tudo com segurança.

## 💸 Custos & Segurança

* **Custo:** zero fora do Free Tier (somente recursos de IAM/STS/CLI).
* **Segurança:** aplique o **princípio do menor privilégio**. Não deixe chaves ativas à toa. Ao terminar, **desative/exclua** chaves e, se for um lab descartável, **remova** o usuário.

## ✅ Pré-requisitos

* Conta AWS com permissão para administrar IAM.
* Acesso ao Console AWS.
* App de MFA (Google Authenticator, Authy, 1Password etc.).
* **AWS CLI v2** instalada no seu computador (ou use o **CloudShell**).

## 🗺️ O que vamos montar (visão rápida)

1. Criar o usuário **`usuario-terminal`** (apenas acesso programático).
2. Anexar **permissões mínimas** (ex.: `ReadOnlyAccess` ou policy custom de leitura S3).
3. **Ativar MFA** no usuário.
4. Configurar **perfil base** na CLI com as chaves do usuário.
5. Usar **STS** + **MFA** para emitir **credenciais temporárias**.
6. Configurar **perfil temporário** e **testar** comandos.
7. **Revogar/limpar** tudo.

## 0) Padronização & limpeza (opcional, mas recomendado)

* **Nome do usuário:** `usuario-terminal`
* **Perfis da CLI:**

  * Base (chave longa): `usuario-mfa`
  * Temporário (STS): `usuario-temporario`
* **Plano de limpeza:** ao fim, desativar/excluir chaves e, se desejar, excluir o usuário.

## 1) Criar o usuário IAM (apenas programático)

Console AWS → **IAM** → **Users** → **Add users**:

1. **User name:** `usuario-terminal`
2. **Access type:** marque **Access key – Programmatic access**
3. **Permissions:**

   * Rápido: anexe **`ReadOnlyAccess`**
   * (OU) Policy custom mínima (exemplo S3 leitura):

     ```json
     {
       "Version": "2012-10-17",
       "Statement": [
         { "Effect": "Allow", "Action": ["s3:ListBucket","s3:GetObject"], "Resource": "*" }
       ]
     }
     ```
4. Finalize e **guarde** `Access Key ID` e `Secret Access Key` **com segurança**.

> 💡 Por que “apenas programático”? Assim o usuário **não** faz login no Console; serve só para CLI/SDK/APIs, reduzindo superfície de ataque.

## 2) Habilitar o MFA do usuário

Console → **IAM > Users > `usuario-terminal` > Security credentials**:

1. Em **Assigned MFA device**, clique **Manage**.
2. Escolha **Virtual MFA device** e leia o QR com o app.
3. Informe **dois códigos seguidos** para confirmar.
4. Salve.

> 🔎 Depois de configurar, você pode ver o **ARN do dispositivo MFA** na tela do usuário ou via CLI:
> `aws iam list-mfa-devices --user-name usuario-terminal --profile <perfil_admin>`

## 3) Instalar/validar a AWS CLI

* **Windows (PowerShell):**

  ```powershell
  msiexec.exe /i https://awscli.amazonaws.com/AWSCLIV2.msi
  ```
* **macOS:**

  ```bash
  brew install awscli
  ```
* **Linux (Debian/Ubuntu):**

  ```bash
  curl "https://awscli.amazonaws.com/awscli-exe-linux-x86_64.zip" -o "awscliv2.zip"
  unzip awscliv2.zip
  sudo ./aws/install
  ```

Valide:

```bash
aws --version
```

## 4) Configurar o **perfil base** (chaves longas)

Esse perfil guarda **as chaves do usuário** (não é o que você usará no dia a dia, é só o “ponto de partida” para pedir o token STS):

```bash
aws configure --profile usuario-mfa
# Preencha: Access Key ID / Secret Access Key / region (ex: us-east-1) / json
```

Teste (deve funcionar com as permissões mínimas que você deu):

```bash
aws sts get-caller-identity --profile usuario-mfa
```

## 5) Gerar **credenciais temporárias** via STS + MFA

Use o **ARN do dispositivo MFA** e o código TOTP atual:

```bash
aws sts get-session-token \
  --serial-number arn:aws:iam::<SEU_ID_CONTA>:mfa/<NOME_DO_DEVICE> \
  --token-code 123456 \
  --profile usuario-mfa
```

Copie do retorno **os 3 itens**:

* `AccessKeyId`
* `SecretAccessKey`
* `SessionToken`  ← **obrigatório!**

---

## 6) Criar o **perfil temporário** e testar

Crie/atualize o perfil **incluindo o session token**:

```bash
aws configure set aws_access_key_id     ASIA...   --profile usuario-temporario
aws configure set aws_secret_access_key QGQA...   --profile usuario-temporario
aws configure set aws_session_token     IQoJ...   --profile usuario-temporario
aws configure set region                us-east-1 --profile usuario-temporario
aws configure set output                json      --profile usuario-temporario
```

Teste:

```bash
aws sts get-caller-identity --profile usuario-temporario
aws s3 ls                   --profile usuario-temporario
```

> ⏱️ **Validade**: por padrão, as credenciais STS expiram em algumas horas (veja `Expiration` no JSON). Quando expirar, gere novas e **substitua os 3 valores** no perfil.

---

## 7) Limpeza (boa prática)

Quando terminar:

1. Desative/exclua **access keys** do `usuario-terminal` (IAM → Users → Security credentials).
2. Se for um usuário apenas de lab, considere **excluir o usuário**.
3. Remova o perfil temporário do arquivo `~/.aws/credentials` se quiser.

---

## 🧯 Solução de Problemas (FAQ)

### ❌ `InvalidAccessKeyId` ao usar o perfil temporário

**Causa comum:** você **não configurou** o `aws_session_token`.
As credenciais de **STS** têm **3 partes**; sem o `SessionToken` a AWS entende que a chave “não existe”.
**Como resolver:** rode os `aws configure set ...` acima **incluindo** `aws_session_token` (ou edite `~/.aws/credentials` manualmente).

### 🔐 `AccessDenied` em algum comando

* O usuário tem apenas `ReadOnlyAccess`? Tente uma ação de leitura (ex.: `aws s3 ls`).
* Falta permissão específica (ex.: `s3:ListAllMyBuckets`)? Ajuste a policy.

### 🧭 ARN do MFA incorreto

* Confirme o ARN exato em **IAM > Users > usuario-terminal > Security credentials**
* Ou via CLI:

  ```bash
  aws iam list-mfa-devices --user-name usuario-terminal --profile <perfil_admin>
  ```

### ⏱️ Código MFA inválido / expiração

* O TOTP muda a cada \~30s. Gere um código **recente**.
* Garanta que o relógio do celular esteja **sincronizado**.

### ⌛ Expirou o STS

* Gere um **novo get-session-token** e atualize os **3** campos no perfil `usuario-temporario`.

### 👤 Perfil errado

* Verifique `--profile` em cada comando.
* Para saber qual credencial está ativa, use:

  ```bash
  aws sts get-caller-identity --profile <perfil>
  ```

---

## 🗒️ Notas & Aprendizados (preencha após rodar)

* Recursos usados: IAM, STS, CLI
* Tempo gasto:
* Dificuldades:
* O que melhoraria / automatizaria: 