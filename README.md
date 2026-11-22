# WhatsApp Chip Manager — Painel White-Label para Evolution API v2

Interface SaaS mobile-first para conexão via QR Code, pareamento e gestão de múltiplos chips/instâncias de WhatsApp integrada nativamente à **Evolution API v2**.

## Segurança e Zero Credenciais no Código (White-Label)
Este repositório foi projetado como produto SaaS white-label:
- **Zero segredos ou dados pessoais no código-fonte**: nenhuma URL privada, chave de API (`apikey`), nome ou telefone está gravado no código.
- **Variáveis de ambiente / Runtime Config**:
  - Se hospedado com build ou injeção no servidor, utiliza as variáveis definidas em `.env.example` (`VITE_EVO_API_URL`, `VITE_EVO_API_KEY`, `VITE_DEFAULT_WEBHOOK_URL`) ou `window.__ENV__`.
  - Se hospedado como página estática (ex.: GitHub Pages / Cloudflare Pages), a tela de login grava a `Server URL` (`apiUrl`) e a `Global API Key` (`token`) exclusivamente no `localStorage` do navegador do operador, utilizando exatamente as mesmas chaves (`apiUrl`, `token`, `version`, `clientName`, `instanceId`, `instanceName`, `instanceToken`) do Evolution Manager oficial.

## Endpoints Nativos (Compatibilidade 1:1 com Evolution API v2)
- `GET /` e `POST /verify-creds` (Header `apikey`) — Validação do servidor, versão da Evolution API e autenticação da Global API Key.
- `GET /instance/fetchInstances` e `GET /instance/fetchInstances?instanceId=<id>` — Listagem completa de todas as instâncias, perfil conectado, avatar, JID, tokens, contadores e códigos de desconexão.
- `GET /instance/connectionState/<instanceName>` — Verificação em tempo real do estado do socket (`open`, `connecting`, `close`).
- `GET /instance/connect/<instanceName>` (e `?number=<numero>`) — Geração instantânea de **QR Code** (`base64` / `code`) e **Pairing Code** de 8 dígitos (`XXXX-XXXX`), com polling automático de pareamento a cada 3 segundos.
- `POST /instance/restart/<instanceName>` — Reinício do socket da instância sem perder a sessão.
- `DELETE /instance/logout/<instanceName>` — Desconexão limpa da sessão atual do aparelho.
- `POST /instance/create` e `DELETE /instance/delete/<instanceName>` — Criação de novas instâncias (`WHATSAPP-BAILEYS`, `WHATSAPP-BUSINESS`, `EVOLUTION`) e exclusão com confirmação.
- `GET /settings/find/<instanceName>` e `POST /settings/set/<instanceName>` — Configuração de comportamento (rejeitar chamadas, ignorar grupos, sempre online, ler mensagens, sincronizar histórico).
- `GET /webhook/find/<instanceName>` e `POST /webhook/set/<instanceName>` — Configuração de webhook (ex.: n8n `MESSAGES_UPSERT`, `base64`, `byEvents`).

## Roadmap Reservado na Interface (Módulos Futuros)
- **Limite Diário por Chip**: controle de teto diário de abordagens e janelas de horário comercial.
- **Aquecimento de Chip (Warm-up)**: rampa progressiva automática para números recém-conectados.
- **Fila de Pendentes & Cadência**: distribuição balanceada (round-robin) e intervalo humanizado anti-bloqueio.
