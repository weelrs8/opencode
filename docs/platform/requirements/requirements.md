# Requisitos iniciais

Estado: necessidades registradas em 2026-10-24; suporte atual no upstream a verificar.

## REQ-01 — Execução interativa e elevação local

Disponibilizar terminal interativo/PTY para comandos que exigem intervenção, inclusive sudo.
Permitir aprovar, negar e negar com justificativa; o agente deve receber a decisão e a justificativa.
Experiência desejada: aprovação sem digitação repetida de senha quando a configuração do ambiente permitir.

Critérios propostos: mostrar comando, diretório, usuário e ambiente; não executar enquanto
a decisão estiver pendente; cancelamento e negação devolvem resultado estruturado ao agente.
PTY e autorização são mecanismos distintos. A aprovação na interface não autentica sudo por si só.
O mecanismo de elevação e o escopo/duração das aprovações ainda precisam ser projetados.

## REQ-02 — SSH e sudo remoto

Permitir investigação de logs e erros em servidores remotos, inclusive produção,
com terminal interativo e aprovação/negação de comandos elevados.

Critérios propostos: distinguir destino local/remoto, identificar host e usuário,
encaminhar entrada interativa quando necessária e devolver negação com justificativa.
Não presumir que podemos alterar sudoers ou dispensar autenticação do servidor.
Política de credenciais, reconexão e identificação do destino: a definir.

## REQ-03 — Plan flexível

O usuário relata que o modo plan atual injeta avisos de somente leitura que impedem
escritas desejadas. Permitir documentação, ferramentas MCP selecionadas e comandos
de terminal durante o planejamento, conforme permissões configuradas.

Critérios propostos: política e prompt coerentes; documentação autorizada pode ser escrita;
alterações fora da política continuam bloqueadas ou pedem aprovação.
Caminhos permitidos, comandos e MCPs concretos serão configuráveis e ainda precisam de definição.

## REQ-04 — Permissões globais e por projeto

Preservar o sistema de permissões. Configuração do projeto deve sobrescrever a global.
O usuário não sabe se essa hierarquia já existe: verificar antes de implementar.

Critérios propostos: regras conflitantes têm resultado determinístico; interface mostra
valor efetivo e origem. Mesclagem de listas/mapas e regras de confiança do projeto são questões abertas.
Proposta: mudanças vindas do repositório que ampliem acesso exigem confiança explícita.

## REQ-05 — Web responsiva e modo desenvolvedor/depurador

Melhorar a interface web para uso entre dispositivos, preservando chat e terminal.
Oferecer modo opcional semelhante às estatísticas nerd do YouTube, exibindo detalhes
disponíveis que a interface normal resume ou não apresenta.

Proposta de conteúdo: eventos, chamadas de ferramentas, resultados, tempos, consumo informado,
falhas, tentativas, compactações e decisões de permissão; contexto/requisições quando disponíveis.
Não prometer acesso a raciocínio interno que o provider não expõe.
Dados sensíveis precisam de tratamento também no modo de depuração.

## REQ-06 — Exportação de sessões

Exportar sessões para análise e preparação de dados de treinamento/fine-tuning.
O usuário possui centenas de sessões existentes no OpenCode; considerar importação/conversão.

Critérios propostos: preservar ordem, mensagens, ferramentas, resultados e metadados disponíveis;
formato versionado, exportação seletiva e tratamento de segredos/dados sensíveis.
Separar arquivo fiel da sessão de dataset derivado e curado. Não assumir que toda sessão
é um exemplo de solução correta. Formatos e modelos de destino ainda não escolhidos.

## REQ-07 — Ambientes independentes do agente

Provisionar ambientes limpos ou preparados, idealmente em segundos com imagens disponíveis.
O próprio agente pode instalar dependências conforme necessário.
Separar disponibilidade operacional do ambiente da prontidão da aplicação.
Setup obrigatório configurado deve concluir antes de liberar o agente; preparação delegada
ao agente é outro modo explícito. Falhas devem expor logs e permitir retomada.

Projetos precisam de isolamento de arquivos, processos e serviços. Git worktree sozinha
não isola banco, portas ou processos. Granularidade de containers e persistência: a definir.

## REQ-08 — Trabalho contínuo com Git

Organizar projetos, issues detalhadas, specs, ADRs e memória; usar branches, worktrees e PRs.
Suportar múltiplos projetos e tarefas. Sessões devem ser recuperáveis entre dispositivos.
Documentação versionada e referências de origem são propostas para conhecimento duradouro.

Atualização aceita em 2026-10-27: usar GitHub Issues como fonte, com labels separadas
para tipo, prioridade e tamanho estimado da implementação. Exibir as três dimensões
na lista e nos detalhes; permitir filtros e ordenação por prioridade. Vincular a issue
à worktree e às sessões sem perder a identidade do repositório de origem. Ver
[ADR 0003](../architecture/adr/0003-github-issues.md).

## REQ-09 — Múltiplos providers e modalidades de acesso

Preservar acesso a múltiplos providers. Investigar uso do ChatGPT Plus via autenticação
compatível com a integração já utilizada no OpenCode, sem confundir assinatura com créditos de API.
Confirmar implementação, endpoints e condições aplicáveis; não assumir OAuth genérico suficiente.
Não remover caminhos existentes de autenticação durante modificações no fork.
