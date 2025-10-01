

# ✅ Gabarito comentado — VPC, Subnets, IGWs e Route Tables

## 1. Você criou uma VPC com CIDR `10.0.0.0/16` e duas subnets. Uma tem rota para um Internet Gateway e a outra não. Qual afirmação está correta?

- [ ] A) Ambas as subnets são públicas. 
  ❌ *Errado* Subnet só é pública se tiver rota para IGW.
* [x] B) Apenas a subnet com rota para o IGW é pública.
  ✅ *Certo* Rota para IGW na route table é o que define subnet pública (além de IP público na instância para acesso efetivo).
- [ ] C) Subnets só são públicas se tiverem NAT Gateway.
  ❌ *Errado* NAT é para saída de subnets privadas.
- [ ] D) Ambas são privadas, pois precisam de NACLs para serem públicas.
  ❌ *Errado* NACL não define “pública/privada”; a rota para IGW define.

---

## 2. Qual recurso é obrigatório para que uma instância EC2 em subnet pública acesse a internet?

- [ ] A) NACL permitindo tráfego outbound.
  ❌ *Errado* Pode ajudar, mas não é o requisito “chave”.
- [ ] B) NAT Gateway configurado.
  ❌ *Errado* NAT é para instância em subnet privada sair.
* [x] C) Internet Gateway anexado à VPC e rota `0.0.0.0/0`.
  ✅ *Certo* É o componente de rede essencial. *(Obs.: também é necessário IP público na instância ou via EIP/auto-assign.)*
- [ ] D) Security Group com saída liberada.
  ❌ *Errado* Normalmente já vem liberado; sem IGW/rota não há internet.

---

## 3. O NAT Gateway é usado principalmente para:

- [ ] A) Permitir entrada da internet em instâncias privadas.
  ❌ *Errado* Não aceita conexões de entrada.
- [ ] B) Permitir saída da internet para instâncias públicas.
  ❌ *Errado* Públicas saem direto via IGW.
* [x] C) Permitir saída para a internet de instâncias privadas.
  ✅ *Certo* Padrão de egress de subnets privadas.
- [ ] D) Substituir o Internet Gateway em subnets públicas.
  ❌ *Errado*

---

## 4. Você cria uma subnet com rota `0.0.0.0/0 → IGW`. O que isso significa?

* [x] A) Todo tráfego não local vai para a internet.
  ✅ *Certo*
- [ ] B) A subnet é privada.
  ❌ *Errado*
- [ ] C) O tráfego entre subnets é bloqueado.
  ❌ *Errado* Há rota local implícita.
- [ ] D) A subnet exige NAT Gateway para funcionar.
  ❌ *Errado*

---

## 5. Qual afirmação sobre o Internet Gateway é verdadeira?

- [ ] A) É associado diretamente a uma subnet.
  ❌ *Errado* É em nível de VPC.
* [x] B) É associado a uma VPC inteira.
  ✅ *Certo*
- [ ] C) É opcional em subnets públicas.
  ❌ *Errado* Sem IGW não há internet pública.
- [ ] D) Só funciona se a VPC tiver CIDR 10.0.0.0/16.
  ❌ *Errado*

---

## 6. Uma instância em subnet privada precisa baixar pacotes da internet. Qual configuração é necessária?

- [ ] A) Adicionar IGW diretamente à subnet privada.
  ❌ *Errado* Privada não aponta para IGW.
- [ ] B) Configurar NACL com regra `Allow 0.0.0.0/0`.
  ❌ *Errado* Não resolve rota/egress.
* [x] C) Usar NAT Gateway em subnet pública e apontar rota da subnet privada para ele.
  ✅ *Certo*
- [ ] D) Associar um Elastic IP diretamente à instância privada.
  ❌ *Errado* Instância privada não tem rota pública.

---

## 7. Em uma tabela de rotas de subnet privada, qual rota você esperaria encontrar para saída à internet via NAT?

- [ ] A) `0.0.0.0/0 → IGW`
  ❌ *Errado* Seria pública.
* [x] B) `0.0.0.0/0 → NAT Gateway`
  ✅ *Certo*
- [ ] C) `10.0.0.0/16 → IGW`
  ❌ *Errado* Não faz sentido como rota externa.
- [ ] D) `::/0 → VPN Gateway`
  ❌ *Errado* Rota IPv6/VPN não atende ao caso.

---

## 8. Qual diferença entre Security Groups e NACLs é correta?

* [x] A) SGs são stateful; NACLs são stateless.
  ✅ *Certo*
- [ ] B) SGs funcionam em nível de subnet; NACLs em nível de instância.
  ❌ *Errado* É o contrário.
- [ ] C) Ambos são avaliados em ordem numérica de regra.
  ❌ *Errado* NACL tem numeração/ordem; SG não.
- [ ] D) Ambos bloqueiam todo tráfego por padrão.
  ❌ *Errado* SG padrão permite egress; NACL default permite tudo.

---

## 9. Você configurou uma subnet pública com IGW, mas a instância EC2 não acessa internet. Qual a causa mais provável?

- [ ] A) O NACL está bloqueando tráfego de saída.
  ❌ *Errado* *Possível, mas menos comum.* NACL default permite.
- [ ] B) O Security Group está liberando todas as portas.
  ❌ *Errado* *Irrelevante.* Isso não causaria falta de internet.
* [x] C) A instância não tem Elastic IP associado.
  ✅ *Certo no espírito da questão.* Para sair à internet em subnet pública, a instância precisa **IP público** (pode ser EIP ou auto-assign).
- [ ] D) A subnet precisa estar em múltiplas AZs.
  ❌ *Errado*

### 🔍 Mais Detalhes 

#### 📌 Situação

* Você tem uma **subnet pública** (tem rota `0.0.0.0/0 → IGW`).
* Criou uma **instância EC2** nela.
* Mesmo assim, ela **não acessa a internet**.

#### 🔑 O ponto-chave

Para que a instância consiga sair para a internet, três condições precisam ser verdadeiras ao mesmo tempo:

1. **VPC com IGW anexado** ✔️
   → Você já fez isso.
2. **Route Table da subnet aponta para IGW (`0.0.0.0/0 → igw-xxxx`)** ✔️
   → Também está feito.
3. **A instância precisa de um endereço IP público (ou Elastic IP) associado**
   → Esse é o ponto que costuma “travar”.

👉 **Sem IP público, a instância não consegue ser roteada para fora**.
Mesmo com IGW e rotas corretas, a internet não sabe como devolver o tráfego, porque só o IP privado (`10.0.x.x`) não é válido fora da VPC.

#### ❌ Por que não é a opção A (NACL)?

* As **NACLs padrão permitem todo tráfego (inbound/outbound)**.
* Só se você tivesse manualmente bloqueado as regras de saída é que seria o problema.
* Mas na prova, se a subnet é pública e nada foi dito sobre regras extras, **não é NACL que está bloqueando**.

#### ✅ A resposta correta é C

> **“A instância não tem Elastic IP associado.”**

* Para comunicação **de saída** (egress), instâncias em subnets públicas precisam de um **IP público** ou **Elastic IP (EIP)**.
* Sem isso, a internet não consegue rotear a volta.

#### 🧠 Dicas para lembrar na prova

1. **Público = IGW + rota + IP público.**
   → As 3 peças são obrigatórias.
2. **Privado = NAT Gateway.**
   → Se a instância não tem IP público, mas precisa sair, ela usa um NAT em subnet pública.
3. **NACL default = tudo liberado.**
   → Então, se a questão não falar que foi customizada, não culpe a NACL.
4. **Elastic IP = identidade fixa para fora.**
   → Instâncias em produção quase sempre usam EIP, para não perder conectividade em reinicializações.

---

## 10. Qual prática é recomendada para alta disponibilidade em uma arquitetura de VPC?

- [ ] A) Criar todas as subnets em uma única AZ para simplificação.
  ❌ *Errado*
- [ ] B) Usar apenas NACLs para segurança.
  ❌ *Errado*
* [x] C) Distribuir subnets em múltiplas AZs.
  ✅ *Certo* Melhora tolerância a falhas.
- [ ] D) Usar somente a Default VPC.
  ❌ *Errado*

---

## 11. O que diferencia uma subnet pública de uma privada?

- [ ] A) Subnet pública sempre tem NAT Gateway.
  ❌ *Errado*
* [x] B) Subnet pública possui rota para IGW.
  ✅ *Certo*
- [ ] C) Subnet privada não pode ter instâncias EC2.
  ❌ *Errado* Pode (e costuma ter).
- [ ] D) Subnet privada só funciona com Elastic IPs.
  ❌ *Errado*

---

## 12. Qual rota padrão garante que instâncias em subnet pública possam acessar a internet?

* [x] A) `0.0.0.0/0 → IGW`
  ✅ *Certo*
- [ ] B) `10.0.0.0/16 → IGW`
  ❌ *Errado*
- [ ] C) `0.0.0.0/0 → NAT Gateway`
  ❌ *Errado* Isso é típico de subnet privada.
- [ ] D) `::/0 → IGW`
  *Incompleto.* Fala de IPv6; questão trata do padrão IPv4.

---

## 13. Qual configuração é necessária para que duas subnets em AZs diferentes se comuniquem dentro da mesma VPC?

- [ ] A) Adicionar NAT Gateway em cada subnet.
  ❌ *Errado*
* [x] B) Garantir que ambas estão associadas à mesma tabela de rotas interna.

  ✅ *Certo* *Aceitável no contexto da questão.* Na prática, **basta a rota local implícita (local)** presente em qualquer route table da VPC e SGs permitindo. Ter a mesma route table **não é obrigatório**, mas a questão foca no roteamento.
- [ ] C) Criar peering entre as duas subnets.
  ❌ *Errado* Peering é entre VPCs.
- [ ] D) Associar Elastic IPs a cada instância.
  ❌ *Errado*

---

## 14. Você precisa restringir acesso a toda uma subnet. Qual recurso é mais adequado?

- [ ] A) Security Groups.
  ❌ *Errado* SG atua na instância.
- [ ] B) IAM Policies.
  ❌ *Errado* IAM não controla tráfego de rede IP.
* [x] C) Network ACLs.
  ✅ *Certo* Atua no nível da subnet.
- [ ] D) Route Tables.
  ❌ *Errado* Rotas não fazem filtragem por portas/protocolos.

---

## 15. Quais endereços IP são sempre reservados dentro de uma subnet?

- [ ] A) O primeiro e último IP do range.
  *Parcial.* Há mais reservados.
- [ ] B) Os três primeiros IPs apenas.
  ❌ *Errado*
* [x] C) Os quatro primeiros e o último IP.
  ✅ *Certo* (5 IPs por subnet são reservados.)
- [ ] D) Nenhum, todos podem ser usados.
  ❌ *Errado*

### 🔍 Mais Detalhes 

#### 🔎 Por que são 5 IPs reservados?

Quando você cria uma subnet dentro de uma VPC, a AWS automaticamente reserva **5 endereços IP por subnet**. Isso acontece porque a AWS precisa de alguns desses IPs para gerenciamento interno de rede.

Esses 5 IPs têm funções específicas:

1. **Primeiro endereço (network address)**

   * Exemplo: `10.0.0.0/24 → 10.0.0.0`
   * **Função:** Identifica a rede. É um conceito clássico de redes (não pode ser atribuído a host).

2. **Segundo endereço (VPC router)**

   * Exemplo: `10.0.0.1`
   * **Função:** Usado como o **gateway padrão** da subnet. Todas as instâncias usam esse IP para sair da subnet.

3. **Terceiro endereço (AWS reserved)**

   * Exemplo: `10.0.0.2`
   * **Função:** Reservado pela AWS para DNS dentro da VPC.
   * Esse IP responde como **AmazonProvidedDNS**, usado por instâncias que precisam resolver nomes (ex.: `ec2.amazonaws.com`).

4. **Quarto endereço (future use)**

   * Exemplo: `10.0.0.3`
   * **Função:** Mantido reservado pela AWS para **possíveis funcionalidades futuras** (não pode ser usado por você).

5. **Último endereço (broadcast address)**

   * Exemplo: `10.0.0.255` (em um `/24`)
   * **Função:** Usado tradicionalmente como broadcast em redes IP.
   * A AWS não implementa broadcast, mas reserva esse endereço mesmo assim.

#### 🧠 Dicas para lembrar na prova

* **Regra fixa:** sempre **5 IPs por subnet**.
* **Fórmula:** `total de IPs da subnet – 5 = IPs disponíveis`.

  * Exemplo: `/24 = 256 IPs` → `256 - 5 = 251 utilizáveis`.
  * Exemplo: `/28 = 16 IPs` → `16 - 5 = 11 utilizáveis`.
* **Memorizar assim:**

  * 1 = network
  * 1 = gateway (10.x.x.1)
  * 1 = DNS (10.x.x.2)
  * 1 = reservado futuro (10.x.x.3)
  * 1 = broadcast

---

## 16. O que acontece se duas VPCs com blocos CIDR sobrepostos tentarem criar um VPC Peering?

- [ ] A) Peering é criado, mas tráfego é bloqueado.
  ❌ *Errado*
* [x] B) Peering é recusado automaticamente.
  ✅ *Certo*
- [ ] C) Peering funciona apenas em subnets públicas.
  ❌ *Errado*
- [ ] D) O tráfego é redirecionado para NAT Gateway.
  ❌ *Errado*

---

## 17. Qual vantagem principal de uma Custom VPC sobre a Default VPC?

- [ ] A) A Default VPC não suporta instâncias EC2.
  ❌ *Errado*
* [x] B) A Custom VPC permite controle total sobre CIDR, subnets e rotas.
  ✅ *Certo*
- [ ] C) A Default VPC não pode ser usada em múltiplas regiões.
  ❌ *Errado* Cada região tem sua default.
- [ ] D) A Custom VPC é gratuita e a Default é cobrada.
  ❌ *Errado*

---

## 18. Qual afirmação sobre rotas locais em VPCs é correta?

- [ ] A) Rotas locais só existem em subnets públicas.
  ❌ *Errado* Existem em qualquer route table da VPC.
* [x] B) Rotas locais permitem comunicação entre subnets da mesma VPC.
  ✅ *Certo*
- [ ] C) Rotas locais são removidas se a subnet for privada.
  ❌ *Errado*
- [ ] D) Rotas locais precisam ser configuradas manualmente.
  ❌ *Errado* São automáticas.

---

## 19. Em qual situação você usaria múltiplas tabelas de rotas em uma VPC?

* [x] A) Para dividir tráfego entre subnets públicas e privadas.
  ✅ *Certo* Cada grupo pode apontar para destinos diferentes (IGW vs NAT).
- [ ] B) Para limitar tráfego em Security Groups.
  ❌ *Errado* SG ≠ rotas.
- [ ] C) Para permitir que NACLs tenham regras diferentes.
  ❌ *Errado* NACL independe de route tables.
- [ ] D) Para permitir que a Default VPC funcione.
  ❌ *Errado*

---

## 20. Uma instância em subnet privada precisa acessar o serviço S3 de forma segura e sem passar pela internet. Qual recurso usar?

- [ ] A) Elastic IP associado à instância.
  ❌ *Errado* Tornaria pública.
* [x] B) VPC Gateway Endpoint para S3.
  ✅ *Certo* Tráfego privado para S3 sem internet.
- [ ] C) NAT Gateway.
  *Parcial.* Funciona, mas usa internet pública e custa mais; não atende “sem passar pela internet”.
- [ ] D) Internet Gateway com rota privada.
  ❌ *Errado* IGW é internet pública.
