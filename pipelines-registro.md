# Registro de Pipelines — Inventário Baseline para HF Model Scout

Este inventário registra as etapas de código, APIs e prompts dos agentes do ecossistema local para monitoramento e varredura diária da skill `hf-model-scout`.

---

## 1. Agente Gmail (Triagem e Categorização de E-mails)
- **Etapa:** Classificação de e-mails recebidos por prioridade (🔴 Urgente, 🟡 Importante, 🟢 Informativo) e sumarização executiva em PT-BR.
- **Solução atual:** Chamada ao modelo LLM principal via prompt zero-shot.
- **Custo atual:** Variável por token (modelo de chat/prompt).
- **Latência atual:** ~2.0s a ~4.0s por mensagem.
- **Taxa de erro/observações:** Alta precisão, mas custo acumulado em caixas com alto volume. Candidato a modelo de classificação/embeddings leve e local se rodar sem GPU dedicada e em PT-BR.

## 2. Agente Fluxogramas (Geração de Sintaxe Mermaid)
- **Etapa:** Conversão de requisitos textuais ou passos de arquitetura em blocos válidos de sintaxe Mermaid.js com estilo semântico.
- **Solução atual:** Geração via LLM principal com regras de craft floor e validação anti-overflow.
- **Custo atual:** Grátis no tier do assistente ou custo de tokens do modelo principal.
- **Latência atual:** ~1.5s a ~3.0s.
- **Taxa de erro/observações:** Raros erros de escape em rótulos. Modelos especializados pequenos poderiam acelerar se garantirem suporte à gramática Mermaid.

## 3. Agente Assistente Financeiro (Categorização de Despesas e Extração de Recibos)
- **Etapa:** Extração de texto de comprovantes em PDF/imagens e categorização de despesas (alimentação, moradia, etc.).
- **Solução atual:** OCR genérico / visão multimodal via API LLM.
- **Custo atual:** Custo por imagem de API multimodal.
- **Latência atual:** ~2.5s por comprovante.
- **Taxa de erro/observações:** Alto valor se houver modelo local de OCR leve para faturas/recibos em PT-BR (ex: Donut/TrOCR fine-tuned ou SmolVLM quantizado) com custo zero e menor latência.

## 4. Agente Administrador Notebook (Deduplicação e Análise de Logs)
- **Etapa:** Deduplicação criptográfica SHA-256 e parsing de alertas/logs de eventos do Windows 11.
- **Solução atual:** Script PowerShell nativo + regex local.
- **Custo atual:** Grátis, 100% local.
- **Latência atual:** ~0.2s a ~1.0s.
- **Taxa de erro/observações:** 0% de erro; sem necessidade de substituição por ML hoje (solução determinística é ótima).
