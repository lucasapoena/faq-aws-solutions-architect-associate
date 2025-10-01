# ❓ Questões de Revisão — VPC, Subnets, IGWs e Route Tables

### 1.  
Você criou uma VPC com CIDR `10.0.0.0/16` e duas subnets. Uma tem rota para um Internet Gateway e a outra não. Qual afirmação está correta?

- A) Ambas as subnets são públicas.  
- B) Apenas a subnet com rota para o IGW é pública.  
- C) Subnets só são públicas se tiverem NAT Gateway.  
- D) Ambas são privadas, pois precisam de NACLs para serem públicas.  

---

### 2.  
Qual recurso é obrigatório para que uma instância EC2 em subnet pública acesse a internet?

- A) NACL permitindo tráfego outbound.  
- B) NAT Gateway configurado.  
- C) Internet Gateway anexado à VPC e rota `0.0.0.0/0`.  
- D) Security Group com saída liberada.  

---

### 3.  
O NAT Gateway é usado principalmente para:

- A) Permitir entrada da internet em instâncias privadas.  
- B) Permitir saída da internet para instâncias públicas.  
- C) Permitir saída para a internet de instâncias privadas.  
- D) Substituir o Internet Gateway em subnets públicas.  

---

### 4.  
Você cria uma subnet com rota `0.0.0.0/0 → IGW`. O que isso significa?

- A) Todo tráfego não local vai para a internet.  
- B) A subnet é privada.  
- C) O tráfego entre subnets é bloqueado.  
- D) A subnet exige NAT Gateway para funcionar.  

---

### 5.  
Qual afirmação sobre o Internet Gateway é verdadeira?

- A) É associado diretamente a uma subnet.  
- B) É associado a uma VPC inteira.  
- C) É opcional em subnets públicas.  
- D) Só funciona se a VPC tiver CIDR 10.0.0.0/16.  

---

### 6.  
Uma instância em subnet privada precisa baixar pacotes da internet. Qual configuração é necessária?

- A) Adicionar IGW diretamente à subnet privada.  
- B) Configurar NACL com regra `Allow 0.0.0.0/0`.  
- C) Usar NAT Gateway em subnet pública e apontar rota da subnet privada para ele.  
- D) Associar um Elastic IP diretamente à instância privada.  

---

### 7.  
Em uma tabela de rotas de subnet privada, qual rota você esperaria encontrar para saída à internet via NAT?

- A) `0.0.0.0/0 → IGW`  
- B) `0.0.0.0/0 → NAT Gateway`  
- C) `10.0.0.0/16 → IGW`  
- D) `::/0 → VPN Gateway`  

---

### 8.  
Qual diferença entre Security Groups e NACLs é correta?

- A) SGs são stateful; NACLs são stateless.  
- B) SGs funcionam em nível de subnet; NACLs em nível de instância.  
- C) Ambos são avaliados em ordem numérica de regra.  
- D) Ambos bloqueiam todo tráfego por padrão.  

---

### 9.  
Você configurou uma subnet pública com IGW, mas a instância EC2 não acessa internet. Qual a causa mais provável?

- A) O NACL está bloqueando tráfego de saída.  
- B) O Security Group está liberando todas as portas.  
- C) A instância não tem Elastic IP associado.  
- D) A subnet precisa estar em múltiplas AZs.  

---

### 10.  
Qual prática é recomendada para alta disponibilidade em uma arquitetura de VPC?

- A) Criar todas as subnets em uma única AZ para simplificação.  
- B) Usar apenas NACLs para segurança.  
- C) Distribuir subnets em múltiplas AZs.  
- D) Usar somente a Default VPC.  

---

### 11.  
O que diferencia uma subnet pública de uma privada?

- A) Subnet pública sempre tem NAT Gateway.  
- B) Subnet pública possui rota para IGW.  
- C) Subnet privada não pode ter instâncias EC2.  
- D) Subnet privada só funciona com Elastic IPs.  

---

### 12.  
Qual rota padrão garante que instâncias em subnet pública possam acessar a internet?

- A) `0.0.0.0/0 → IGW`  
- B) `10.0.0.0/16 → IGW`  
- C) `0.0.0.0/0 → NAT Gateway`  
- D) `::/0 → IGW`  

---

### 13.  
Qual configuração é necessária para que duas subnets em AZs diferentes se comuniquem dentro da mesma VPC?

- A) Adicionar NAT Gateway em cada subnet.  
- B) Garantir que ambas estão associadas à mesma tabela de rotas interna.  
- C) Criar peering entre as duas subnets.  
- D) Associar Elastic IPs a cada instância.  

---

### 14.  
Você precisa restringir acesso a toda uma subnet. Qual recurso é mais adequado?

- A) Security Groups.  
- B) IAM Policies.  
- C) Network ACLs.  
- D) Route Tables.  

---

### 15.  
Quais endereços IP são sempre reservados dentro de uma subnet?

- A) O primeiro e último IP do range.  
- B) Os três primeiros IPs apenas.  
- C) Os quatro primeiros e o último IP.  
- D) Nenhum, todos podem ser usados.  

---

### 16.  
O que acontece se duas VPCs com blocos CIDR sobrepostos tentarem criar um VPC Peering?

- A) Peering é criado, mas tráfego é bloqueado.  
- B) Peering é recusado automaticamente.  
- C) Peering funciona apenas em subnets públicas.  
- D) O tráfego é redirecionado para NAT Gateway.  

---

### 17.  
Qual vantagem principal de uma Custom VPC sobre a Default VPC?

- A) A Default VPC não suporta instâncias EC2.  
- B) A Custom VPC permite controle total sobre CIDR, subnets e rotas.  
- C) A Default VPC não pode ser usada em múltiplas regiões.  
- D) A Custom VPC é gratuita e a Default é cobrada.  

---

### 18.  
Qual afirmação sobre rotas locais em VPCs é correta?

- A) Rotas locais só existem em subnets públicas.  
- B) Rotas locais permitem comunicação entre subnets da mesma VPC.  
- C) Rotas locais são removidas se a subnet for privada.  
- D) Rotas locais precisam ser configuradas manualmente.  

---

### 19.  
Em qual situação você usaria múltiplas tabelas de rotas em uma VPC?

- A) Para dividir tráfego entre subnets públicas e privadas.  
- B) Para limitar tráfego em Security Groups.  
- C) Para permitir que NACLs tenham regras diferentes.  
- D) Para permitir que a Default VPC funcione.  

---

### 20.  
Uma instância em subnet privada precisa acessar o serviço S3 de forma segura e sem passar pela internet. Qual recurso usar?

- A) Elastic IP associado à instância.  
- B) VPC Gateway Endpoint para S3.  
- C) NAT Gateway.  
- D) Internet Gateway com rota privada.  