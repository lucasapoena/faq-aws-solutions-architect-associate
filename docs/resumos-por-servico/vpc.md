# 🏙️ AWS VPC — Virtual Private Cloud

VPC (Virtual Private Cloud) é uma rede virtual lógica dentro da AWS, isolada, onde você pode executar recursos como EC2, RDS etc.

Ele permite criar uma rede **isolada e personalizada** dentro da AWS, onde você controla **endereços IP, sub-redes, rotas e regras de segurança** para os recursos implantados.

👉 Pense na VPC como o **“terreno” da sua aplicação na nuvem**: você define os limites, ruas, entradas e saídas.

- Você define o **CIDR block** (intervalo de IPs), sub-redes, zonas de disponibilidade (AZs) e controla tráfego de rede.  
- Cada região pode ter múltiplas VPCs, até alguns limites (soft limits).

---

## 🔑 Conceito básico

- Cada conta AWS já vem com uma **Default VPC** (pronta para uso).  
- Você pode criar suas próprias VPCs (**Custom VPCs**) para ter controle total sobre:  
  - Faixa de endereços IP (CIDR block).  
  - Divisão em sub-redes (públicas ou privadas).  
  - Gateways de acesso (Internet Gateway, NAT Gateway).  
  - Rotas para direcionar o tráfego.  
  - Camadas de segurança (Security Groups e Network ACLs).

---

## 📌 Elementos principais de uma VPC

| Elemento | Explicação simples | O que a prova pode cobrar |
|----------|-------------------|---------------------------|
| **CIDR Block** | Intervalo de endereços IP que sua VPC vai usar (ex.: 10.0.0.0/16). | Saber escolher tamanho adequado (quantidade de IPs) e lembrar que não pode haver sobreposição em peering/VPN. |
| **Subnets** | Dividem a VPC em partes menores. Cada subnet fica dentro de uma única AZ. | Distinguir **pública** (rota para IGW) de **privada** (sem acesso direto à internet). |
| **Route Tables** | Regras que dizem para onde o tráfego deve ir (ex.: internet, NAT, endpoint). | Entender rotas padrão `0.0.0.0/0` e rotas privadas via NAT. |
| **Internet Gateway (IGW)** | Permite que subnets públicas tenham acesso à internet. | Perguntas comuns: instância não acessa internet → faltou IGW ou rota correta. |
| **NAT Gateway** | Dá acesso de saída à internet para instâncias em subnets privadas. | Importante: só saída, não entrada; gera custo; precisa de 1 por AZ para alta disponibilidade. |
| **Security Groups (SG)** | Firewall de nível de instância. Regras **stateful** (respostas são automáticas). | Saber que SGs permitem por padrão **outbound all traffic**. |
| **Network ACLs (NACLs)** | Firewall em nível de subnet. Regras **stateless** (precisa permitir entrada e saída). | Questões de prova com foco em bloquear tráfego em subnets inteiras. |
| **VPC Peering** | Conecta duas VPCs para comunicação interna. | Atenção: não suporta CIDR sobreposto; precisa configurar rotas. |
| **VPC Endpoints** | Permite acessar serviços AWS (S3, DynamoDB etc.) sem passar pela internet. | Saber diferenciar **Gateway Endpoint (S3, DynamoDB)** e **Interface Endpoint (outros via PrivateLink)**. |