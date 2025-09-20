## ❓ Perguntas de Simulado — Seção Fundamentos

1. Um usuário IAM chamado `usuario-terminal` foi criado com acesso programático. Ele tem permissão apenas de leitura (ReadOnlyAccess) e nenhuma permissão para console. Qual comportamento abaixo está correto quando esse usuário tenta fazer login no Console da AWS?

    -[] A) Ele pode fazer login normalmente, mas verá apenas opções de leitura.
- [] B) Ele não pode fazer login no Console AWS, pois não foi criada senha de Console.
- [] C) Ele pode fazer login no Console se usar a mesma Access Key + Secret.
- [] D) Ele pode fazer login apenas se ativar MFA primeiro.

---

2. Ao configurar um dispositivo MFA virtual para `usuario-terminal`, qual das seguintes práticas é importante para garantir que o sistema de MFA funcione corretamente?

- [] A) Usar um app autenticador que sincronize automaticamente com o relógio da AWS.
- [] B) Usar qualquer app, pois a AWS sincroniza automaticamente.
- [] C) Colocar a data do dispositivo manualmente no futuro para evitar expiração.
- [] D) Criar dois dispositivos MFA para o mesmo usuário e usar qualquer um aleatoriamente.

---

3. Qual comando da AWS CLI devolve a identidade do usuário ou perfil autenticado, útil para verificar se você está usando credenciais temporárias?

- [] A) `aws iam get-user`
- [] B) `aws sts get-caller-identity`
- [] C) `aws sts list-session-tokens`
- [] D) `aws iam list-access-keys`

---

4. Você executou `aws sts get-session-token --serial-number arn:aws:iam::123456789012:mfa/usuario-terminal --token-code 123456 --profile usuario-mfa` e recebeu `AccessKeyId`, `SecretAccessKey` e `SessionToken`. Depois, você executou `aws configure --profile usuario-temporario` e inseriu apenas o AccessKeyId e SecretAccessKey, deixando o SessionToken em branco. Depois, `aws s3 ls --profile usuario-temporario` retornou **InvalidAccessKeyId**. Qual é a causa mais provável?

- [] A) As chaves base estão expiradas.
- [] B) O ARN do dispositivo MFA está incorreto.
- [] C) Faltou configurar o `aws_session_token` no perfil temporário.
- [] D) O perfil base `usuario-mfa` não tem permissão para listar buckets.

---

5. Qual política mínima tornaria possível que o `usuario-terminal` liste buckets S3, mas não crie, apague ou modifique objetos?

- [] A) `AmazonS3FullAccess`
- [] B) `AmazonS3ReadOnlyAccess`
- [] C) Policy custom com ações `s3:ListBucket` e `s3:GetObject` com `Effect: Allow`
- [] D) Uma política que use `Deny` explícito para todas as ações de escrita em S3

---

6. Qual seção do Well-Architected Framework está mais diretamente relacionada ao uso de MFA, políticas IAM e controle de acesso?

- [] A) Eficiência de Performance
- [] B) Segurança
- [] C) Alta Disponibilidade / Resiliência
- [] D) Otimização de Custos

---

7. Dentro do contexto de um laboratório local de IAM + STS, qual é a vantagem de usar credenciais temporárias via STS ao invés de deixar as Access Keys permanentes?

- [] A) Credenciais permanentes nunca expiram.
- [] B) Credenciais temporárias não exigem MFA.
- [] C) Credenciais temporárias reduzem risco se forem comprometidas, pois expiram automaticamente.
- [] D) STS permite usar comandos só no Console, não na CLI.

---

8. Suponha que você configurou tudo corretamente (usuário, MFA, CLI, STS) e rodou `aws sts get-caller-identity --profile usuario-temporario` retornando um ARN de usuário diferente do `usuario-terminal`. Por que isso pode acontecer?

- [] A) Porque STS gera uma nova entidade com ARN próprio para a sessão temporária.
- [] B) Porque o perfil temporário está usando credenciais base.
- [] C) Porque a política IAM base impede revelar o nome de usuário.
- [] D) Isso indica que algo está errado; o ARN deveria ser igual ao usuário original.

---

9. O Free Tier da AWS permite que suas credenciais IAM tenham permissões de criar instâncias EC2 sem custos para sempre?

- [] A) Sim, se você usar o tipo de instância t2.micro dentro dos limites de Free Tier.
- [] B) Não, porque qualquer uso de EC2 além dos serviços gratuitos especificados pode gerar custos.
- [] C) Sim, desde que você desligue a instância todas as noites.
- [] D) Sim, se você tiver MFA ativo.

---

10. Considere um cenário onde você está projetando uma arquitetura segura para compartilhar recursos entre contas (multi-conta). Qual prática do Well-Architected Framework ajuda nesse caso?

- [] A) Fornecer credenciais IAM permanentes para cada usuário em cada conta.
- [] B) Usar IAM roles via STS para delegar acesso entre contas (cross-account) e aplicar o princípio do Least Privilege.
- [] C) Copiar políticas do administrador (`AdministratorAccess`) para todas as contas.
- [] D) Usar grupos IAM globais que funcionam em todas as contas sem STS.

