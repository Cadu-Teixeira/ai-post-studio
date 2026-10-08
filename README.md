# AI Post Studio

Plataforma de criação de conteúdo com **agentes de IA** que pesquisa as novidades do mercado de IA, escolhe o que vale virar post, escreve, revisa e publica no LinkedIn — com um painel de revisão humana no meio do caminho.

🔗 Em produção: [studio.caduteixeira.com](https://studio.caduteixeira.com)
📊 O desenho completo do sistema (fluxogramas, agentes, ciclo diário) está em [ARQUITETURA.md](ARQUITETURA.md).

## O problema

Produzir conteúdo técnico relevante todo dia custa caro: é preciso acompanhar dezenas de fontes (a maioria em inglês), separar o que é novidade de verdade do que é ruído, adaptar para o público brasileiro, escrever bem, checar os fatos e ainda publicar no horário certo. Feito à mão, isso consome horas por dia — e a consistência é a primeira coisa a cair.

## A solução

Um pipeline em etapas, onde cada etapa tem um responsável claro (um agente de IA ou uma regra), e o humano só entra para aprovar:

- **Pesquisa diária automática:** todo dia de manhã o sistema lê fontes de notícias de IA (internacionais e brasileiras), remove duplicatas e descarta o que tem mais de 48h.
- **Pontuação de relevância:** cada artigo recebe uma nota calibrada por tema (agentes, MCP, automação, LLMs etc.), por atualidade e por fonte. Só os melhores seguem.
- **Editor-chefe de IA:** um modelo de linguagem escolhe os 3 artigos mais relevantes e devolve uma análise estruturada (impacto, se vale post, público-alvo, por que importa, aplicação prática e gancho para o LinkedIn).
- **5 agentes com papéis definidos:** Estrategista, Redator (ghostwriter), Checador de fatos, Designer e Curador — cada um com uma responsabilidade, em vez de um único prompt que tenta fazer tudo.
- **Geração por tema:** além da pauta diária, é possível pedir posts sobre um tema específico, com pesquisa na web antes de escrever.
- **Cockpit de revisão:** painel web onde os rascunhos aparecem para aprovar, editar, agendar ou descartar. Nada é publicado sem passar por ali (ou por uma regra explícita de publicação automática).
- **Publicação agendada:** um agendador roda de hora em hora, publica o que está vencido e respeita a cadência configurada.

## Decisões técnicas que valem destacar

- **Humano no circuito (human-in-the-loop):** a IA acelera, mas a decisão final de publicar é do autor. Isso protege a reputação de quem posta.
- **Laço de autocorreção:** a geração automática valida a própria saída e tenta de novo quando o resultado vem fora do formato esperado.
- **Tolerância a falhas:** se uma fonte de notícias cai, as outras continuam. Chamadas à API de IA têm nova tentativa em caso de sobrecarga.
- **Saída estruturada:** os agentes devolvem JSON com campos definidos, o que permite exibir, filtrar e reaproveitar o conteúdo no painel sem "parsing" frágil de texto livre.

## Stack

n8n (self-hosted em VPS, Docker) · Claude API (escrita, checagem de fatos e pesquisa web) · React + TypeScript (cockpit) · Supabase / PostgreSQL · API do LinkedIn · feeds RSS · webhooks

## Status e próximos passos

- ✅ Pesquisa diária, pontuação, geração automática, geração por tema, cockpit e publicação de posts de texto e imagem no LinkedIn — em produção.
- 🔄 Integração com Metricool para publicar também em Instagram e TikTok — implementada, em ativação.
- 🔜 Camada de pesquisa diária alimentando **todos** os tipos de post (hoje a pesquisa prévia na web cobre parte deles).
- 🔜 Análise automática de vídeos como fonte de pauta.
- ⚠️ Carrosséis (documento PDF) são publicados manualmente: a API do LinkedIn não aceitou esse formato nos testes feitos.

## Sobre este repositório

Este é um projeto real, em produção, usado no dia a dia. O código-fonte completo (prompts, regras de pontuação, credenciais e integrações) é privado. Este repositório público documenta a arquitetura e as decisões técnicas — a parte que interessa para entender como o sistema foi pensado e construído, sem expor a operação.

---
Feito por **Cadu Teixeira** · [caduteixeira.com](https://caduteixeira.com)
