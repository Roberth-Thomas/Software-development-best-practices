# Checklist de auditoria

Ferramentas entre parênteses são exemplos para Python. Item que não se aplica ao projeto vira "n/a" com o motivo.

## Qualidade de código

- **Q1. Módulos por responsabilidade.** Confira com `wc -l` nos `.py`: arquivo acima de ~400 linhas é candidato a divisão, e módulo que mistura rede, disco e tela também.
- **Q2. Erros próprios e tipos.** Exceções do projeto num módulo só (ex.: `erros.py`) e type hints nas assinaturas públicas. Confira por `grep` e por amostra de funções.
- **Q3. Teste ao lado de cada módulo.** Uma suíte por módulo, com teste de falha e de borda, além do caminho feliz.
- **Q4. Integração com o serviço real.** Teste separado por marcador (`pytest -m integration`), rodando em agenda no CI. Confira o marcador no `pytest.ini` e o `schedule:` no workflow.
- **Q5. Migração de banco.** Teste que cria o schema de uma versão antiga e abre com o código novo (só se houver banco).
- **Q6. Dublê que imita a falha real.** Leia os fakes: onde o original lança exceção (ex.: `Cancelado`), o fake lança igual. Um fake que devolveu `[]` onde o real lança deixou passar um bug alto com a suíte inteira verde.
- **Q7. Falha nunca silenciosa.** Procure `except` seguido de `pass` ou de valor padrão (`return []`, `None`). Confira o que o usuário vê quando uma dependência cai; em app sem console, o erro precisa aparecer na tela ou no log.
- **Q8. "Não sei" é diferente de "não existe".** Onde várias fontes são consultadas, uma falhando e outra vazia vira "busca incompleta", nunca "não encontrado".
- **Q9. Cancelar cancela.** O botão Parar alcança todas as esperas (travas, rede, fila). Fechar a janela só fecha o banco depois de a fila terminar.
- **Q10. Dados e arquivos.** Transações explícitas, consultas parametrizadas, `PRAGMA user_version` para migrar, `timeout` na conexão. Arquivo gravado em temporário e renomeado (`os.replace`, que também é atômico em Linux).
- **Q11. Ferramentas automáticas.** Linter/formatador (ruff), verificador de tipos (mypy ou pyright), cobertura (pytest-cov), pre-commit e `pyproject.toml`. Confira o arquivo de configuração de cada um e rode os que existirem.
- **Q12. Documentado e versionado.** README (rodar, testar, gerar), LICENSE, CHANGELOG e tag de versão (`git tag`).
- **Q13. Tudo que o projeto usa está no repositório.** Scripts de build, de captura de tela e utilitários ficam versionados; o que mora só na máquina de quem escreveu não se reproduz.

## Segurança

- **S1. Borda única para dado externo.** Lista de domínios permitidos, só HTTPS, redirecionamentos conferidos, limite de tamanho e conteúdo validado contra a extensão, com a mesma regra para toda fonte. Procure um caminho que pule a validação (ex.: o id de uma fonte sem a checagem das outras).
- **S2. Nomes de arquivo seguros.** Sem `../`, sem nomes reservados do Windows (`CON`, `NUL`), sem sobrescrever o que já existe.
- **S3. Entrada não confiável tratada.** CSV com limites, caracteres invisíveis removidos e fórmulas neutralizadas (`=`, `+`, `-`, `@`). Texto digitado normalizado antes de ir a uma API ou ao SQL.
- **S4. Nada baixado é executado.** "Abrir" só abre tipos de documento, dentro da pasta do projeto.
- **S5. Segredos.** Nenhum no código nem no histórico: `grep` e `git log --all -S` por `api_key`, `token`, `password`, `secret`. O `.gitignore` cobre `.env` e bancos locais.
- **S6. Dependências.** Versões exatas (ideal: com hashes), Dependabot para `pip` e `github-actions`, `pip-audit` e `bandit` no CI.
- **S7. CI endurecido.** `permissions: contents: read` no topo e escrita só no job que precisa; ações fixadas por hash com a versão em comentário; `pull_request` (nunca `pull_request_target`); `persist-credentials: false`. Confira se cada hash bate com a tag do comentário.
- **S8. SECURITY.md preciso.** Diz como relatar uma vulnerabilidade (relato privado) e o que o app recusa fazer. Confira cada frase contra o código: documentação mais forte que o código é achado.
- **S9. Release verificável.** SHA-256 publicado junto do binário. Melhor ainda: atestado de procedência (`attest-build-provenance`) e tags `v*` protegidas.
- **S10. Dois efeitos sem transação.** Mover arquivo e gravar registro: se o segundo falha, o primeiro é desfeito ou fica registrado com aviso. Confira o ponto em que isso acontece.
- **S11. Configurações do GitHub.** Proteção da branch principal, secret scanning e relato privado de vulnerabilidades. Só se confere no GitHub: marque "?" e peça ao usuário para conferir em Settings.

## Como trabalhamos

- **P1. Regras escritas.** O projeto tem `CLAUDE.md` com as regras de trabalho.
- **P2. Fases pequenas.** `git log --stat -20` mostra commits pequenos, cada um com seu teste.
- **P3. Revisão arquivada.** Existe relatório de uma revisão por agentes (segurança, falhas silenciosas, código) com as correções testadas.
