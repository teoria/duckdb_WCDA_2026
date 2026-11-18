# duckdb_WCDA_2026

Material do minicurso **Análise de dados com DuckDB**, apresentado no WCDA 2026 (Workshop de Ciência de Dados e Analytics) da UPE, em 18 de novembro de 2026, no Auditório da Poli, em Recife.

Ministrante: Rodrigo Carneiro, especialista de dados na Filterbuy e mestre em Engenharia da Computação pela UPE ([github.com/teoria](https://github.com/teoria)).

## Sobre o treinamento

São 2 horas, presenciais e em português, cobrindo o DuckDB da série 1.x até a versão 2.0. O DuckDB é um banco de dados analítico que roda dentro do processo da sua aplicação, sem servidor e sem configuração. Ele lê CSV, Parquet e JSON direto do disco ou da nuvem e funciona com SQL, Python e R.

Público: analistas, engenheiros e estudantes que já usam Python e SQL.

## Roteiro

| Bloco | O que vemos | Tempo |
|---|---|---|
| Introdução | O que é o DuckDB, primeira consulta, integração com DataFrames, quando não usar | 15 min |
| Recursos da v1 | Leitura de arquivos, SQL amigável, QUALIFY, PIVOT, ASOF JOIN, MERGE, TRY, FILL, lambdas, extensões | 25 min |
| Novidades da v2 | Mudanças incompatíveis da 2.0, triggers, CTEs que gravam, `$variavel`, VARIANT, JSON, servidor Quack, CLI agente | 20 min |
| UI | DuckDB UI: como abrir e demonstração | 10 min |
| Casos | Sensores industriais, investigação de logs, lakehouse particionado | 20 min |
| Experimentos práticos | Pandas x Polars x DuckDB, paralelismo, CSV x Parquet, limite de memória, exercícios | 30 min |

## Notebooks

| Arquivo | Conteúdo |
|---|---|
| `notebooks/00_preparacao.ipynb` | Instala as dependências e gera os dados sintéticos do curso em `dados/` (menos de 1 minuto). Rode antes da aula. |
| `notebooks/01_introducao.ipynb` | Primeira consulta, consultas sobre DataFrames e quando o DuckDB não é a ferramenta certa. Cria `curso.duckdb`. |
| `notebooks/02_recursos_v1.ipynb` | Recursos da série 1.x: arquivos, SQL amigável, QUALIFY, PIVOT, ASOF, MERGE, TRY, FILL, lambdas e extensões. |
| `notebooks/03_novidades_v2.ipynb` | O que mudou na 2.0: quebras de compatibilidade, triggers, CTEs que gravam, variáveis, VARIANT, JSON, Quack e CLI agente. |
| `notebooks/04_ui.ipynb` | Como abrir a DuckDB UI e roteiro da demonstração. |
| `notebooks/05_casos.ipynb` | Três casos: falha numa prensa a partir de sensores industriais, investigação de logs e lakehouse particionado. |
| `notebooks/06_experimentos.ipynb` | Comparação Pandas x Polars x DuckDB, paralelismo, CSV x Parquet, limite de memória e 5 exercícios com gabarito. |
| `notebooks/07_queries_dos_slides.ipynb` | Todas as consultas dos slides, na ordem da apresentação. Só depende dos dados do 00. |

Os dados são sintéticos (cerca de 5 milhões de vendas, 600 mil linhas de log e 311 mil leituras de sensores, uns 255 MB). Depois do notebook 00, o curso roda sem internet.

## Slides

O deck está em `slides/curso-duckdb-slides.html`, um único arquivo HTML com as imagens embutidas. Para apresentar, abra no navegador. As setas ou o clique avançam (nos slides com consulta, o resultado aparece primeiro), N mostra as notas do apresentador e F entra em tela cheia.

## Como rodar

Requisitos: Python 3.11 ou mais recente. O DuckDB 2.0 não tem pacote para Python 3.10 ou anterior, e nesse caso o `pip` para com `No matching distribution found for duckdb>=2.0.0`. Confira a versão com `python3 --version`.

```bash
python3.11 -m venv .venv    # ou python3.12, python3.13
source .venv/bin/activate   # no Windows: .venv\Scripts\activate
cd notebooks
pip install -r requirements.txt
jupyter lab
```

Pacotes usados: `duckdb>=2.0.0`, `duckdb-cli>=2.0.0`, `pandas`, `polars`, `pyarrow`, `matplotlib` e `jupyter`.

Rode os notebooks em ordem. O 00 gera os dados e o 01 cria `curso.duckdb`, que o 02, o 04 e o 05 usam.

Se a sua máquina tem pouca memória, troque `N_VENDAS = 5_000_000` por `1_000_000` no notebook 00.

Algumas células precisam de internet: extensões `httpfs` e `ui`, dados do NYC Taxi, criptografia e servidor Quack. Sem rede, elas tratam o erro e o notebook segue.

## Notas da versão 2.0

Material testado com DuckDB 2.0.0 (Python), pandas 3.0 e Polars 2.1. Alguns detalhes da 2.0.0 que já estão contornados nos notebooks:

- `QUALIFY` não pode ser combinado com `GROUP BY ALL`. Liste as colunas.
- `MERGE INTO` não roda em tabela com trigger.
- `DROP TRIGGER nome ON tabela` exige o `ON tabela`.
- No Python, `$variavel` é lido como parâmetro. Use `getvariable()`. No CLI e na UI, `$variavel` funciona.
- Relações Python não têm mais `fetchone()`, e resultados de `execute()` precisam ser consumidos.
- Gravar banco criptografado exige `LOAD httpfs`.

Fontes: anúncios oficiais em [duckdb.org/news](https://duckdb.org/news) e a página da release v2.0.0 no GitHub.
