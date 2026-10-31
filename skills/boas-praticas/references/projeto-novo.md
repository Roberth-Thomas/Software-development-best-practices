# Projeto novo do zero

Cada etapa termina num critério de conclusão. Mostre a evidência ao usuário antes da etapa seguinte.

1. **Entender.** Pergunte o que o projeto faz, quem usa e que dados de fora ele toca (rede, arquivos, CSV, texto digitado). Esses dados são a superfície de segurança. Conclusão: três respostas escritas, uma frase cada.
2. **Esqueleto.** `git init`; `.gitignore` (com `.env`, bancos locais, `__pycache__`, `dist/`); LICENSE (pergunte qual); README mínimo (o que é, como rodar, como testar); o `CLAUDE.md` do modelo; `pyproject.toml` com ruff, mypy e pytest; dependências com versão exata. Conclusão: `pytest`, `ruff check` e `mypy` rodam sem erro com um teste de fumaça.
3. **Estrutura.** Um módulo por responsabilidade (rede, arquivos, banco, regra de negócio, tela) e `erros.py` com as exceções do projeto, antes da primeira funcionalidade. Conclusão: cada módulo tem uma frase de propósito no topo e nenhum módulo de regra importa a tela.
4. **Borda de segurança.** Reúna num módulo só as regras de entrada da etapa 1 (domínios, tamanhos, formatos, normalização) e escreva primeiro os testes de recusa. Conclusão: um teste de recusa por regra, todos passando.
5. **Funcionalidades, uma por vez.** Para cada uma: teste (feliz, falha, borda) → código → commit. Para cada dependência externa, pergunte "o que o usuário vê quando ela cai?" e escreva o teste dessa resposta. Conclusão por funcionalidade: testes verdes e commit.
6. **CI.** `ci.yml` com `permissions: contents: read`, ações fixadas por hash e `persist-credentials: false`; `dependabot.yml` para `pip` e `github-actions`; teste de integração com marcador, em agenda semanal. Conclusão: os arquivos conferem com S6 e S7 do [checklist](checklist.md). A primeira execução real só acontece depois do push aprovado.
7. **SECURITY.md e versão.** SECURITY.md escrito a partir do código da etapa 4, com cada frase apontando para um teste; CHANGELOG; tag `v0.1.0`. Conclusão: nenhuma frase do SECURITY.md sem teste que a sustente.
8. **GitHub, só com aprovação.** Pergunte antes de criar o repositório e antes de qualquer push. Depois, o usuário liga em Settings a proteção da branch principal, o secret scanning e o relato privado de vulnerabilidades. Conclusão: o usuário confirma que ligou os três.

Ao fim, rode o modo **auditar** no projeto novo. Todo item deve voltar ✔ ou n/a, e os "?" restantes ficam listados.
