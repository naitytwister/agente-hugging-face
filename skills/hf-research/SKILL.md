---
name: hf-research
description: Pesquisa avançada e precisa no Hugging Face Hub (modelos, datasets, spaces, papers e trending) via Hugging Face MCP Server, a partir de pedidos em linguagem natural. Use esta skill sempre que alguém pedir para buscar, encontrar, comparar, investigar ou acompanhar modelos, datasets, spaces, papers ou tendências no Hugging Face — mesmo que não cite as ferramentas por nome. Também dispare para pedidos como "o que tem de novo em X no HF", "modelo mais leve/barato para Y", "dataset para treinar Z", "existe um Space pronto para W", ou qualquer investigação técnica que dependa de dados atualizados do Hub em vez de conhecimento antigo do modelo.
---

# HF Research

Skill para traduzir pedidos em linguagem natural em chamadas precisas às ferramentas do
Hugging Face MCP Server, e devolver um resumo técnico útil (não só uma lista de links).

## Ferramentas disponíveis

| Ferramenta | Uso |
|---|---|
| `hf_whoami` | Confirma conta/token/orgs ativos. Rodar só se a busca depender de permissões (repo privado, org específica). |
| `hub_repo_search` | Busca unificada de Models / Datasets / Spaces por palavra-chave, com filtro de tipo. Retorna downloads, likes, tags, link. **Ponto de partida padrão para qualquer busca.** |
| `hub_repo_details` | Aprofunda um repo específico: schema, splits, `dataset_preview`, arquivos, README. Usar depois do `hub_repo_search` quando o usuário precisa decidir entre 2-3 candidatos. |
| `hf_fs` | Filesystem virtual `hf://`. Use para: `ls hf://models/trending`, `ls hf://datasets/trending`, `ls hf://papers/daily/latest`, `cat hf://papers/{id}/paper.md`, `cat`/`find`/`stat`/`search` em qualquer repo ou em `hf://docs/`. **É a ferramenta certa para "o que está bombando hoje" e para ler um paper.** |
| `dynamic_space` | Descobre e executa Gradio Spaces em tempo real (`discover` → `view_parameters` → `invoke`). Usar quando o pedido é "existe algo pronto que faça X" (remoção de fundo, OCR, TTS, transcrição etc.) em vez de "me dê um modelo para eu integrar". |
| `gr1_z_image_turbo_generate` | Geração de imagem rápida via Z-Image-Turbo. Só usar se o pedido for explicitamente gerar imagem. |

## Como interpretar o pedido

1. **Identifique o tipo de necessidade** antes de escolher a ferramenta:
   - "modelo/dataset para X" → `hub_repo_search` (filtrado por tipo)
   - "o que há de novo / tendências / trending" → `hf_fs` com `ls hf://models/trending` ou `hf://datasets/trending`
   - "papers recentes sobre X" → `hf_fs` (`ls hf://papers/daily/latest`, depois `cat` no paper relevante)
   - "algo pronto para rodar/testar já" (sem precisar treinar/integrar) → `dynamic_space`
   - "qual desses dois é melhor / detalhes de licença, tamanho, splits" → `hub_repo_details`
2. **Nunca pare na primeira chamada** se o pedido for de decisão (ex: "qual modelo eu uso"): faça `hub_repo_search` para levantar candidatos e depois `hub_repo_details` nos 2-3 mais fortes antes de recomendar.
3. **Sempre filtre por recência e relevância real**, não só por likes/downloads — um modelo com poucos downloads mas lançado essa semana pode ser mais relevante que um consagrado e defasado, dependendo do pedido.

## Formato de resposta

Para pedidos de busca/comparação, sempre estruturar assim (não uma lista crua de links):

- **Resumo em 1-2 frases** do que foi encontrado.
- Por candidato: nome, o que faz, tamanho/licença, downloads/likes, por que ele se encaixa (ou não) no pedido.
- **Recomendação explícita** quando o pedido pedir decisão ("eu usaria X porque...").
- Link direto do Hub para cada item citado.

## Erros comuns a evitar

- Não confundir "trending" (popularidade recente) com "recente" (data de lançamento) — checar ambos quando relevante.
- Não recomendar um modelo/Space sem checar licença quando o uso é comercial (WAiStudio, Coletcai, produtos vendidos).
- Não usar `dynamic_space` para algo que precisa rodar embutido em produção — Spaces da comunidade têm SLA/disponibilidade não garantidos; sinalizar isso quando o usuário for colocar em pipeline crítico.
