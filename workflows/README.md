# Workflows importáveis para n8n (Outlook + Google Alerts) — v2

Este pacote contém templates para importar no n8n e acelerar a automação do clipping.

## Arquivos

1. `01_outlook_alertas_ingestao.json`
2. `02_enriquecimento_deduplicacao.json`
3. `03_fechamento_semanal_email.json`
4. `base_noticias_headers.csv` (cabeçalhos sugeridos para a aba `Base_Noticias`)

## 1) Importação no n8n

1. Abra **Workflows** → **Import from File**.
2. Importe os 3 arquivos `.json`.
3. Em cada workflow, ajuste os valores `__SET_ME__`.
4. Salve e rode manualmente 1 vez antes de ativar.

## 2) Credenciais necessárias

### Outlook IMAP (entrada de e-mails)

- Host: `outlook.office365.com`
- Porta: `993`
- Segurança: `SSL/TLS`
- Usuário: seu e-mail completo (`Estevao.eller@fgv.br`)

### Outlook SMTP (envio do clipping)

- Host recomendado: `smtp.office365.com`
- Porta: `587`
- Segurança: `STARTTLS`
- Usuário: seu e-mail completo (`Estevao.eller@fgv.br`)

> Observação: em alguns tenants funciona `smtp-mail.outlook.com`; se o envio falhar, teste esse host alternativo.

### Google Sheets OAuth2

- Conectar conta Google que possui a planilha.
- Preencher `documentId` no lugar de `__SET_ME__` em todos os nodes de planilha.

## 3) Estrutura da planilha

Crie a aba `Base_Noticias` e cole a linha de cabeçalhos do arquivo `base_noticias_headers.csv`.

Colunas mínimas esperadas:

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
- `sent_at`
- `batch_id`

## 4) Fluxo recomendado de ativação

1. Ativar workflow `01_outlook_alertas_ingestao`.
2. Validar se novas linhas entram na planilha.
3. Ativar workflow `02_enriquecimento_deduplicacao`.
4. Revisar se `status` está sendo atualizado corretamente.
5. Ativar workflow `03_fechamento_semanal_email`.

## 5) Checklist de troubleshooting

- Não lê e-mail: valide IMAP + senha/app password + MFA da conta.
- Não envia e-mail: valide SMTP host/porta/STARTTLS.
- Não escreve na planilha: validar `documentId` e escopo OAuth do Google Sheets.
- Muitos duplicados: revisar normalização de URL no node `Code - Normalizar URL e hash`.

## 6) Importante

- Os workflows vêm **desativados** (`active: false`).
- O node de enriquecimento está em modo base; você pode trocar por `Information Extractor`/LLM depois.
