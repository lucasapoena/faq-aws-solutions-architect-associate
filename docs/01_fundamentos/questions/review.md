### ✅ Review das Perguntas — Seção Fundamentos

1. Um usuário IAM chamado `usuario-terminal` foi criado com acesso programático. Ele tem permissão apenas de leitura (ReadOnlyAccess) e nenhuma permissão para console. Qual comportamento abaixo está correto quando esse usuário tenta fazer login no Console da AWS?

   * [ ] A) Ele pode fazer login normalmente, mas verá apenas opções de leitura.
     *Errado* — sem senha de Console, o usuário não poderá fazer login via interface web.
   * [x] B) Ele não pode fazer login no Console AWS, pois não foi criada senha de Console.
     *Certo* — o acesso programático não cria credenciais de console (senha/login).
   * [ ] C) Ele pode fazer login no Console se usar a mesma Access Key + Secret.
     *Errado* — Access Key + Secret são para CLI / APIs, não para login web.
   * [ ] D) Ele pode fazer login apenas se ativar MFA primeiro.
     *Errado* — ativar MFA pressupõe que já exista credencial de console; aqui não há senha para login.

---

2. Ao configurar um dispositivo MFA virtual para `usuario-terminal`, qual das seguintes práticas é importante para garantir que o sistema de MFA funcione corretamente?

   * [x] A) Usar um app autenticador que sincronize automaticamente com o relógio da AWS.
     *Certo* — apps cujos relógios estão sincronizados geram códigos válidos; se estiverem fora, os códigos podem falhar.
   * [ ] B) Usar qualquer app, pois a AWS sincroniza automaticamente.
     *Errado* — a sincronização depende do app/relógio local; AWS não ajusta relógio de dispositivo do usuário.
   * [ ] C) Colocar a data do dispositivo manualmente no futuro para evitar expiração.
     *Errado* — definir data ao futuro cria mais problemas (códigos inválidos, sincronização ruim).
   * [ ] D) Criar dois dispositivos MFA para o mesmo usuário e usar qualquer um aleatoriamente.
     *Errado* — múltiplos dispositivos podem causar confusão; prática comum é ter um, ou backup.

---

3. Qual comando da AWS CLI devolve a identidade do usuário ou perfil autenticado, útil para verificar se você está usando credenciais temporárias?

   * [ ] A) `aws iam get-user`
     *Errado* — retorna informações de usuário IAM persistente; não confirma credenciais temporárias usadas no momento.
   * [x] B) `aws sts get-caller-identity`
     *Certo* — retorna ARN / ID da entidade autenticada (usuário ou sessão STS).
   * [ ] C) `aws sts list-session-tokens`
     *Errado* — não existe esse comando.
   * [ ] D) `aws iam list-access-keys`
     *Errado* — lista chaves permanentes, não verifica sessão ativa.

---

4. Você executou `aws sts get-session-token --serial-number arn:aws:iam::123456789012:mfa/usuario-terminal --token-code 123456 --profile usuario-mfa` e recebeu `AccessKeyId`, `SecretAccessKey` e `SessionToken`. Depois, você executou `aws configure --profile usuario-temporario` e inseriu apenas o AccessKeyId e SecretAccessKey, deixando o SessionToken em branco. Depois, `aws s3 ls --profile usuario-temporario` retornou **InvalidAccessKeyId**. Qual é a causa mais provável?

   * [ ] A) As chaves base estão expiradas.
     *Errado* — se fosse base, o erro seria diferente; e o comando `get-session-token` retornou chaves novas.
   * [ ] B) O ARN do dispositivo MFA está incorreto.
     \*Possível — mas se `get-session-token` funcionou, o ARN estava correto.
   * [x] C) Faltou configurar o `aws_session_token` no perfil temporário.
     *Certo* — sem o session token, AWS não aceita as credenciais temporárias.
   * [ ] D) O perfil base `usuario-mfa` não tem permissão para listar buckets.
     *Errado* — se o perfil base tivesse problema, `get-session-token` falharia ou `aws sts get-caller-identity` falharia.

---

5. Qual política mínima tornaria possível que o `usuario-terminal` liste buckets S3, mas não crie, apague ou modifique objetos?

   * [ ] A) `AmazonS3FullAccess`
     *Errado* — dá permissões de escrita e exclusão também.
   * [x] B) `AmazonS3ReadOnlyAccess`
     *Certo* — permite listar buckets e ler objetos, mas nada de gravação ou modificações.
   * [ ] C) Policy custom com ações `s3:ListBucket` e `s3:GetObject` com `Effect: Allow`
     *Certo tecnicamente também,* mas é uma alternativa customizada. Se considerar “política mínima”, essa opção serve, mas no simulado o padrão geralmente é usar role ou política já existente.
   * [ ] D) Uma política que use `Deny` explícito para todas as ações de escrita em S3
     *Errado* — pode funcionar, mas políticas com `Deny` explícito precisam ser bem cuidadas; além disso, se há outras policies `Allow`, o `Deny` explícito sobrescreve, mas esse cenário é mais complexo.

---

6. Qual seção do Well-Architected Framework está mais diretamente relacionada ao uso de MFA, políticas IAM e controle de acesso?

   * [ ] A) Eficiência de Performance
     *Errado* — performance fala de latência, escalabilidade, throughput.
   * [x] B) Segurança
     *Certo* — segurança cobre autenticação, autorização, criptografia, práticas de controle.
   * [ ] C) Alta Disponibilidade / Resiliência
     *Errado* — esse pilar cuida de falhas, tolerância a erros.
   * [ ] D) Otimização de Custos
     *Errado* — custo fala de otimização financeira, não de controle de identidade.

---

7. Dentro do contexto de um laboratório local de IAM + STS, qual é a vantagem de usar credenciais temporárias via STS ao invés de deixar as Access Keys permanentes?

   * [ ] A) Credenciais permanentes nunca expiram.
     *Errado* — elas não expiram, mas representam risco maior se expostas.
   * [ ] B) Credenciais temporárias não exigem MFA.
     *Errado* — pode exigir MFA dependendo da configuração.
   * [x] C) Credenciais temporárias reduzem risco se forem comprometidas, pois expiram automaticamente.
     *Certo* — se forem roubadas, o dano é limitado ao tempo de validade.
   * [ ] D) STS permite usar comandos só no Console, não na CLI.
     *Errado* — STS funciona para CLI / SDK / APIs.

---

8. Suponha que você configurou tudo corretamente (usuário, MFA, CLI, STS) e rodou `aws sts get-caller-identity --profile usuario-temporario` retornando um ARN de usuário diferente do `usuario-terminal`. Por que isso pode acontecer?

   * [x] A) Porque STS gera uma nova entidade com ARN próprio para a sessão temporária.
     *Certo* — credenciais temporárias geralmente geram ARN com sufixo ou algo indicando “session/...”.
   * [ ] B) Porque o perfil temporário está usando credenciais base.
     *Errado* — o comando diferiria se estivesse usando base.
   * [ ] C) Porque a política IAM base impede revelar o nome de usuário.
     *Errado* — política não altera o ARN retornado por `get-caller-identity`.
   * [ ] D) Isso indica que algo está errado; o ARN deveria ser igual ao usuário original.
     *Errado* — diferença de ARN é esperada quando se usa sessão temporária.

---

9. O Free Tier da AWS permite que suas credenciais IAM tenham permissões de criar instâncias EC2 sem custos para sempre?

   * [ ] A) Sim, se você usar o tipo de instância t2.micro dentro dos limites de Free Tier.
     *Errado* — embora haja instâncias elegíveis ao Free Tier, há limites e uso além gera custos.
   * [x] B) Não, porque qualquer uso de EC2 além dos serviços gratuitos especificados pode gerar custos.
     *Certo* — Free Tier tem limites; ultrapassar ou usar serviços fora dele gera cobrança.
   * [ ] C) Sim, desde que você desligue a instância todas as noites.
     *Errado* — desligar não cobre todos os custos (storage, transferência, etc.), e uso acumulado conta.
   * [ ] D) Sim, se você tiver MFA ativo.
     *Errado* — MFA nada a ver com custos, serve para segurança.

---

10. Considere um cenário onde você está projetando uma arquitetura segura para compartilhar recursos entre contas (multi-conta). Qual prática do Well-Architected Framework ajuda nesse caso?

    * [ ] A) Fornecer credenciais IAM permanentes para cada usuário em cada conta.
      *Errado* — credenciais permanentes aumentam o risco de comprometimento.
    * [x] B) Usar IAM roles via STS para delegar acesso entre contas (cross-account) e aplicar o princípio do Least Privilege.
      *Certo* — roles + STS permitem acesso controlado e temporário em outra conta, bo e prática de segurança.
    * [ ] C) Copiar políticas do administrador (`AdministratorAccess`) para todas as contas.
      *Errado* — isso dá permissões amplas demais.
    * [ ] D) Usar grupos IAM globais que funcionam em todas as contas sem STS.
      *Errado* — grupos IAM não funcionam entre contas, não substituem roles/federation/STS para acesso entre contas.
