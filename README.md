# Tech Challenge FIAP Workflows

Repositório de workflows reutilizáveis do GitHub Actions para o projeto FIAP Challenge.

## 📋 Sobre

Este repositório centraliza workflows reutilizáveis que podem ser chamados por outros repositórios da organização, promovendo: 

- ✅ Padronização de processos de CI/CD
- ♻️ Reutilização de código
- 🔧 Facilidade de manutenção
- 🚀 Automação de deploy e análise de qualidade

## 🔄 Workflows Disponíveis

### 1. Quality Analysis (`quality-analysis.yaml`)

Workflow para análise de qualidade de código Java com SonarCloud e JaCoCo.

**Características:**
- Execução de testes unitários com Maven
- Verificação de cobertura de código com JaCoCo (mínimo 80%)
- Análise de código com SonarCloud
- Validação de cobertura em Pull Requests (mínimo 70% para código novo)

**Exemplo de uso:**

```yaml
name: Quality Check

on:
  pull_request: 
  push: 
    branches: [main, master]

jobs:
  quality: 
    uses: fiap-challenge-group-52/challenge-fiap-workflows/.github/workflows/quality-analysis.yaml@main
    with:
      java-version: "21"
    secrets:
      SONAR_TOKEN: ${{ secrets.SONAR_TOKEN }}
```

**Parâmetros:**
- `java-version` (opcional): Versão do Java a ser utilizada.  Padrão: "21"

**Secrets necessários:**
- `SONAR_TOKEN`: Token de autenticação do SonarCloud

**Variáveis necessárias:**
- `SONAR_HOST_URL`: URL do SonarCloud
- `SONAR_ORG_KEY`: Chave da organização no SonarCloud
- `SONAR_PROJECT_KEY`: Chave do projeto no SonarCloud

---

### 2. Build, Push to ECR & Deploy to EKS (`build-push-ecr.yaml`)

Workflow para build de imagem Docker, push para Amazon ECR e deploy no EKS.

**Características:**
- Build de imagem Docker
- Push para Amazon ECR
- Deploy automatizado no Amazon EKS
- Configuração automática de kubeconfig

**Exemplo de uso:**

```yaml
name: Deploy to Production

on:
  push: 
    branches: [main]

jobs:
  deploy:
    uses: fiap-challenge-group-52/challenge-fiap-workflows/.github/workflows/build-push-ecr.yaml@main
    with:
      ref: 'main'
    secrets:
      AWS_ACCESS_KEY_ID: ${{ secrets.AWS_ACCESS_KEY_ID }}
      AWS_SECRET_ACCESS_KEY: ${{ secrets.AWS_SECRET_ACCESS_KEY }}
      AWS_SESSION_TOKEN: ${{ secrets.AWS_SESSION_TOKEN }}
```

**Parâmetros:**
- `ref` (opcional): Branch ou commit a ser utilizado. Padrão: "master"

**Secrets necessários:**
- `AWS_ACCESS_KEY_ID`: Access Key ID da AWS
- `AWS_SECRET_ACCESS_KEY`: Secret Access Key da AWS
- `AWS_SESSION_TOKEN`: Session Token da AWS

**Requisitos do projeto:**
- Arquivo `docker/Dockerfile` na raiz do repositório
- Manifests Kubernetes na pasta `k8s/`
- Repositório ECR com nome `lanchonete-app`
- Cluster EKS com nome `fiap-lanchonete-eks` na região `us-east-1`

---

## 🚀 Como Usar

### Pré-requisitos

1. **Secrets configurados**:  Configure os secrets necessários no repositório que irá consumir os workflows
2. **Variáveis de repositório**: Configure as variáveis necessárias (para SonarCloud)
3. **Permissões**:  Certifique-se de que o repositório tem permissão para usar workflows de outros repositórios

### Chamando um workflow reutilizável

No seu repositório, crie um arquivo de workflow em `.github/workflows/`:

```yaml
name: Seu Workflow

on:
  push:
    branches: [main]

jobs:
  seu-job:
    uses: fiap-challenge-group-52/challenge-fiap-workflows/.github/workflows/<nome-do-workflow>.yaml@main
    with:
      # seus parâmetros aqui
    secrets:
      # seus secrets aqui
```

## 📦 Estrutura do Repositório

```
. 
├── . github/
│   └── workflows/
│       ├── build-push-ecr.yaml      # Workflow de CI/CD para AWS
│       └── quality-analysis.yaml    # Workflow de análise de qualidade
├── LICENSE                          # Licença MIT
└── README.md                        # Este arquivo
```


## 👥 Grupo 52

Desenvolvido como parte do Tech Challenge FIAP.
