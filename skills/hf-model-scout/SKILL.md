---
name: hf-model-scout
description: Rotina de varredura ativa e periódica (diária) no Hugging Face para achar modelos que possam substituir etapas de código, lógica manual ou chamadas de API cara nos pipelines dos agentes de desenvolvimento do usuário — sempre exigindo ganho real e mensurável, sem aumento de custo computacional ou monetário. Use quando pedirem para "rodar a varredura diária", "ver se dá pra otimizar algum agente com um modelo pronto do HF", "escanear novidades relevantes pro meu stack", ou ao revisar o pipeline de um agente e quiser saber se existe um modelo/Space que faça aquela etapa melhor, mais rápido ou mais barato que o código atual. Depende da skill hf-research para as buscas técnicas no Hub.
---

# HF Model Scout

Skill de "melhoria contínua vigiada": varre o Hugging Face em busca de modelos/Spaces que
possam substituir uma etapa de pipeline por algo melhor — e só reporta quando o ganho é
real, metrificado e não custa mais caro (nem em dinheiro, nem em compute) do que a solução
atual. Não é para inflar a lista de ferramentas do usuário; é para reduzir bugs, latência
ou custo em produção.

## Pré-requisito: registro de pipelines

Esta skill precisa de um **inventário mínimo** dos pipelines a vigiar. Se ele não existir
ainda, proponha criar `pipelines-registro.md` (ou usar arquivo já existente do usuário) com,
por pipeline/agente:

```markdown
## <nome do agente/pipeline>
- Etapa: <o que essa etapa faz>
- Solução atual: <código / API / prompt>
- Custo atual: <ex: $0.002/execução, ou "grátis, tier local">
- Latência atual: <ex: ~1.2s>
- Taxa de erro/observações: <ex: "falha em ~3% dos casos com PT-BR informal">
```

Sem isso, a varredura não tem baseline para comparar — não adivinhe custos/latência atuais,
pergunte ou leia do que já existe (ex: agentes descritos em outras áreas do projeto).

## Rotina diária

1. **Varredura de novidades** (via `hf-research` / `hf_fs`):
   - `ls hf://models/trending`
   - `ls hf://papers/daily/latest` (papers às vezes anunciam modelo antes dele bombar)
   - Buscas direcionadas por categoria de cada etapa do registro (ex: "OCR pt-br", "function calling small model", "image background removal")
2. **Filtro de candidatos** — descartar direto se:
   - Licença incompatível com uso comercial (produtos WAiStudio/Coletcai)
   - Sem suporte a PT-BR, quando a etapa exige
   - Requer infra que o usuário não tem hoje (ex: GPU dedicada, VPS — hoje não há VPS contratado)
3. **Para cada candidato que sobrar**, usar `hub_repo_details` para levantar:
   - Tamanho do modelo / requisitos de hardware
   - Licença
   - Benchmarks públicos ou evidência de qualidade (não confiar só em likes)
4. **Calcular o veredito** (ver critério abaixo) e só incluir no relatório final quem passar.

## Critério de aprovação (obrigatório, não é sugestão)

Um candidato só entra no relatório se, comparado à solução atual do registro:

- **Custo monetário:** igual ou menor (nunca maior).
- **Custo computacional:** igual ou menor (nunca mais GPU/RAM/infra do que hoje é usado).
- **E pelo menos um ganho real:** menos erros, menor latência, menos manutenção/menos código, ou desbloqueia algo que hoje é feito manualmente.

Se um modelo é melhor mas custa mais (dinheiro ou compute), ele **não** entra como
recomendação — pode entrar numa seção separada "observado, mas fora do critério de custo"
só se o ganho for muito grande, deixando a decisão explícita para o usuário.

## Formato do relatório diário

```markdown
# Varredura HF — <data>

## Recomendados (ganho real, custo igual ou menor)
- <modelo/Space>: substitui <etapa> no <agente>. Ganho: <métrica>. Custo: <comparação>. Link.

## Observados (ganho relevante, mas custo maior — decisão do usuário)
- ...

## Sem novidades relevantes hoje
(se for o caso, dizer isso e não inventar candidato fraco só para preencher)
```

Não gerar recomendação nenhuma dia sim, dia não só para parecer produtivo — o "sem
novidades relevantes hoje" é um resultado válido e esperado na maioria dos dias.

## Integração com outras skills/processos

Combine com qualquer skill de revisão de pipeline que o usuário já usa para decidir onde
vale a pena investigar primeiro (ex: pipelines com maior taxa de erro atual, ou maior custo
mensal atual têm prioridade de varredura). Se o usuário citar uma skill específica para essa
priorização, ela deve ser lida antes de rodar a varredura para saber quais etapas focar.
