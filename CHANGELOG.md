# Changelog

Todas as mudanças notáveis deste projeto serão documentadas neste arquivo.

O formato é baseado em [Keep a Changelog](https://keepachangelog.com/pt-BR/1.1.0/),
e este projeto adere ao [Versionamento Semântico](https://semver.org/lang/pt-BR/spec/v2.0.0.html).

> **RachaConta** é um aplicativo web e mobile (com API pública) para dividir despesas
> entre amigos, repúblicas e viagens em grupo: quem pagou, quem deve e quanto.

## [Unreleased]

### Adicionado
- Tela de estatísticas mensais por categoria de despesa (em desenvolvimento).

## [2.0.1] - 2026-07-15

### Segurança
- Corrigida vulnerabilidade de controle de acesso (IDOR) no endpoint
  `GET /api/v2/groups/{id}/expenses`: qualquer usuário autenticado podia consultar
  as despesas de grupos dos quais não fazia parte apenas trocando o `id` na URL.
  A atualização é **fortemente recomendada** para todas as instalações da 2.0.0.
- Tokens de acesso OAuth2 agora expiram em 1 hora (antes: 30 dias) e passam a
  ser renovados via *refresh token*.
- Atualizada a biblioteca de leitura de imagens usada pelo OCR para corrigir
  vulnerabilidade de execução remota de código via arquivos de imagem malformados.

## [2.0.0] - 2026-07-06

### Adicionado
- API v2 (`/api/v2`) com autenticação OAuth2.
- Suporte a múltiplas moedas com conversão automática pela cotação do dia.
- Subgrupos dentro de um grupo (ex.: "Viagem Floripa" › "Aluguel da casa").

### Modificado
- **BREAKING:** valores monetários na API passam a ser inteiros em centavos
  (`1050` = R$ 10,50) em vez de números decimais (`10.5`), eliminando erros de
  arredondamento de ponto flutuante.
- **BREAKING:** o campo `user` das despesas foi renomeado para `payer_id`.

### Removido
- **BREAKING:** API v1 (`/api/v1`), descontinuada na versão 1.4.0.
- **BREAKING:** autenticação por token estático (`X-Api-Token`), descontinuada na 1.4.0.
- **BREAKING:** endpoint legado de exportação `GET /api/v1/export/csv`,
  descontinuado na 1.4.0. Use `GET /api/v2/reports?format=csv`.

## [1.4.0] - 2026-04-06

### Adicionado
- Endpoint de relatórios `GET /api/v1/reports` com suporte a `format=csv` e `format=json`.
- Autenticação OAuth2 disponível em paralelo ao token estático.

### Obsoleto
- Endpoint `GET /api/v1/export/csv`: substituído por `GET /api/v1/reports?format=csv`.
  Será removido na versão 2.0.0.
- Autenticação por token estático (`X-Api-Token`): substituída por OAuth2.
  Será removida na versão 2.0.0. Requisições com esse cabeçalho passam a
  retornar o cabeçalho de aviso `Deprecation: true`.

## [1.3.0] - 2026-02-02

### Adicionado
- Campo opcional `note` em `POST /api/v1/expenses` para observações na despesa.

### Modificado
- Notificações de cobrança agora também podem ser enviadas por *push*; o e-mail
  continua sendo o canal padrão, sem necessidade de alteração por parte dos usuários.
- Algoritmo de simplificação de dívidas reescrito: até 5x mais rápido em grupos
  grandes, produzindo exatamente o mesmo resultado da versão anterior.

## [1.2.1] - 2025-12-09

### Corrigido
- OCR interpretava a vírgula decimal como separador de milhar: um café de
  "R$ 10,00" era cadastrado como R$ 1.000,00.
- OCR reconhecia fotos de gatos como notas fiscais no valor de R$ 7,00
  (sempre R$ 7,00, ninguém da equipe sabe explicar o porquê). Imagens sem texto
  agora são rejeitadas.

## [1.2.0] - 2025-12-01

### Adicionado
- Leitura de nota fiscal por foto (OCR): os itens e o valor total são
  preenchidos automaticamente na nova despesa.
- Possibilidade de atribuir itens específicos da nota a pessoas específicas.

## [1.1.1] - 2025-10-13

### Corrigido
- Despesas recorrentes configuradas para o dia 31 eram simplesmente ignoradas
  nos meses com 30 dias ou menos — fevereiro virou o "mês do aluguel grátis".
  Agora são lançadas no último dia do mês.
- Editar uma despesa recorrente criava uma nova recorrência sem apagar a antiga,
  duplicando a conta de *streaming* a cada edição.

## [1.1.0] - 2025-10-06

### Adicionado
- Despesas recorrentes (diárias, semanais ou mensais), ideais para aluguel,
  internet e assinaturas.
- Lembrete automático um dia antes do lançamento de cada recorrência.

## [1.0.2] - 2025-08-25

### Corrigido
- Clicar várias vezes seguidas no botão "Quitar dívida" registrava um pagamento
  por clique: um usuário ansioso clicou 47 vezes e passou a ter crédito de
  R$ 2.350,00 com o próprio grupo.
- Despesas registradas às 23h59 do dia 31/12 em alguns fusos horários apareciam
  com a data de 01/01/1970.
- Nomes de grupo contendo apenas emojis (ex.: "🍕🍺") travavam a tela de resumo.

## [1.0.1] - 2025-08-11

### Corrigido
- O "centavo fantasma": centavos restantes de divisões não exatas eram sempre
  atribuídos ao primeiro membro em ordem alfabética — o colega Aarão estava
  pagando R$ 0,01 a mais em todas as contas do grupo. Agora a sobra é distribuída
  de forma rotativa.
- Dividir uma despesa em um grupo sem membros exibia "Cada um paga: NaN reais".
  Agora é exibida uma mensagem de erro amigável.

## [1.0.0] - 2025-08-04

### Adicionado
- Primeira versão estável. A API pública `/api/v1` passa a ser considerada
  estável e documentada; mudanças incompatíveis a partir daqui só ocorrerão
  em uma nova versão MAJOR.
- Documentação completa da API em OpenAPI 3.

### Modificado
- Tempo de carregamento da tela de saldos reduzido em 40%.

## [1.0.0-rc.1] - 2025-07-14

### Adicionado
- Página de ajuda e perguntas frequentes.

### Corrigido
- Tema escuro exibia valores negativos em vermelho escuro sobre fundo preto,
  tornando as dívidas "invisíveis".
- QR Code Pix gerado com valor zerado quando a despesa tinha mais de 10 participantes.

## [1.0.0-beta.1] - 2025-06-23

### Adicionado
- Geração de QR Code Pix para quitar dívidas diretamente pelo aplicativo.
- Tema escuro.

## [0.4.0] - 2025-05-26

### Adicionado
- Notificações por e-mail quando alguém adiciona uma despesa em que você participa.
- Exportação das despesas do grupo em CSV (`GET /api/v1/export/csv`).

## [0.3.0] - 2025-04-28

### Adicionado
- Resumo de saldos "quem deve para quem".
- Simplificação de dívidas: reduz o número de transferências necessárias
  para zerar o grupo.

## [0.2.0] - 2025-03-31

### Adicionado
- Divisão desigual de despesas, por porcentagem ou por valor fixo.
- Categorias de despesa (mercado, transporte, moradia, lazer).

### Modificado
- Tela de nova despesa reorganizada para caber em celulares pequenos.

## [0.1.0] - 2025-03-03

### Adicionado
- Cadastro e login de usuários.
- Criação de grupos e convite de membros por link.
- Registro de despesas com divisão igualitária entre os membros do grupo.

[Unreleased]: https://github.com/equipe-rachaconta/rachaconta/compare/v2.0.1...HEAD
[2.0.1]: https://github.com/equipe-rachaconta/rachaconta/compare/v2.0.0...v2.0.1
[2.0.0]: https://github.com/equipe-rachaconta/rachaconta/compare/v1.4.0...v2.0.0
[1.4.0]: https://github.com/equipe-rachaconta/rachaconta/compare/v1.3.0...v1.4.0
[1.3.0]: https://github.com/equipe-rachaconta/rachaconta/compare/v1.2.1...v1.3.0
[1.2.1]: https://github.com/equipe-rachaconta/rachaconta/compare/v1.2.0...v1.2.1
[1.2.0]: https://github.com/equipe-rachaconta/rachaconta/compare/v1.1.1...v1.2.0
[1.1.1]: https://github.com/equipe-rachaconta/rachaconta/compare/v1.1.0...v1.1.1
[1.1.0]: https://github.com/equipe-rachaconta/rachaconta/compare/v1.0.2...v1.1.0
[1.0.2]: https://github.com/equipe-rachaconta/rachaconta/compare/v1.0.1...v1.0.2
[1.0.1]: https://github.com/equipe-rachaconta/rachaconta/compare/v1.0.0...v1.0.1
[1.0.0]: https://github.com/equipe-rachaconta/rachaconta/compare/v1.0.0-rc.1...v1.0.0
[1.0.0-rc.1]: https://github.com/equipe-rachaconta/rachaconta/compare/v1.0.0-beta.1...v1.0.0-rc.1
[1.0.0-beta.1]: https://github.com/equipe-rachaconta/rachaconta/compare/v0.4.0...v1.0.0-beta.1
[0.4.0]: https://github.com/equipe-rachaconta/rachaconta/compare/v0.3.0...v0.4.0
[0.3.0]: https://github.com/equipe-rachaconta/rachaconta/compare/v0.2.0...v0.3.0
[0.2.0]: https://github.com/equipe-rachaconta/rachaconta/compare/v0.1.0...v0.2.0
[0.1.0]: https://github.com/equipe-rachaconta/rachaconta/releases/tag/v0.1.0

---

# Apêndice: padrões de versionamento adotados pela equipe

## Versionamento Semântico (SemVer 2.0.0)

O Versionamento Semântico define que o número de versão tem o formato
**MAJOR.MINOR.PATCH** e que cada parte só é incrementada por um motivo específico:

| Parte     | Quando incrementar                                                        | Exemplo neste projeto |
|-----------|---------------------------------------------------------------------------|-----------------------|
| **MAJOR** | Mudanças **incompatíveis** com a API pública                              | 1.4.0 → 2.0.0 (remoção da API v1 e valores em centavos) |
| **MINOR** | Novas funcionalidades **compatíveis** com versões anteriores, incluindo marcar algo como obsoleto | 1.0.2 → 1.1.0 (despesas recorrentes); 1.3.0 → 1.4.0 (descontinuação) |
| **PATCH** | Correções de bugs **compatíveis** com versões anteriores                  | 1.0.0 → 1.0.1 (centavo fantasma); 2.0.0 → 2.0.1 (segurança) |

Regras importantes aplicadas neste changelog:

- **0.y.z é a fase de desenvolvimento inicial**: qualquer coisa pode mudar a
  qualquer momento e a API não deve ser considerada estável. Por isso o projeto
  começou em 0.1.0.
- **Versões de pré-lançamento** são indicadas por um hífen após o PATCH, como em
  `1.0.0-beta.1` e `1.0.0-rc.1`. Elas têm precedência **menor** que a versão normal
  associada, ou seja: `0.4.0 < 1.0.0-beta.1 < 1.0.0-rc.1 < 1.0.0`.
- **A versão 1.0.0 define a API pública estável.** A partir dela, quebrar
  compatibilidade exige obrigatoriamente um novo MAJOR.
- Ao incrementar MINOR, o PATCH volta a zero; ao incrementar MAJOR, MINOR e PATCH
  voltam a zero.
- Uma versão publicada nunca é alterada: qualquer mudança exige uma nova versão.
- O caminho recomendado para remover algo é **descontinuar primeiro** (em uma MINOR,
  seção "Obsoleto", como na 1.4.0) e **remover depois** (em uma MAJOR, seção
  "Removido", como na 2.0.0), dando tempo para os usuários da API migrarem.

## Keep a Changelog

O padrão Keep a Changelog organiza o arquivo para ser lido por **pessoas**, não
por máquinas. Seus princípios, seguidos aqui, são: a versão mais recente fica no
topo; cada versão tem sua data no formato ISO 8601 (`AAAA-MM-DD`); existe uma seção
`[Unreleased]` para o que ainda não foi lançado; os links no final permitem comparar
cada versão com a anterior; e as mudanças são agrupadas nos tipos **Adicionado**
(*Added*), **Modificado** (*Changed*), **Obsoleto** (*Deprecated*), **Removido**
(*Removed*), **Corrigido** (*Fixed*) e **Segurança** (*Security*).

## Conventional Commits

Conventional Commits é uma convenção para escrever mensagens de commit com uma
estrutura legível tanto por pessoas quanto por ferramentas:

```
<tipo>[escopo opcional]: <descrição>

[corpo opcional]

[rodapé(s) opcional(is)]
```

Os tipos principais se relacionam diretamente com o SemVer:

| Commit                                              | Efeito na versão |
|-----------------------------------------------------|------------------|
| `fix: ...`                                          | PATCH            |
| `feat: ...`                                         | MINOR            |
| `feat!: ...` ou rodapé `BREAKING CHANGE: ...`       | MAJOR            |

Outros tipos comuns (sem efeito direto na versão) são `docs`, `style`, `refactor`,
`perf`, `test`, `build`, `ci` e `chore`. Exemplos reais deste projeto:

```
fix(split): distribui centavos restantes de forma rotativa

feat(recurring): adiciona despesas recorrentes mensais

feat(api)!: representa valores monetários em centavos inteiros

BREAKING CHANGE: os campos de valor passam de decimal (10.5) para inteiro (1050).
```

Com commits padronizados, ferramentas como *semantic-release* ou
*standard-version* conseguem calcular automaticamente a próxima versão e gerar
um rascunho do changelog, que a equipe então revisa e escreve para leitura humana.

## Por que padronizar o versionamento?

A padronização transforma o número de versão em um **contrato de comunicação**.
Quem consome uma aplicação ou API sabe, só de olhar o número, o risco de atualizar:
um PATCH pode ser aplicado com tranquilidade, um MINOR traz novidades sem quebrar
nada, e um MAJOR exige leitura das notas e possivelmente adaptação do código.
Isso permite que gerenciadores de dependências (npm, pip, Maven, Composer) usem
regras como `^1.4.0` para aceitar atualizações seguras automaticamente e evita o
chamado "inferno das dependências".

Para a equipe, a padronização melhora a rastreabilidade (é fácil descobrir em qual
versão um bug entrou ou foi corrigido), facilita a revisão de código, permite
automatizar a geração de releases e changelogs e reduz discussões subjetivas sobre
"que número dar" para cada entrega. Para usuários e suporte, um changelog claro
reduz dúvidas e dá transparência, especialmente em correções de segurança, que
precisam ser comunicadas de forma rápida e inequívoca, como na versão 2.0.1.

## Conclusão da equipe

Ao construir o histórico do RachaConta, percebemos que versionar não é só "aumentar
um número": cada versão conta uma decisão tomada pela equipe. O SemVer nos obrigou a
pensar no impacto de cada mudança para quem usa a API, e o fluxo de descontinuar
antes de remover mostrou como é possível evoluir o produto sem surpreender ninguém.
O Conventional Commits liga o trabalho do dia a dia (os commits) ao resultado
publicado (as versões e o changelog), e o Keep a Changelog garante que esse
resultado seja compreensível por pessoas, não apenas por máquinas. Concluímos que
adotar esses padrões desde a versão 0.1.0 custa pouco e gera um ganho enorme em
qualidade, confiança e manutenção do software ao longo do tempo — e que até o
centavo fantasma do Aarão merece ser documentado.
