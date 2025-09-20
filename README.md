# Roadmap de Estudos — AWS Certified Solutions Architect Associate (SAA-C03)

Este repositório documenta meu estudo para a certificação **AWS Certified Solutions Architect – Associate (SAA-C03)**

---

## 🔍 Informações do exame

- Nome: AWS Certified Solutions Architect – Associate (SAA-C03)  
- Peso dos domínios:  
  - Design de Arquiteturas Seguras: ~30%
  - Arquiteturas Resilientes: ~26%
  - Alta performance: ~24%
  - Otimização de custo: ~20%

---

## 📆 Estrutura de Estudo

- **Metodologia**:
  1. Assistir aulas teóricas
  2. Executar laboratórios práticos (quando aplicável)
  3. Registrar notas em `/docs`
  4. Criar/resolver Issues correspondentes
  5. Revisar e aplicar simulados

---

## 🔍 Roadmap

### Fundamentos da AWS
- Console, CLI e CloudShell
- Free Tier (antigo e novo)
- IAM (usuários, grupos, políticas, MFA, STS)
- Well-Architected Framework

### Redes e Conectividade
- VPC, subnets, route tables, IGWs
- Segurança: NACLs, SGs, Bastion, Flow Logs
- NAT Gateway
- VPC Peering, Endpoints, VPN, Direct Connect

### Armazenamento
- S3 (classes, versionamento, lifecycle, replicação, segurança avançada)
- Labs: buckets, políticas, site estático, CloudFront + OAC
- EFS, FSx, Snowball, Storage Gateway

### Computação
- EC2: tipos, AMIs, keypairs, volumes EBS
- Elastic Load Balancing + Auto Scaling
- ECS vs EKS vs Lambda (visão geral)

### Bancos de Dados
- RDS, Aurora, DynamoDB
- Redshift, ElastiCache
- Aurora Serverless, Global DB
- DocumentDB e Keyspaces (extra)

### Segurança e Identidade
- IAM Policies, Roles, Federation
- Cognito, SSO
- KMS, Secrets Manager

### Observabilidade e Monitoramento
- CloudWatch Logs/Metrics/Dashboards
- CloudTrail e Config
- Trusted Advisor

### Serviços Avançados
- SQS, SNS, EventBridge
- Step Functions, SWF
- Parameter Store, AppConfig, Systems Manager

### Arquitetura e Design
- Alta disponibilidade e recuperação de desastres
- Multi-Region e Global Services
- Trade-offs entre custo e performance
- Edge Computing (Outposts)

### Preparação para Exame
- Estratégias de prova
- Questões comentadas
- Revisão final e simulados

### Simulados

---

## 📚 Materiais de Referência

- [AWS Exam Guide SAA-C03 (oficial)](https://d1.awsstatic.com/training-and-certification/docs-sa-assoc/AWS-Certified-Solutions-Architect-Associate_Exam-Guide.pdf)
- [AWS Well-Architected Framework](https://aws.amazon.com/architecture/well-architected/)
- Documentação oficial AWS (IAM, VPC, S3, EC2, etc.)
- Cursos:
  - [Certificação AWS Solutions Architect Associate SAA-C03 PT-BR](https://www.udemy.com/course/certificacao-aws-solutions-architect-associate-saa-c03-curso) 

---

## 🔄 Progresso

- Usar **Issues** para cada aula/lab/simulado, marcar quando concluído  
- Um **Project** tipo Kanban (“A Fazer”, “Em Progresso”, “Revisar”, “Concluído”)  
- Badges de progresso (ex: % concluído, semanas concluídas)  