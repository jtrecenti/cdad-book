# Ciência de Dados Aplicada ao Direito

Guia de contribuição para o livro "Ciência de Dados Aplicada ao Direito" desenvolvido no Insper.

## Estrutura do Projeto

Este livro está organizado em três partes principais conforme definido no `_quarto.yml`:

### Parte I: Planejando pesquisa e construindo dados

- Operacionalização de conceitos
- Fontes de dados judiciário
- Raspagem de dados
- Técnicas de amostragem
- Tipos de dados
- Construção e limpeza de dados

### Parte II: Análise estatística

- Medidas de posição e variabilidade
- Visualização de dados jurídicos
- Correlação e causalidade
- Probabilidade e distribuições
- Intervalo de confiança
- Testes de hipóteses

### Parte III: Técnicas estatísticas e aplicações

- Análise de regressão
- Modelos de classificação
- Modelagem de tempos de processos
- Aplicações corporativas

## Status dos Documentos

### Documentos Existentes

- [ ] `00-intro.qmd` - Capítulo de introdução
- [x] `index.qmd` - Página inicial
- [x] `01-operacionalizacao-conceitos.qmd`
- [ ] `02-fontes-dados-judiciario.qmd`
- [ ] `03-raspagem-dados.qmd`
- [ ] `04-tecnicas-amostragem.qmd`
- [ ] `05-tipos-dados.qmd`
- [ ] `06-construcao-limpeza-dados.qmd`
- [x] `07-medidas-posicao-variabilidade.qmd`
- [ ] `08-visualizacao-dados-juridicos.qmd`
- [ ] `09-correlacao-causalidade.qmd`
- [ ] `10-probabilidade-distribuicoes.qmd`
- [ ] `11-intervalo-confianca.qmd`
- [ ] `12-testes-hipoteses.qmd`
- [ ] `13-analise-regressao.qmd`
- [ ] `14-modelos-classificacao.qmd`
- [ ] `15-modelagem-tempos-processos.qmd`
- [ ] `16-aplicacoes-corporativas.qmd`
- [ ] `resumo.qmd`
- [x] `refs.qmd`

## Configuração do Ambiente de Desenvolvimento

### Pré-requisitos

- Python 3.11+
- Quarto CLI
- Git

### Instalação com uv

Este projeto utiliza `uv` para gerenciamento de dependências Python. Siga os passos abaixo:

#### 1. Instalar uv

```bash
# Windows
curl -LsSf https://astral.sh/uv/install.sh | sh

# Ou via pip
pip install uv
```

#### 2. Configurar ambiente virtual

```bash
# Clonar o repositório
git clone <url-do-repositorio>
cd cdad-book

# Criar ambiente virtual com uv
uv venv

# Ativar ambiente virtual
# Windows
.venv\Scripts\activate
# Linux/Mac
source .venv/bin/activate

# Instalar dependências
uv sync
```

#### 3. Instalar Quarto

Baixe e instale o Quarto CLI em: https://quarto.org/docs/get-started/

### Verificar Instalação

```bash
# Verificar Quarto
quarto --version

# Verificar Python no ambiente virtual
python --version

# Renderizar o livro localmente
quarto render
```

## Exercícios Interativos com Quarto Live

O projeto utiliza a extensão Quarto Live para criar exercícios interativos em Python. Esta extensão permite executar código Python diretamente no navegador usando WebAssembly.

### Configuração do Quarto Live

A extensão já está configurada no `_quarto.yml`:

```yaml
filters:
  - pyodide
```

### Criando Exercícios Interativos

#### Exemplo Básico

````
```{pyodide-python}
# Código executável
import pandas as pd
import matplotlib.pyplot as plt

# Exemplo com dados jurídicos
dados = pd.DataFrame({
    'processo': ['001', '002', '003'],
    'tempo_dias': [120, 95, 180]
})

print(dados.head())
```
````

#### Exercício com Entrada do Usuário

````markdown
```{pyodide-python}
#| exercise: ex_1
#| autorun: false

# Complete o código abaixo
import pandas as pd

# Carregue os dados
df = pd.read_csv('dados_processos.csv')

# TODO: Calcule a média de tempo dos processos
media_tempo = ___

print(f"Tempo médio: {media_tempo} dias")
```
````

#### Exercício com Solução

````markdown
```{pyodide-python}
#| exercise: ex_1
#| solution: true

import pandas as pd

df = pd.read_csv('dados_processos.csv')
media_tempo = df['tempo_dias'].mean()

print(f"Tempo médio: {media_tempo} dias")
```
````

### Boas Práticas para Exercícios

1. **Contexto Jurídico**: Sempre use exemplos relacionados ao direito
2. **Progressividade**: Exercícios devem aumentar em complexidade
3. **Feedback**: Inclua mensagens de erro e sucesso claras
4. **Dados Realistas**: Use datasets que simulem casos reais do judiciário

## Fluxo de Contribuição

### 1. Preparação

```bash
# Criar branch para nova funcionalidade
git checkout -b feat/nome-da-funcionalidade

# Ou para correções
git checkout -b fix/nome-da-correcao
```

### 2. Desenvolvimento

- Edite os arquivos `.qmd` relevantes
- Adicione exercícios interativos quando apropriado
- Teste localmente com `quarto render`

### 3. Submissão

```bash
# Commit das alterações
git add .
git commit -m "feat: adiciona exercícios interativos no capítulo X"

# Push da branch
git push origin feat/nome-da-funcionalidade
```

### 4. Pull Request

- Crie PR no GitHub
- Descreva as alterações realizadas
- Aguarde revisão e deploy automático

## Deploy Automático

### Branches de Referência

O projeto está configurado para deploy automático de branches com prefixo `/ref`:

- `main` → Deploy principal
- `ref/capitulo-1` → Deploy de preview em `/ref/capitulo-1`
- `ref/exercicios` → Deploy de preview em `/ref/exercicios`

### GitHub Actions

O workflow de deploy está configurado em `.github/workflows/_publish.yml`:

```yaml
name: Publish Quarto Book

on:
  push:
    branches: 
      - main
      - 'ref/**'
      - 'feat/**'
      - 'fix/**'
  pull_request:
    branches: 
      - main

jobs:
  publish:
    runs-on: ubuntu-latest
    permissions:
      contents: read
      pages: write
      id-token: write
    
    steps:
    - name: Checkout
      uses: actions/checkout@v4
    
    - name: Setup Python
      uses: actions/setup-python@v4
      with:
        python-version: '3.11'
    
    - name: Install uv
      run: pip install uv
    
    - name: Setup dependencies
      run: |
        uv venv
        source .venv/bin/activate
        uv pip install -r pyproject.toml
    
    - name: Setup Quarto
      uses: quarto-dev/quarto-actions/setup@v2
    
    - name: Render Book
      run: |
        source .venv/bin/activate
        quarto render
    
    - name: Deploy to GitHub Pages
      if: github.ref == 'refs/heads/main'
      uses: peaceiris/actions-gh-pages@v3
      with:
        github_token: ${{ secrets.GITHUB_TOKEN }}
        publish_dir: ./_book
    
    - name: Deploy Preview
      if: startsWith(github.ref, 'refs/heads/ref/')
      uses: peaceiris/actions-gh-pages@v3
      with:
        github_token: ${{ secrets.GITHUB_TOKEN }}
        publish_dir: ./_book
        destination_dir: ${{ github.ref_name }}
```

### Acessando Previews

- **Site principal**: `https://usuario.github.io/cdad-book/`
- **Preview de branch**: `https://usuario.github.io/cdad-book/<ref|feat|fix>/nome-da-branch/`

## Estrutura de Arquivos

```
cdad-book/
├── _quarto.yml           # Configuração principal do Quarto
├── pyproject.toml        # Dependências Python
├── uv.lock              # Lock file do uv
├── index.qmd            # Página inicial
├── 00-introducao.qmd    # Introdução
├── 01-*.qmd             # Capítulos da Parte I
├── 07-*.qmd             # Capítulos da Parte II  
├── 13-*.qmd             # Capítulos da Parte III
├── resumo.qmd           # Resumo final
├── refs.qmd             # Referências
├── references.bib       # Bibliografia
├── assets/              # Imagens e recursos
├── _extensions/         # Extensões do Quarto
└── .github/workflows/   # GitHub Actions
```

## Padrões de Código

### Estrutura de Capítulos

````markdown
# Título do Capítulo

## Objetivos de Aprendizagem
- Objetivo 1
- Objetivo 2

## Conceitos Fundamentais
[Conteúdo teórico]

## Exemplo Prático
```{pyodide-python}
# Código demonstrativo
```

## Exercícios

```{pyodide-python}
#| exercise: ex_nome
# Exercício interativo
```

## Resumo

[Pontos principais]

## Referências

[Bibliografia específica]
````

### Nomenclatura

- Arquivos: `XX-nome-do-capitulo.qmd`
- Exercícios: `ex_capitulo_numero`
- Imagens: `assets/capitulo-XX/nome-imagem.png`

## Suporte

Para dúvidas ou problemas:

1. Consulte a documentação do Quarto: https://quarto.org
2. Documentação do Quarto Live: https://r-wasm.github.io/quarto-live/
3. Abra uma issue no repositório do projeto

## Licença

Este projeto está sob licença MIT.