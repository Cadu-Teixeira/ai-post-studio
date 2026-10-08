# Arquitetura visual

Este documento é o "desenho" do AI Post Studio: as etapas do pipeline, os agentes e o ciclo de um dia. Serve para entender o sistema sem ver uma linha de código.

Veja também o [README.md](README.md) (visão geral em texto).

## 1. Panorama geral

```mermaid
flowchart LR
A[Fontes RSS<br/>internacionais + BR] --> B[Pesquisa Diária<br/>dedup + filtro 48h]
B --> C[Pontuação de<br/>relevância]
C --> D[Editor-chefe IA<br/>TOP 3 + análise]
D --> E[Agentes de escrita<br/>e revisão]
T[Geração por Tema<br/>+ pesquisa web] --> E
E --> F[(Banco de dados<br/>rascunhos)]
F --> G[Cockpit<br/>revisão humana]
G -->|aprovado / agendado| H[Agendador<br/>de hora em hora]
H --> I[LinkedIn]
```

## 2. Os 5 agentes

Cada agente tem uma única responsabilidade. Isso deixa os prompts menores, mais fáceis de ajustar e de testar.

```mermaid
flowchart TB
S[Estrategista<br/>define ângulo e tipo de post] --> W[Redator<br/>escreve no tom do autor]
W --> FC[Checador de fatos<br/>confere afirmações]
FC --> DS[Designer<br/>gera a imagem do post]
R[Curador / Radar<br/>traz pautas e referências] --> S
DS --> OUT[Rascunho pronto<br/>no cockpit]
```

## 3. Pontuação de relevância

Antes de gastar chamada de IA, uma regra simples separa o que importa:

```mermaid
flowchart LR
A[Artigo] --> B{Tema}
B -->|agentes, MCP, automação| P1[+10]
B -->|ferramentas de dev e dados| P2[+8]
B -->|LLMs, multimodal| P3[+6]
A --> C[Bônus de atualidade]
A --> D[Bônus por fonte]
P1 & P2 & P3 & C & D --> N[Nota final]
N --> TOP[Melhores seguem<br/>para o editor-chefe]
```

## 4. Geração automática com autocorreção

```mermaid
flowchart LR
A[Pauta do dia] --> B[Gerar rascunho]
B --> C{Saída no<br/>formato esperado?}
C -->|sim| D[Salvar no banco]
C -->|não| E[Nova tentativa<br/>com correção]
E --> C
```

## 5. Linha do tempo de um dia

```mermaid
flowchart LR
A[07h<br/>Pesquisa diária] --> B[08h<br/>Geração automática]
B --> C[Durante o dia<br/>revisão no cockpit]
C --> D[De hora em hora<br/>publicação do que está agendado]
```

Os diagramas usam Mermaid e são renderizados direto pelo GitHub.
