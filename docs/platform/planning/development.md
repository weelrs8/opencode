# Ambiente de desenvolvimento do fork

## Bloqueio observado nesta sessão

O clone HTTPS falhou ao conectar ao proxy configurado em `proxy:8080`.
O conector GitHub permite leitura e publicação de documentação, mas não substitui
o checkout, instalação de dependências, execução da aplicação e benchmarks.
Nenhuma alteração funcional ou benchmark foi executado nesta etapa.

## Execução quando o checkout estiver disponível

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
