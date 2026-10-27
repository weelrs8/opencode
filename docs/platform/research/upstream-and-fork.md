# Upstream e preparação do fork

Verificação realizada em 2026-10-24 pelo conector GitHub.

## Dados confirmados

- Repositório: https://github.com/anomalyco/opencode
- Clone URL: https://github.com/anomalyco/opencode.git
- Branch padrão informada: `dev`.
- Release retornada por `/releases/latest`: `v1.19.4`.
- Publicação: `2026-10-22T13:03:41Z`.
- Release: https://github.com/anomalyco/opencode/releases/tag/v1.19.4
- `target_commitish` informado pela release: `c0c7bae726798d02592f8d3305891ba2f0cd880a`.
  A referência da tag ainda deve ser resolvida antes de fixar o checkout.

Fonte: https://api.github.com/repos/anomalyco/opencode/releases/latest

“OpenCode 2” é a identificação lembrada pelo usuário e permanece a esclarecer.
Não encontramos evidência nesta consulta para escolher uma versão 2 automaticamente.

## Estado do workspace e impedimentos

O diretório `/workspace` não contém um checkout Git utilizável. A documentação foi criada
em `/workspace/opencode-platform` sem alterar os diretórios de configuração montados.

`gh auth status` rejeitou a credencial de CLI disponível. Não imprimir nem copiar seu valor.
O conector GitHub consegue ler o upstream, mas não disponibiliza operação de criação de fork.
As tentativas HTTP pela CLI falharam ao conectar ao proxy configurado, embora o serviço
de status reporte rede aplicada e conectada. Não há clone local, fork remoto ou push concluído.

## Procedimento após disponibilizar acesso

O usuário já pediu a criação do fork; não é necessário pedir a mesma autorização novamente.
Confirmar o destino somente se houver ambiguidade entre contas/organizações.

1. Resolver a referência pretendida e revisar licença e instruções AGENTS.md upstream.
2. Criar fork público na conta escolhida com recurso GitHub que suporte essa operação.
3. Clonar para um diretório novo, preservando esta documentação.
4. Configurar `origin` para o fork e `upstream` para o repositório original.
5. Criar branch de trabalho a partir da referência escolhida.
6. Copiar estes documentos para `docs/platform/` no fork, ajustando links internos e
   adicionando o índice à documentação existente sem sobrescrever arquivos upstream.
7. Registrar tag/commit reais, URL do fork e política inicial de atualização upstream.

Um download de arquivos ou `git clone` não cria um fork no GitHub. Não confundir os estados.

## Atualização — 26/10/2026 (America/Cuiaba)

O usuário criou o fork https://github.com/weelrs8/opencode. O conector confirmou
acesso de escrita e branch padrão `dev`. O fork remoto existe; o clone local ainda
falha ao conectar ao proxy. A leitura pelo conector funciona; a publicação foi testada separadamente e falhou, conforme registro abaixo.
Os impedimentos anteriores acima são um registro histórico, não o estado atual do fork.

- Base da branch documental: `dev`.
- Commit base: `0b050ac5cdd995ba8165e0699d4150b3409cfe51`.
- Árvore base: `0b388730f164af65a4446f014c08cd00ab649b2b`.
- Branch de trabalho escolhida: `platform-docs`.
- Destino da documentação: `docs/platform/`.
- Licença conferida no fork: MIT; preservar licença e atribuições existentes.
- Instruções `AGENTS.md` lidas: usar `dev` como base, nomes curtos sem barras
  e mensagens de commit convencionais.
- A referência usada nesta etapa é o `dev` do fork, sem pressupor equivalência
  com a release estável consultada anteriormente ou resolver a expressão “OpenCode 2”.

O procedimento de criação do fork já foi cumprido pelo usuário. Não recriar o fork.
O checkout e a configuração de remotes permanecem pendentes da recuperação da rede local.

### Resultado da tentativa de publicação

A criação de `platform-docs` foi recusada com HTTP 403: `Resource not accessible by integration`.
A permissão `push: true` retornada nos metadados não garantiu autorização efetiva da integração.
Nenhuma branch ou commit foi publicado. É necessário disponibilizar acesso de escrita
para a integração neste fork. Não houve revisão automática de aprovação: a recusa veio da API GitHub.

### Nova tentativa — acesso restabelecido

A criação da branch `platform-docs` foi concluída após ajuste de acesso pelo usuário.
Base confirmada: `0b050ac5cdd995ba8165e0699d4150b3409cfe51`. A documentação
segue para `docs/platform/` nessa branch. Os erros anteriores são histórico da preparação.
O clone local continua sem nova verificação nesta tentativa.

## Identificação da versão pelo código

A inspeção de `package.json` no commit base confirmou `version: 2.1.4`.
A menção do usuário a OpenCode 2 é compatível com a base do fork. A release estável
v1.19.4 consultada anteriormente não descreve a versão dessa branch.
Ver [mapeamento do código](code-map.md) para evidências e limitações da análise.
