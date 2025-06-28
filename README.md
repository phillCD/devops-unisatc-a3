---

# 🚀 Projeto DevOps A3 — Strapi CMS

Este repositório tem como finalidade apresentar uma implementação completa de CI/CD utilizando **GitHub Actions**, **Docker** e **Terraform** com o **Strapi CMS**, conforme solicitado na atividade A3 da disciplina de DevOps.

---

## 📦 Tecnologias empregadas

* **Strapi CMS** (v5)
* **Docker**
* **Docker Hub**
* **GitHub Actions**
* **Terraform**
* **AWS ECS Fargate**
* **Playwright** (para testes end-to-end)
* **Node.js 18 + pnpm**
* **SQLite** (armazenado em volume do container)

---

## ⚙️ Recursos disponíveis

* Integração e entrega contínua com GitHub Actions
* Publicação automatizada na AWS (ECS + Fargate)
* Criação e envio de imagens Docker
* Testes automatizados de ponta a ponta com Playwright
* Utilização segura de variáveis sensíveis via GitHub Secrets
* Persistência dos dados do banco por volume local

---

## 📁 Organização do projeto

```
.
├── .github/
│   └── workflows/
│       ├── deploy.yml         # Workflow de Deploy
│       └── tests.yml          # Workflow de Testes
├── terraform/
│   ├── main.tf                # Definição da infraestrutura na AWS
│   └── terraform.tfvars       # Variáveis da infraestrutura (geradas na pipeline)
├── tests/
│   └── e2e.spec.ts            # Roteiros de teste com Playwright
├── .env                       # Arquivo de variáveis (não versionado)
├── Dockerfile                 # Instruções para gerar a imagem
└── README.md
```

---

## 🚀 Executando localmente

1. Crie o arquivo `.env` com os seguintes valores:

```
APP_KEYS=...
API_TOKEN_SALT=...
ADMIN_JWT_SECRET=...
JWT_SECRET=...
DATABASE_FILENAME=.tmp/data.db
```

2. Execute a aplicação com Docker:

```bash
docker build -t strapi-a3 .
docker run -p 1337:1337 --env-file .env -v ${PWD}/.tmp:/app/.tmp strapi-a3
```

Para usuários Windows (PowerShell):

```powershell
docker run -p 1337:1337 --env-file .env -v ${PWD}/.tmp:/app/.tmp strapi-a3
```

---

## ✅ Testes automatizados

* Desenvolvidos com **Playwright**
* Executados automaticamente via GitHub Actions em pull requests
* Verificam o funcionamento de ao menos duas coleções no CMS

### Executar localmente

```bash
pnpm install
pnpm exec playwright install
pnpm exec playwright test
```

---

## 🧪 Workflows no GitHub Actions

### `tests.yml` (Pull Requests)

* Disparado em eventos de `pull_request` na branch `develop`
* Realiza a instalação de dependências
* Inicializa o Strapi
* Executa os testes automatizados

### `deploy.yml` (Deploy)

* Executado ao realizar `push` na branch `develop`
* Constrói e envia a imagem para o Docker Hub
* Aplica a infraestrutura via `terraform apply` na AWS

---

## ☁️ Secrets necessários no GitHub

| Nome                     | Finalidade                            |
| ------------------------ | ------------------------------------- |
| DOCKER\_USERNAME         | Usuário do Docker Hub                 |
| DOCKER\_PASSWORD         | Senha da conta Docker                 |
| APP\_KEYS                | Chaves de aplicação do Strapi         |
| API\_TOKEN\_SALT         | Salt para geração de tokens           |
| ADMIN\_JWT\_SECRET       | JWT do painel administrativo          |
| JWT\_SECRET              | JWT geral da aplicação                |
| DATABASE\_FILENAME       | Caminho para o banco SQLite           |
| AWS\_ACCESS\_KEY\_ID     | Chave de acesso IAM da AWS            |
| AWS\_SECRET\_ACCESS\_KEY | Chave secreta da IAM da AWS           |
| ECS\_EXECUTION\_ROLE     | Permissão de execução no ECS          |
| ECS\_TASK\_ROLE          | Permissão da Task ECS                 |
| SUBNET\_IDS              | Lista de sub-redes da VPC             |
| SECURITY\_GROUP\_ID      | ID do grupo de segurança (porta 1337) |

---

## 📌 Considerações

* Kubernetes não foi utilizado neste projeto.
* O banco de dados SQLite é armazenado via volume com `-v ./tmp:/app/.tmp`.
* O processo de deploy é totalmente automatizado via Terraform na pipeline.

---

## 👨‍💻 Acesso ao painel do Strapi

* **Endereço**: [http://localhost:1337](http://localhost:1337)
* **Conta administrativa**:

  * Email: [admin@satc.edu.br](mailto:admin@satc.edu.br)
  * Senha: welcomeToStrapi123

---

## 🧠 Conclusão

Este projeto serve como exemplo prático de como implementar uma pipeline completa de DevOps, englobando microsserviços, automação de testes, uso de containers e provisionamento de infraestrutura como código. 

---