# Workflows no GitHub

<!-- Este arquivo explica diferentes workflows e recursos do GitHub -->

## 📋 Objetivos de Aprendizagem

<!-- TODO: Objetivos sobre workflows colaborativos -->

## 🎯 Introdução

<!-- TODO: GitHub além de hospedagem de código -->
<!-- Plataforma completa de colaboração e automação -->

## O que é um Workflow?

<!-- TODO: Definição de workflow de desenvolvimento -->
<!-- Conjunto de práticas e processos para colaborar -->

## Fork Workflow

### O que é Fork?

<!-- TODO: Cópia do repositório na sua conta -->

### Quando Usar

<!-- TODO: Projetos open source, contribuições externas -->

### Passo a Passo

#### 1. Fork do Repositório

<!-- TODO: Botão Fork no GitHub -->

#### 2. Clone do Fork

```bash
# TODO: git clone SEU-FORK
```

#### 3. Configurar Upstream

```bash
# TODO: git remote add upstream REPO-ORIGINAL
```

#### 4. Criar Branch

```bash
# TODO: git checkout -b feature/minha-contribuicao
```

#### 5. Fazer Mudanças e Commit

```bash
# TODO: git add e git commit
```

#### 6. Push para Fork

```bash
# TODO: git push origin feature/minha-contribuicao
```

#### 7. Abrir Pull Request

<!-- TODO: PR do fork para repositório original -->

#### 8. Manter Fork Atualizado

```bash
# TODO: git fetch upstream
# git merge upstream/main
```

### Vantagens

<!-- TODO: Segurança, experimentação, contribuições externas -->

## GitHub Flow

### O que é

<!-- TODO: Workflow simples e ágil -->

### Princípios

1. <!-- Main está sempre deployável -->
2. <!-- Branches descritivas -->
3. <!-- PRs para discussão -->
4. <!-- Deploy após merge -->

### Fluxo Completo

```
main → branch → commits → PR → review → merge → deploy
```

### Quando Usar

<!-- TODO: Projetos com deploy contínuo -->

## Git Flow

### O que é

<!-- TODO: Workflow mais estruturado -->

### Branches Principais

#### main (ou master)

<!-- TODO: Código em produção -->

#### develop

<!-- TODO: Desenvolvimento atual -->

### Branches de Suporte

#### feature/*

<!-- TODO: Novas funcionalidades -->

#### release/*

<!-- TODO: Preparação de release -->

#### hotfix/*

<!-- TODO: Correções urgentes em produção -->

### Fluxo Visual

<!-- TODO: Diagrama do Git Flow -->

### Quando Usar

<!-- TODO: Projetos com releases planejadas -->

### Ferramentas

<!-- TODO: git-flow extension -->

## Trunk-Based Development

### O que é

<!-- TODO: Todos commitam em uma branch principal -->

### Características

<!-- TODO: Integração contínua, feature flags -->

### Quando Usar

<!-- TODO: Equipes maduras, CI/CD forte -->

## Issues

### O que São Issues

<!-- TODO: Sistema de rastreamento de tarefas -->

### Tipos de Issues

<!-- TODO: Bugs, features, questions, documentation -->

### Criando Issues

<!-- TODO: Título, descrição, labels, assignees -->

### Templates de Issues

<!-- TODO: .github/ISSUE_TEMPLATE/ -->

### Labels

<!-- TODO: Organização com labels -->

#### Labels Comuns

- `bug` - <!-- Algo não está funcionando -->
- `enhancement` - <!-- Nova funcionalidade -->
- `documentation` - <!-- Melhorias na documentação -->
- `good first issue` - <!-- Bom para iniciantes -->
- `help wanted` - <!-- Precisa de ajuda -->

### Milestones

<!-- TODO: Agrupar issues por objetivo/release -->

### Assignees

<!-- TODO: Atribuir responsáveis -->

### Linking Issues e PRs

<!-- TODO: Closes, Fixes, Resolves -->

## Projects

### GitHub Projects

<!-- TODO: Gestão de projeto estilo Kanban -->

### Criando um Project

<!-- TODO: Passo a passo -->

### Colunas

<!-- TODO: To Do, In Progress, Done -->

### Automatização

<!-- TODO: Auto-move baseado em eventos -->

### Views

<!-- TODO: Board, Table, Roadmap -->

## GitHub Actions

### O que São Actions

**CI (Continuous Integration)** é a prática de integrar código frequentemente ao repositório principal, com testes automáticos a cada push.

**CD (Continuous Delivery/Deployment)** é a prática de entregar ou implantar automaticamente o código após passar pelos testes.

O **GitHub Actions** é a ferramenta nativa do GitHub para automatizar esses fluxos de trabalho diretamente no repositório.


### Casos de Uso


- **Testes automáticos**: rodar testes a cada push ou pull request para garantir que nada quebrou
- **Build**: compilar o projeto e gerar os arquivos finais automaticamente
- **Deploy**: enviar o código para produção após aprovação ou merge na `main`
- **Linting**: verificar formatação e qualidade do código automaticamente
- **Notificações**: avisar a equipe sobre falhas ou sucessos no Slack, e-mail, etc.

---

### Workflow File

Os workflows ficam dentro da pasta `.github/workflows/` e são escritos em YAML:

```yaml
# .github/workflows/exemplo.yml
name: Meu Workflow

on:
  push:
    branches: [main]
  pull_request:
    branches: [main]

jobs:
  exemplo:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      - name: Rodar um comando
        run: echo "Hello, GitHub Actions!"
```

---

### Eventos (Triggers)


O campo `on:` define quando o workflow será disparado:

```yaml
on:
  push:                        # a cada push em qualquer branch
    branches: [main]           # filtra só a branch main

  pull_request:                # quando um PR é aberto ou atualizado
    branches: [main]

  schedule:
    - cron: '0 6 * * 1'        # toda segunda-feira às 6h (UTC)

  workflow_dispatch:           # permite rodar manualmente pelo GitHub
```

---


### Jobs e Steps


Um workflow é composto por **jobs**, e cada job é composto por **steps**:

```yaml
jobs:
  meu-job:
    runs-on: ubuntu-latest      # sistema onde o job vai rodar

    steps:
      - name: Baixar o código
        uses: actions/checkout@v4     # action pronta do marketplace

      - name: Configurar Node.js
        uses: actions/setup-node@v4
        with:
          node-version: '20'

      - name: Instalar dependências
        run: npm install              # comando shell direto

      - name: Rodar testes
        run: npm test
```

- **`uses`**: usa uma action pronta (do marketplace ou do próprio repositório)
- **`run`**: executa um comando shell
- **`with`**: passa parâmetros para a action
- **`env`**: define variáveis de ambiente para o step

---


### Exemplo: CI Básico

Workflow completo para rodar testes automaticamente a cada push ou pull request:

```yaml
# .github/workflows/ci.yml
name: CI

on:
  push:
  pull_request:

jobs:
  test:
    runs-on: ubuntu-latest

    strategy:
      matrix:
        node-version: [18, 20, 22]   # testa em múltiplas versões

    steps:
      - uses: actions/checkout@v4

      - name: Configurar Node.js ${{ matrix.node-version }}
        uses: actions/setup-node@v4
        with:
          node-version: ${{ matrix.node-version }}
          cache: 'npm'               # acelera instalação com cache

      - name: Instalar dependências
        run: npm ci

      - name: Rodar testes
        run: npm test
```

---

### Marketplace

O [GitHub Actions Marketplace](https://github.com/marketplace?type=actions) reúne milhares de actions prontas criadas pela comunidade. Exemplos úteis:

| Action | O que faz |
|---|---|
| `actions/checkout@v4` | Baixa o código do repositório |
| `actions/setup-node@v4` | Configura o Node.js |
| `actions/setup-python@v5` | Configura o Python |
| `actions/cache@v4` | Faz cache de dependências |
| `actions/upload-artifact@v4` | Salva arquivos gerados pelo workflow |
| `actions/download-artifact@v4` | Baixa arquivos salvos anteriormente |

Para usar uma action do marketplace, basta referenciar com `uses`:

```yaml
- uses: actions/setup-node@v4
  with:
    node-version: '20'
```

> 💡 Sempre use uma versão fixada (ex: `@v4`) para evitar quebras inesperadas quando a action for atualizada.

---

### Build Matrix

A build matrix permite testar em várias versões de linguagem ou sistema operacional ao mesmo tempo:

```yaml
jobs:
  test:
    runs-on: ${{ matrix.os }}
    strategy:
      matrix:
        os: [ubuntu-latest, windows-latest, macos-latest]
        node-version: [18, 20, 22]

    steps:
      - uses: actions/checkout@v4

      - name: Configurar Node.js ${{ matrix.node-version }}
        uses: actions/setup-node@v4
        with:
          node-version: ${{ matrix.node-version }}

      - name: Instalar dependências
        run: npm install

      - name: Rodar testes
        run: npm test
```

Isso cria 9 combinações (3 SOs × 3 versões) e todas rodam em paralelo.

---

### Deploy Automático na Main

Para fazer deploy apenas quando há merge na branch `main`:

```yaml
# .github/workflows/deploy.yml
name: Deploy

on:
  push:
    branches:
      - main

jobs:
  deploy:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4

      - name: Instalar dependências
        run: npm install

      - name: Build do projeto
        run: npm run build

      - name: Deploy para produção
        run: npm run deploy
        env:
          DEPLOY_TOKEN: ${{ secrets.DEPLOY_TOKEN }}
```

---

### Artifacts

Artifacts são arquivos gerados durante o workflow que você quer preservar (relatórios, binários, logs):

```yaml
jobs:
  build:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4

      - name: Build
        run: npm run build

      - name: Upload dos artifacts
        uses: actions/upload-artifact@v4
        with:
          name: build-output
          path: dist/
          retention-days: 7
```

Para baixar um artifact em outro job:

```yaml
      - name: Download dos artifacts
        uses: actions/download-artifact@v4
        with:
          name: build-output
```

---

### Secrets Management

Nunca coloque senhas ou tokens diretamente no código. Use os **Secrets** do GitHub:

**Como adicionar um Secret:**

1. Vá em **Settings** do repositório
2. Clique em **Secrets and variables > Actions**
3. Clique em **New repository secret**
4. Dê um nome (ex: `API_KEY`) e insira o valor

**Como usar no workflow:**

```yaml
jobs:
  deploy:
    runs-on: ubuntu-latest
    steps:
      - name: Usar secret
        run: echo "Conectando com token seguro..."
        env:
          API_KEY: ${{ secrets.API_KEY }}
          DATABASE_URL: ${{ secrets.DATABASE_URL }}
```

> ⚠️ Secrets nunca aparecem nos logs — o GitHub os oculta automaticamente.

---

### Cache de Dependências

Cachear dependências evita baixar tudo do zero a cada execução, tornando o workflow mais rápido:

```yaml
jobs:
  test:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4

      - name: Configurar Node.js
        uses: actions/setup-node@v4
        with:
          node-version: '20'
          cache: 'npm'

      - name: Instalar dependências
        run: npm ci

      - name: Rodar testes
        run: npm test
```

Para projetos Python:

```yaml
      - uses: actions/setup-python@v5
        with:
          python-version: '3.12'
          cache: 'pip'

      - run: pip install -r requirements.txt
```

---

### Status Badges

Badges mostram o status do workflow diretamente no `README.md`. A URL segue o padrão:

```
https://github.com/USUARIO/REPOSITORIO/actions/workflows/ARQUIVO.yml/badge.svg
```

Para adicionar ao README:

```markdown
![CI](https://github.com/hikazudani/gh0/actions/workflows/ci.yml/badge.svg)
![Deploy](https://github.com/hikazudani/gh0/actions/workflows/deploy.yml/badge.svg)
```

| Status | Significado |
|---|---|
| ![passing](https://img.shields.io/badge/build-passing-brightgreen) | Workflow rodou com sucesso |
| ![failing](https://img.shields.io/badge/build-failing-red) | Algum passo falhou |

---

### Workflows Avançados

**Jobs com dependência (`needs`):**

```yaml
jobs:
  test:
    runs-on: ubuntu-latest
    steps:
      - run: npm test

  build:
    needs: test
    runs-on: ubuntu-latest
    steps:
      - run: npm run build

  deploy:
    needs: build
    runs-on: ubuntu-latest
    steps:
      - run: npm run deploy
```

**Execução manual (`workflow_dispatch`):**

```yaml
on:
  workflow_dispatch:
    inputs:
      ambiente:
        description: 'Ambiente de deploy'
        required: true
        default: 'staging'
        type: choice
        options:
          - staging
          - production
```

**Agendamento com cron:**

```yaml
on:
  schedule:
    - cron: '0 6 * * 1'   # toda segunda-feira às 6h
```

---

### Resumo

| Recurso | Para que serve |
|---|---|
| `on: push` | Dispara o workflow a cada push |
| `matrix` | Testa em múltiplas versões e SOs |
| `needs` | Define ordem de execução dos jobs |
| `secrets` | Armazena dados sensíveis com segurança |
| `cache` | Acelera instalação de dependências |
| `upload-artifact` | Preserva arquivos gerados no workflow |
| `badge` | Exibe status do CI no README |
| `workflow_dispatch` | Permite execução manual |
| `schedule` | Agenda execuções automáticas |


## GitHub Pages

### O que é

<!-- TODO: Hospedagem estática gratuita -->

### Casos de Uso

<!-- TODO: Documentação, portfolio, landing pages -->

### Habilitando Pages

<!-- TODO: Settings → Pages -->

### Fontes

- <!-- main branch -->
- <!-- docs/ folder -->
- <!-- gh-pages branch -->

### Jekyll

<!-- TODO: Gerador de sites estáticos integrado -->

### Custom Domain

<!-- TODO: Usar domínio próprio -->

## GitHub Discussions

### O que São

<!-- TODO: Fórum de comunidade -->

### Quando Usar

<!-- TODO: Q&A, ideias, anúncios -->

### Diferença de Issues

<!-- TODO: Discussions = conversas, Issues = tarefas -->

## GitHub Wiki

### O que É

<!-- TODO: Documentação colaborativa -->

### Quando Usar

<!-- TODO: Docs extensas, knowledge base -->

## GitHub Gists

### O que São

<!-- TODO: Compartilhar snippets de código -->

### Tipos

<!-- TODO: Públicos vs secretos -->

## Code Owners

### Arquivo CODEOWNERS

<!-- TODO: Definir revisores por arquivo/pasta -->

```
# TODO: Exemplo de CODEOWNERS
# /docs/ @documentacao-team
# *.js @javascript-team
```

## Branch Protection

### O que É

<!-- TODO: Regras para proteger branches -->

### Regras Comuns

- <!-- Require PR -->
- <!-- Require reviews -->
- <!-- Require status checks -->
- <!-- Require conversation resolution -->
- <!-- Restrict push -->

### Configurando

<!-- TODO: Settings → Branches → Add rule -->

## Security

### Dependabot

<!-- TODO: Alertas de segurança em dependências -->

### Security Advisories

<!-- TODO: Reportar vulnerabilidades -->

### Secret Scanning

<!-- TODO: Detectar secrets commitados -->

## Notifications

### Configurar Notificações

<!-- TODO: Email, web, mobile -->

### Watching

<!-- TODO: Níveis de watching em repos -->

### Unsubscribe

<!-- TODO: Como parar de receber notificações -->

## GitHub CLI

### Instalação

```bash
# TODO: Como instalar gh CLI
```

### Comandos Úteis

```bash
# TODO: Exemplos de comandos gh
# gh repo clone
# gh pr create
# gh issue list
# gh workflow run
```

## Integrations & Apps

### GitHub Apps

<!-- TODO: Integrar serviços externos -->

### Popular Apps

- <!-- Slack -->
- <!-- Discord -->
- <!-- Trello -->
- <!-- Codecov -->

## Exemplos Práticos

### Exemplo 1: Contribuir para Open Source

<!-- TODO: Fork workflow completo -->

### Exemplo 2: Criar Documentação com Pages

<!-- TODO: Setup de GitHub Pages -->

### Exemplo 3: Automatizar Testes com Actions

<!-- TODO: Workflow de CI -->

## Workflows de Equipe

### Async vs Sync

<!-- TODO: Trabalho assíncrono com Git -->

### Code Review Culture

<!-- TODO: Estabelecer cultura de revisão -->

### Release Management

<!-- TODO: Como gerenciar releases -->

## Erros Comuns

### Erro 1: Não Atualizar Fork

<!-- TODO: Fork desatualizado -->

### Erro 2: Ignorar Issues

<!-- TODO: Importância de documentar trabalho -->

### Erro 3: Não Configurar Branch Protection

<!-- TODO: Main desprotegida -->

## Boas Práticas

<!-- TODO: Lista de boas práticas para workflows -->

- <!-- Documentar workflow da equipe -->
- <!-- Usar templates -->
- <!-- Automatizar o que puder -->
- <!-- Comunicar mudanças -->
- <!-- Regular reviews -->

## Exercícios

<!-- TODO: Exercícios práticos -->

1. <!-- Fork um repositório e contribuir -->
2. <!-- Criar GitHub Project para organizar issues -->
3. <!-- Configurar GitHub Pages -->
4. <!-- Criar workflow de CI simples -->

## Recursos Adicionais

<!-- TODO: Links sobre workflows -->

- [GitHub Flow Guide](https://guides.github.com/introduction/flow/)
- [Git Flow](https://nvie.com/posts/a-successful-git-branching-model/)
- [GitHub Actions Documentation](https://docs.github.com/en/actions)
- [GitHub Pages](https://pages.github.com/)
- <!-- Mais recursos -->

## Resumo

<!-- TODO: Principais workflows e recursos GitHub -->

---

## 👥 Contribuidores

<!-- Este conteúdo é colaborativo. Contribuidores deste arquivo: -->
<!-- Adicione seu nome quando contribuir:
- [@seu-usuario](https://github.com/seu-usuario) - Seção X
-->
