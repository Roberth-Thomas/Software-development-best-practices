# Regras que valem em todo projeto

Responda e comente em português do Brasil. Identificadores seguem o idioma que o projeto já usa.

## Como trabalhamos

- Divida o trabalho em fases pequenas. Cada fase termina com teste passando e um commit, e a próxima só começa com a aprovação do usuário.
- Peça aprovação explícita antes de qualquer ação que sai da máquina: push, PR, release, configuração do GitHub.
- Relate o que foi verificado e o que ficou sem verificar. "Passou" só vale com a saída do teste vista nesta sessão.
- O usuário está aprendendo a automatizar com IA: explique em uma ou duas frases o porquê de cada decisão não óbvia.

## Qualidade de código

- Escreva o teste junto de cada função nova: caminho feliz, falha e borda. Todo bug corrigido ganha um teste que falha antes da correção.
- O dublê de teste imita a falha real do original. Se o real lança exceção, o falso lança a mesma.
- Todo `except` registra o erro ou o mostra ao usuário. Ausência de informação ("a fonte caiu") nunca vira resposta ("não existe").
- Um módulo por responsabilidade, exceções próprias num módulo só, type hints nas assinaturas.

## Segurança

- Valide todo dado externo (rede, arquivo, CSV, texto digitado) na borda, com a mesma regra para todas as fontes.
- Segredos ficam fora do código e do git; o `.gitignore` existe antes do primeiro commit.
- Dois efeitos sem transação (arquivo + linha no banco) precisam de desfazer explícito.
- Dependências com versão fixa; no CI, permissões mínimas e ações fixadas por hash.

## Skill

Para auditar um projeto existente ou iniciar um novo, o usuário chama `/boas-praticas`.
