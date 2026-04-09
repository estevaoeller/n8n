# Workflows importáveis para n8n (Outlook + Google Alerts)

Arquivos:

1. `01_outlook_alertas_ingestao.json`
2. `02_enriquecimento_deduplicacao.json`
3. `03_fechamento_semanal_email.json`

## Como importar

1. No n8n: **Workflows** → **Import from File**.
2. Importe os 3 JSONs.
3. Abra cada workflow e configure credenciais/IDs:
   - `Outlook IMAP` (host `outlook.office365.com`, porta `993`, SSL/TLS)
   - `Outlook SMTP` (host `smtp-mail.outlook.com`, porta `587`, STARTTLS)
   - `Google Sheets OAuth2`
   - `documentId` da planilha no campo `__SET_ME__`

## Planilha esperada

Aba: `Base_Noticias` com colunas mínimas:

- `id`
- `captured_at`
- `alert_subject`
- `keyword`
- `title_raw`
- `source_raw`
- `url_raw`
- `snippet_raw`
- `status`
- `duplicate`
- `error_reason`
- `url_canonica`
- `hash_titulo`
- `title_final`
- `resumo_final`
- `comentario_analitico`
- `relevancia`
- `published_at`

## Observações

- Os workflows vêm **desativados** (`active: false`).
- A deduplicação está em versão inicial (URL canônica + hash de título).
- Você pode trocar o node `Code - Estruturar saída` por `Information Extractor`/LLM quando quiser evoluir.
