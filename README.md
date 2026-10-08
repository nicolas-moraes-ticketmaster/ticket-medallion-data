# ticket-medallion-data

Dados **100% sintéticos** de vendas de ingressos para estudo de arquitetura medalhão (Raw → Bronze → Silver → Gold) no Azure Data Factory.
Eventos, locais, artistas e clientes são **fictícios**. Nenhum dado real de empresa.

## Arquivos

| Pasta | Arquivo | Conteúdo |
|---|---|---|
| `vendas/` | `vendas_AAAA-MM.csv.gz` | 1 linha por **item de pedido** (~80 mil linhas/mês, out/2025 a out/2026) |
| `eventos/` | `eventos_AAAA-MM.csv` | Snapshot do catálogo de eventos no dia 1 de cada mês |

URL para o ADF (HTTP, sem autenticação):
```
https://raw.githubusercontent.com/<usuario>/ticket-medallion-data/main/vendas/vendas_2025-10.csv.gz
```

## Formato
- Separador `;` · decimal com vírgula · CRLF · gzip (vendas)
- UTF-8 com BOM — **exceto** `vendas_2026-01` e `vendas_2026-02`, que estão em ISO-8859-1
- A partir de `vendas_2026-06`: `valor_total` → `valor_total_brl` e nova coluna `id_promotor`

## Problemas de qualidade embutidos (a tratar na Silver)
Datas em 4 formatos · valores `"R$ 1.234,50"` · CPF formatado/nulo/inválido · e-mails sujos · cidades e UFs inconsistentes · canal/status com caixa mista · nulos como vazio/`NULL`/`null` · pedidos duplicados · pedidos que chegam atrasados · pedidos que mudam para CANCELADO/ESTORNADO dias depois · eventos órfãos · quantidade ≤ 0 · valores do pedido repetidos em cada item (risco de dupla contagem) · eventos adiados (SCD2).

## Regerar
Os dados são gerados por `gerador/exportar_csv.js` via GitHub Actions (aba **Actions → Gerar dados sinteticos → Run workflow**). A geração é determinística: mesmo código, mesmos dados.
