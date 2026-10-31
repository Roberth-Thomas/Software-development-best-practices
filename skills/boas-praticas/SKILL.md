---
name: boas-praticas
description: Audita um projeto existente contra o checklist de boas práticas (qualidade, segurança, como trabalhamos) ou inicia um projeto novo com o esqueleto certo.
argument-hint: "[auditar|iniciar]"
disable-model-invocation: true
---

# Boas práticas

Dois modos: **auditar** um projeto existente ou **iniciar** um projeto novo. Se o Roberth não disse qual, pergunte com AskUserQuestion (menu clicável).

## Auditar

A auditoria só lê. Nada no projeto muda até o Roberth aprovar as correções.

1. Leia `README.md`, `SECURITY.md`, os workflows de CI, a lista de arquivos e `git log --oneline -20`. Conclusão: você sabe a stack e o que o projeto afirma fazer.
2. Percorra [references/checklist.md](references/checklist.md) item a item. Cada item recebe um status (✔ tem, ✘ falta, ? não verificado, n/a não se aplica) e uma **evidência**: `arquivo:linha`, saída de comando ou o motivo de não ter conseguido conferir. Item sem evidência é "?".
3. Rode os testes do projeto uma vez e cite o resultado. Sem execução, o item de testes fica "?".
4. Entregue o relatório no chat. Grave em arquivo só se o Roberth pedir.

A auditoria termina quando todos os itens do checklist têm status e evidência. O que só se confere fora da máquina (configurações do GitHub) fica "?" com o pedido "confira em Settings → …"; a API do GitHub só entra com aprovação.

Para uma auditoria funda, o Roberth pode pedir revisão por agentes: `security-reviewer`, `silent-failure-hunter` e `python-reviewer` em paralelo, cada um com o checklist da sua área. Corrija primeiro o que **dois ou mais** apontaram; o que só um apontou entra no relatório depois de você conferir o trecho no código.

### Formato do relatório

- **Resumo**: uma frase e a contagem de ✔ / ✘ / ?.
- **O que falta**, por prioridade. Alto: o app diz algo diferente do que aconteceu, perde dados ou tem falha de segurança. Médio e Baixo: o resto. Cada linha traz o item, a evidência e a correção em uma frase.
- **Não verificado**: cada "?" com o motivo.
- **Próximas fases**: 3 a 5 fases pequenas (teste + commit), em ordem de prioridade. Pergunte por onde começar com AskUserQuestion.

## Iniciar

Siga [references/projeto-novo.md](references/projeto-novo.md), etapa por etapa. Cada etapa tem um critério de conclusão: mostre a evidência ao Roberth antes de passar à seguinte. As decisões dele (linguagem, licença, se vai para o GitHub) saem em AskUserQuestion.
