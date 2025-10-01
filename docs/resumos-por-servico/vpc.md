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


# 📊 Endereços IP reservados em subnets AWS

Quando criamos uma subnet em uma **VPC da AWS**, **5 endereços IP são sempre reservados** e não podem ser atribuídos a instâncias:

- 1️⃣ **Primeiro endereço** → identificador da rede (ex.: `10.0.0.0`)  
- 2️⃣ **Segundo endereço** → roteador da VPC (gateway padrão da subnet, ex.: `10.0.0.1`)  
- 3️⃣ **Terceiro endereço** → DNS interno da AWS (ex.: `10.0.0.2`)  
- 4️⃣ **Quarto endereço** → reservado para uso futuro da AWS (ex.: `10.0.0.3`)  
- 5️⃣ **Último endereço** → broadcast (ex.: `10.0.0.255`)  

---

## 📌 Exemplos práticos

| Subnet CIDR | Total de IPs | IPs Reservados | IPs Utilizáveis | Observações |
|-------------|--------------|----------------|-----------------|-------------|
| `/24` (ex.: 10.0.0.0/24) | 256 | 5 | **251** | Mais comum em labs; útil para até ~250 instâncias |
| `/28` (ex.: 10.0.0.0/28) | 16 | 5 | **11** | Útil em subnets pequenas; limite reduzido pode atrapalhar escalabilidade |
| `/16` (ex.: 10.0.0.0/16) | 65.536 | 5 | **65.531** | Usado em ambientes grandes, mas ainda perde 5 IPs fixos |

---

## 🧠 Dica de prova
- Sempre **subtraia 5 do total de IPs** calculados pela máscara da subnet.  
- Fórmula: `2^(32 - máscara) – 5 = IPs utilizáveis`  

Exemplo rápido:  
- `/24` → `2^(32-24) = 256 – 5 = 251`  
- `/28` → `2^(32-28) = 16 – 5 = 11`  

📌 **Regra fixa:** não importa o tamanho da subnet, a AWS sempre reserva 5 endereços.