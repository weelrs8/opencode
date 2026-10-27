# Ambiente de desenvolvimento do fork

## Estado atual — 27/10/2026 (America/Cuiaba)

Checkout disponível em `/workspace/opencode`, branch documental `platform-docs`.
Bun 1.4.4 instalado e `bun install --frozen-lockfile` concluído após liberar
`api.github.com` e `pkg.pr.new`. Arquivos de código e lockfile foram preservados.

Validação realizada sobre a base de código compartilhada com a branch documental:

- CLI inicializado e API `/api/info` respondeu HTTP 200 com autenticação.
- 10 testes selecionados do servidor passaram (auth, options e service-status).
- Lint passou com avisos existentes.
- Checagem de tipos do monorepo passou: 35 tarefas, incluindo 17 em cache.
- Frontend no navegador, chamadas a modelos e benchmark de produção não executados.

Neste ambiente cloud, carregar `source /workspace/.cloud-setup/env.sh` em cada shell
para usar Bun e diretórios XDG/cache graváveis. O Turbo em modo estrito remove essas
variáveis; a verificação usada foi `bun run lint && bun turbo typecheck --concurrency=3 --env-mode=loose`.
As instruções de instalação e inicialização foram salvas na configuração do ambiente.
Processos precisam ser iniciados novamente em futuras tarefas.

## Histórico do bloqueio inicial

O clone HTTPS falhou ao conectar ao proxy configurado em `proxy:8080`.
O conector GitHub permite leitura e publicação de documentação, mas não substitui
o checkout, instalação de dependências, execução da aplicação e benchmarks.
Nenhuma alteração funcional ou benchmark foi executado nesta etapa.

## Execução do desenvolvimento

As instruções abaixo vêm de `packages/app/AGENTS.md` da base inspecionada.
Usar o Bun indicado no package.json, atualmente 1.4.4. Instalar dependências
com o lockfile do fork antes de executar.

Backend, na raiz do repositório:

```bash
bun dev serve --port 4096
```

Frontend, em outro terminal, a partir de `packages/app`:

```bash
bun dev -- --port 4444
```

Abrir `http://localhost:4444`. A interface de desenvolvimento aponta para
`http://localhost:4096`. Conferir autenticação/CORS se houver erro de conexão.

O comando instalado `opencode web` serve como referência do produto, mas não
comprova que o código alterado do fork está sendo executado. As instruções também
alertam que `opencode dev web` usa proxy de interface remota e não mostra mudanças locais.

Não reiniciar processos já existentes da aplicação/servidor: a instrução do pacote
proíbe isso. Iniciar processos de desenvolvimento somente no contexto apropriado,
com portas disponíveis.

Antes de alterar sessão/timeline: registrar baseline de benchmark exigido pelo pacote.
O comando específico depende da área alterada e ainda precisa ser selecionado.
