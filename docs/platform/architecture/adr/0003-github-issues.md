# ADR 0003 — GitHub Issues e classificação por labels

- Data: 2026-10-27 (America/Cuiaba)
- Estado: Aceito pelo usuário na conversa
- Complementa: [ADR 0002](0002-platform-in-opencode.md)

## Decisão

Usar GitHub Issues como fonte das issues dos projetos. Classificar cada issue por
tipo de trabalho, prioridade e tamanho estimado da implementação usando labels do
GitHub. A plataforma relaciona a issue às worktrees e sessões. Não criar um cadastro
interno concorrente de issues nesta fase.

Prioridade indica urgência e ordem de atendimento. Tamanho estima esforço,
complexidade e abrangência da implementação; não representa prazo nem prioridade.
Uma issue pequena pode ser pouco urgente e uma grande pode ser prioritária.

## Classificação por labels

Cada dimensão usa um prefixo estável. Uma issue deve ter exatamente uma label
`type:*` e no máximo uma label `priority:*` e uma `size:*`. Tipo descreve a natureza
do trabalho, prioridade sua ordem de atendimento e tamanho uma estimativa da
implementação. As dimensões não devem ser inferidas umas das outras.

| Label de tipo | Uso |
| --- | --- |
| `type:bug` | Corrigir comportamento incorreto ou regressão |
| `type:feature` | Implementar uma nova capacidade para o usuário |
| `type:improvement` | Melhorar uma capacidade existente sem caracterizá-la como defeito |
| `type:docs` | Criar ou atualizar documentação |
| `type:chore` | Manutenção, dependências, automação ou organização interna |
| `type:research` | Investigar uma questão antes de definir a implementação |

Uma melhoria que corrige comportamento objetivamente incorreto é `type:bug`. Uma
mudança que introduz uma capacidade inexistente é `type:feature`. `type:improvement`
fica reservado para evolução de experiência, desempenho ou comportamento existente.
O tipo da issue não determina sozinho o tipo do commit ou do PR.

| Label de prioridade | Significado |
| --- | --- |
| `priority:urgent` | Bloqueio crítico que exige atendimento imediato |
| `priority:high` | Próximo trabalho relevante a atender |
| `priority:normal` | Trabalho planejado no fluxo habitual |
| `priority:low` | Pode aguardar sem bloquear o trabalho atual |

| Label de tamanho | Referência de implementação |
| --- | --- |
| `size:xs` | Ajuste pontual e localizado |
| `size:s` | Mudança pequena em uma área |
| `size:m` | Mudança que integra algumas partes e exige validação conjunta |
| `size:l` | Mudança ampla entre áreas; avaliar divisão |
| `size:xl` | Escopo grande ou incerto; detalhar e dividir antes de iniciar |

Ausência de prioridade ou tamanho deve aparecer como “Não definida”, sem inferir
valores pelo tipo, título, número ou posição da issue. Uma issue sem `type:*` está
“Sem tipo” e deve ser fácil de localizar para triagem. Tamanho pode ser revisto após
investigação e não deve ser convertido automaticamente em horas ou prazo.

Exemplo: uma correção pequena e urgente recebe `type:bug`, `priority:urgent` e
`size:s`. Uma funcionalidade ampla planejada recebe `type:feature`,
`priority:normal` e `size:l`.

## Consequências e pendências

- Preservar identidade estável da issue no GitHub, com host, repositório e ID;
  manter número e URL para navegação, sem usar apenas o número como chave.
- Persistir no backend a associação entre issue, projeto e worktree.
- Exibir tipo, prioridade e tamanho na lista e nos detalhes; permitir filtrar pelas
  três dimensões e ordenar por prioridade com tratamento explícito para valores não definidos.
- Labels são a fonte canônica dessa classificação. GitHub Projects pode oferecer
  visualizações adicionais depois, sem substituir ou duplicar a classificação inicial.
- Tratar labels desconhecidas e conflitos, como duas prioridades na mesma issue,
  sem escolher silenciosamente um valor. A interface deve sinalizar a necessidade de triagem.
- A leitura não deve exigir permissão de escrita. Tratar falta de acesso, paginação
  e repositórios sem issues de forma visível.
- Criar/editar issues, alterar classificações ou criar labels no GitHub são
  operações a implementar com ação explícita; este registro não as executa.
