# 💬 Enterprise WhatsApp AI Gateway & RAG Connector (Node.js)

<p align="center">
  <img src="https://raw.githubusercontent.com/andreasbm/readme/master/assets/lines/rainbow.png" width="100%" alt="divider" />
</p>

<p align="center">
  <strong>Production-ready, self-hosted WhatsApp API Gateway & Conversational RAG Connector engineered with Node.js, NestJS / Fastify, TypeScript, Baileys, Redis BullMQ, and PostgreSQL.</strong>
</p>

<p align="center">
  <a href="#-architecture--conversational-flow"><img src="https://img.shields.io/badge/Architecture-WhatsApp%20%2B%20AI%20RAG-blueviolet?style=for-the-badge&logo=diagramsdotnet" alt="Architecture" /></a>
  <a href="#-key-features"><img src="https://img.shields.io/badge/Features-Multi--Session%20%7C%20AI%20Agents-informational?style=for-the-badge&logo=node.js" alt="Features" /></a>
  <a href="LICENSE"><img src="https://img.shields.io/badge/License-MIT-green?style=for-the-badge&logo=open-source-initiative" alt="License: MIT" /></a>
</p>

<p align="center">
  <img src="https://img.shields.io/badge/Node.js-20.x%2B-339933?style=flat-square&logo=node.js&logoColor=white" alt="Node" />
  <img src="https://img.shields.io/badge/TypeScript-5.x-3178C6?style=flat-square&logo=typescript&logoColor=white" alt="TypeScript" />
  <img src="https://img.shields.io/badge/WhatsApp-Baileys%20Engine-25D366?style=flat-square&logo=whatsapp&logoColor=white" alt="WhatsApp" />
  <img src="https://img.shields.io/badge/Redis-BullMQ%20Queue-DC382D?style=flat-square&logo=redis&logoColor=white" alt="Redis BullMQ" />
  <img src="https://img.shields.io/badge/PostgreSQL-Session%20%26%20Chat%20Store-4169E1?style=flat-square&logo=postgresql&logoColor=white" alt="PostgreSQL" />
  <img src="https://img.shields.io/badge/AI%20RAG-Connected-412991?style=flat-square&logo=openai&logoColor=white" alt="AI RAG" />
  <img src="https://img.shields.io/badge/Docker-Ready-2496ED?style=flat-square&logo=docker&logoColor=white" alt="Docker" />
</p>

---

## 📌 Visão Geral & Proposta de Valor

O **Enterprise WhatsApp AI Gateway** é uma infraestrutura completa, open-source e auto-hospedada (self-hosted) para integração do **WhatsApp com motores de Inteligência Artificial Generativa, RAG Híbrido e Agentes Autônomos**.

Elimina custos de APIs de terceiros e limites abusivos de provedores SaaS, fornecendo **multi-sessões simultâneas via QR Code**, envio assíncrono blindado contra bloqueios com **Redis & BullMQ**, transcrição de áudios com **Whisper** e respostas corporativas em tempo real conectadas ao **PostgreSQL + pgvector**.

---

## 🏛️ Arquitetura do Sistema & Fluxo Conversacional

```text
+---------------------------------------------------------------------------------------------------------+
|                                    WHATSAPP NETWORK & END CLIENTS                                       |
+---------------------------------------------------------------------------------------------------------+
                                                     │
                                                     │ Real-time WhatsApp Protocol (Baileys Engine)
                                                     ▼
+---------------------------------------------------------------------------------------------------------+
|                               ENTERPRISE WHATSAPP AI GATEWAY (Node.js)                                  |
|                                                                                                         |
|   • Multi-Session Manager (QR Code)  • Webhook Inbound Dispatcher  • Security & Anti-Throttle Limiter   |
+---------------------------------------------------------------------------------------------------------+
                                                     │
                                                     │ Ingest Message Event
                                                     ▼
+---------------------------------------------------------------------------------------------------------+
|                                     REDIS BULLMQ ASYNC OUTBOX & INBOX                                   |
|                                                                                                         |
|   • Priority Inbound Queue      • Outbox Anti-Ban Scheduler       • Message Retry & Dead-Letter         |
+---------------------------------------------------------------------------------------------------------+
                                                     │
                                                     ▼
+---------------------------------------------------------------------------------------------------------+
|                                 INTELLIGENT AI & RAG ROUTING LAYER                                      |
|                                                                                                         |
|   1. Audio Voice Note ───► Whisper Audio Transcriber ───► Text Normalized                               |
|   2. User Text Query  ───► Enterprise RAG Node (Hybrid Search pgvector + BM25)                          |
|   3. Complex Actions  ───► Multi-Agent Orchestrator Node (Supervisor & Researcher)                      |
|   4. Human Escalation ───► Live Human Takeover Webhook                                                  |
+---------------------------------------------------------------------------------------------------------+
                                                     │
                                                     │ Contextual AI Response
                                                     ▼
+---------------------------------------------------------------------------------------------------------+
|                                  OUTBOX DISPATCHER (Baileys Outbound)                                   |
|                Envia mensagens de texto, áudios e documentos de volta para o cliente                    |
+---------------------------------------------------------------------------------------------------------+
```

---

## 🤖 Capacidades & Módulos

| Módulo | Tecnologia | Responsabilidade |
|---|---|---|
| **Session Core** | `Baileys / Multi-Device` | Gerenciamento de QR Code, reconexão automática e persistência de sessões. |
| **Outbox Queue** | `Redis / BullMQ` | Agendamento de envios com delay humanizado para evitar banimentos. |
| **RAG Connector** | `Fastify / REST API` | Conexão com o motor `OPENSOUNCER-RAG` para respostas com dados de PDFs/DOCX. |
| **Media Engine** | `FFmpeg + Audio Parser` | Transcrição de áudio recebido e compressão de mídias enviadas. |
| **Human Takeover** | `WebSockets + Webhook` | Pausa no bot para intervenção imediata de atendente humano. |

---

## 🧠 Diagrama de Sequência (Atendimento Inteligente com RAG)

```mermaid
sequenceDiagram
    autonumber
    actor Cliente as Usuário no WhatsApp
    participant GW as WhatsApp Gateway Core
    participant Queue as Redis (BullMQ Queue)
    participant RAG as Enterprise RAG Engine
    participant Human as Atendente Humano

    Cliente->>GW: Envia Mensagem / Áudio ("Qual o status do meu pedido?")
    GW->>Queue: Enfileira evento recebido
    Queue->>RAG: Consulta Contexto na Base Híbrida (pgvector)
    
    alt Resposta Encontrada com Alta Relevância
        RAG-->>Queue: Retorna Resposta Contextualizada
        Queue->>GW: Dispara envio de mensagem humanizada
        GW-->>Cliente: Responde no WhatsApp em segundos
    else Dúvida Crítica / Reclamação
        Queue->>Human: Notifica Atendente (Human Takeover)
        Human->>GW: Atendente responde diretamente
        GW-->>Cliente: Resposta do Especialista Humano
    end
```

---

## 🛠️ Tech Stack & Dependências

- **Runtime**: Node.js 20+ & TypeScript 5.x
- **WhatsApp Engine**: `@whiskeysockets/baileys`
- **Fila & Agendamento**: Redis 7 + BullMQ
- **Armazenamento**: PostgreSQL 16 / SQLite Multi-Session Storage
- **Conexão de IA**: OpenAI / Ollama Local / Enterprise RAG Engine
- **Testes & CI/CD**: Vitest / Jest + GitHub Actions

---

## ⚙️ Quickstart & Instalação

```bash
# 1. Clonar e Instalar Dependências
git clone https://github.com/dev-rodrigues00/OPENSOUNCER-whatsapp-ai-node.git
cd OPENSOUNCER-whatsapp-ai-node
npm install

# 2. Configurar Variáveis de Ambiente
cp .env.example .env

# 3. Subir Infraestrutura e Iniciar
docker compose up -d
npm run start:dev
```

---

## 🛡️ Licença

Distribuído sob a licença **MIT Open Source**. Consulte o arquivo [LICENSE](LICENSE) para mais detalhes.

---

<p align="center">
  <sub>Desenvolvido por <strong>Dev Rodrigues</strong> • Enterprise Conversational AI & WhatsApp Gateway 2026. 🚀</sub>
</p>
