# pipelines.ludens

<p align="center">
  <img src=".github/assets/logo.png" alt="ludens" width="180">

  <h3 align="center">ludens</h3>

  <p align="center">
    Plataforma de Venda de Ingressos para Teatro Comunitário
  </p>
</p>

Workflows reutilizáveis (`workflow_call`) de CI/CD pros repositórios da plataforma Ludens
(`api.ludens`, e futuramente `web.ludens`). Cada repo consumidor chama estes workflows em vez
de reimplementar test/build/deploy do zero.

## Workflows disponíveis

### `_test.yaml`

Sobe as dependências do projeto via `docker compose`, instala o pacote Python e roda `pytest`.
Não falha se ainda não houver suíte de testes (trata "nenhum teste coletado" como sucesso).

```yaml
jobs:
  test:
    uses: gcarvalhow/pipelines.ludens/.github/workflows/_test.yaml@master
    with:
      working-directory: .
      compose-file: docker/docker-compose.Development.yml
```

| Input | Obrigatório | Default | Descrição |
|---|---|---|---|
| `working-directory` | não | `.` | Diretório de onde instalar o pacote e rodar `pytest` |
| `compose-file` | não | — | Path do `docker-compose` com as dependências de teste (ex.: Postgres). Se omitido, não sobe nada |

### `_build.yaml`

Builda a imagem Docker do projeto a partir do `Dockerfile` na raiz.

```yaml
jobs:
  build:
    uses: gcarvalhow/pipelines.ludens/.github/workflows/_build.yaml@master
    with:
      image-name: api-ludens:ci
```

| Input | Obrigatório | Default | Descrição |
|---|---|---|---|
| `image-name` | sim | — | Tag da imagem gerada pelo `docker build` |

## Convenção

- Prefixo `_` no nome do arquivo = workflow reutilizável (`on: workflow_call`), nunca disparado
  sozinho por push/PR.
- Branch padrão `master`, mesmo padrão dos outros repositórios `*.ludens`.
