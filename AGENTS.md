# Governança de Agentes — Workspace Agente Hugging Face

Este documento define os agentes, skills e diretrizes de automação disponíveis neste workspace do Antigravity.

---

## 1. Agentes e Subagentes Registrados

### `hf-specialist`
* **Tipo:** Subagente Especialista em Hugging Face Hub, Pesquisa e Varredura Ativa.
* **Skills Canônicas:**
  - `hf-research` (`skills/hf-research/SKILL.md`)
  - `hf-model-scout` (`skills/hf-model-scout/SKILL.md`)
* **Competências Principais:**
  - Pesquisa precisa de modelos, datasets, spaces e papers no Hub.
  - Varredura diária para otimização de pipelines contra `pipelines-registro.md`.
  - Auditoria de licenças para uso comercial e verificação de hardware (CPU/RAM/GPU).
  - Execução de chamadas seguras e otimizadas via MCP Server `huggingface`.
* **Como Invocar:**
  - `invoke_subagent` com `TypeName: "hf-specialist"`.
  - Linguagem natural: *"Pesquise no Hugging Face modelos leves para...", "Rode a varredura diária de modelos..."*.

---

## 2. Skills Integradas

1. **`hf-research`**:
   - Ponto de partida para traduzir intenções do usuário em chamadas MCP (`hf_fs`, `hub_repo_search`, `hub_repo_details`).
   - Retorna relatórios técnicos sintéticos com justificativa e links diretos do Hub.

2. **`hf-model-scout`**:
   - Rotina periódica / diária que compara modelos recentes com os baselines de `pipelines-registro.md`.
   - Exige ganho real mensurável sem aumento de custos computacionais ou monetários.

---

## 3. Diretrizes de Governança
- **Critério Rígido de Custo:** Custo computacional e monetário deve ser menor ou igual ao da solução atual.
- **Licenciamento Comercial:** Garantir licença compatível (MIT, Apache-2.0, etc.) antes de recomendar substituições para ferramentas em produção.
- **Rastreabilidade:** Todas as recomendações devem conter link direto e dados verificáveis do Hub.
