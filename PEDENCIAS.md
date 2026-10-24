# Pendências e prompts de implementação

## Como usar

Este arquivo separa o **estado observado** do comportamento esperado em [REQUISITOS_BACKEND.md](REQUISITOS_BACKEND.md). Cada seção contém um prompt para uma tarefa independente. Nenhuma correção abaixo foi executada nesta revisão documental.

O frontend é considerado conforme o levantamento fornecido pelo responsável, não como resultado de testes diretos neste repositório. Os termos `viewer` e subtarefa aparecem aqui apenas para identificar código existente a retirar do escopo.

### Instruções comuns para todos os prompts

> Leia `AGENTS.md`, os requisitos e as convenções antes de começar. Confira a branch, alterações locais, Issue e PRs relacionados; não descarte trabalho existente. Faça somente a tarefa escolhida. Mudanças de autenticação, dependências, migrations, configurações globais e contratos compartilhados exigem aprovação do responsável. Defina entradas, saídas, permissões e compatibilidade antes de editar. Não altere migrations históricas nem apague dados sem plano aprovado. Valide com testes de sucesso, falha e isolamento, atualize o OpenAPI e relate resultados reais. Não declare uma integração funcionando apenas porque existe tela, modelo ou resposta simulada.

## Ordem sugerida

1. P-01, P-02 e P-03: sessão, cadastro e organização pessoal.
2. P-04 e P-05: retirada de papéis e hierarquia de tarefas, após aprovação da migração de dados.
3. P-06 e P-07: isolamento de acesso e WebSocket.
4. P-08 a P-13: integração das telas e consistência operacional, conforme prioridade acordada.
5. P-14 e P-15: atualização documental e operação.
6. P-16: decisão de escopo, sem implementação automática.

## P-01 — Sessões, cookies e troca de e-mail

**Estado observado:** `LoginView` devolve `access` e `refresh` também no JSON. `EmailChangeService.confirm_change` troca o endereço sem revogar sessões. Desativação só altera `is_active`; redefinição não revoga explicitamente os registros de sessão. O frontend pede cookies HttpOnly sem tokens no JavaScript.

**Arquivos iniciais:** `core/views/user.py`, `core/services/user.py`, `core/services/email_change.py`, `core/services/password_reset.py`, `core/utils/cookies.py`, `core/spectacular_serializers.py`.

**Prompt:**

> Corrija a política de sessão conforme RNF-01 a RNF-06 e RN-08 a RN-13. Após aprovação do contrato, retire os JWTs do corpo das respostas de login e renovação, preservando cookies e CSRF. Confirme a troca de e-mail atomicamente, consuma o código e revogue todas as sessões, incluindo a atual. Remova cookies na resposta e faça o frontend exigir novo login. Aplique revogação explícita também após redefinição de senha e desativação. Preserve logout e revogação de outros dispositivos. Documente que a correção não está concluída enquanto sockets antigos mantiverem acesso; coordene com P-07. Não amplie privilégios nem aceite identidade enviada pelo cliente.

**Aceite:**

- Login mantém cookies HttpOnly e não retorna tokens no JSON.
- Código ou senha inválidos não alteram e-mail nem revogam sessões.
- Sucesso invalida acesso e atualização anteriores em todos os dispositivos.
- Alteração de uma conta não afeta sessões de outra.
- Cliente limpa estado da conta e trata a necessidade de novo login sem laço de renovação.

## P-02 — Reenvio de código e identificação por e-mail

**Estado observado:** reenvio de cadastro troca o hash, mas não reinicia `used`, `used_at`, `attempts` e `expires_at`. Consultas por e-mail usam critérios diferentes entre cadastro, login e convites.

**Arquivos iniciais:** `core/models/one_time_code.py`, `core/services/registration.py`, `core/services/email_change.py`, `core/services/google_auth.py`, `core/models/user.py`.

**Prompt:**

> Corrija reenvio de cadastro para gerar código novo, invalidar o anterior, renovar validade, zerar tentativas e remover marcações de consumo. Preserve resposta que não revela existência de conta na recuperação. Defina com o responsável uma política única de comparação de e-mail; antes de impor unicidade sem distinguir maiúsculas, identifique duplicações e apresente plano de tratamento. Não una contas nem altere a propriedade delas automaticamente. Preserve os limites de requisição; espera de 45 segundos na interface não substitui proteção do servidor.

**Aceite:** código antigo falha; novo código funciona após expiração, consumo ou esgotamento anteriores; quinta falha bloqueia; repetição após confirmação não reaplica a operação; concorrência não produz duas confirmações válidas.

## P-03 — Organização pessoal automática

**Estado observado:** a organização pessoal só é garantida em `OrganizationService.list_for`, por `ensure_personal`. Cadastro local e primeiro login Google não a provisionam diretamente.

**Arquivos iniciais:** `core/services/registration.py`, `core/services/google_auth.py`, `core/modules/organizations/services/organization.py`.

**Prompt:**

> Implemente RN-03: ao concluir cadastro local ou primeiro acesso Google, garanta uma organização pessoal e vínculo proprietário, sem exigir consulta posterior. Use transação e proteção contra duplicação; proponha restrição de banco se necessária e peça aprovação antes da migration. Planeje recuperação para contas antigas sem espaço pessoal. Preserve o cadastro sem obrigação de configurar organização de equipe e não aceite convites automaticamente. A confirmação repetida não deve gerar outra organização. Não imponha exclusividade de organização de equipe ao usuário.

**Aceite:** cadastro confirmado tem organização pessoal; conta ainda não confirmada não recebe efeitos indevidos; Google também provisiona; falha não deixa vínculo incompleto; repetição e concorrência não duplicam espaços.

## P-04 — Retirar o papel `viewer`

**Estado observado:** `viewer` é um papel ativo de organização e setor, usado em convites, vínculos de times, validação de acesso e testes. O levantamento do frontend ainda oferece leitor, contrariando a decisão mais recente do responsável.

**Arquivos iniciais:** `core/modules/organizations/models/organization.py`, `core/modules/kanban/models/sector.py`, `core/modules/kanban/services/sector.py`, módulos `teams`, `invitations`, `chat`, `search` e `calendar_sync`, seus serializers e testes.

**Prompt:**

> Retire o papel atribuível `viewer` da API e do frontend. Organização terá `member`, `admin` e `owner`; setor terá `editor` e `manager`. Antes de alterar dados, inventarie vínculos e convites existentes e peça decisão sobre conversão, revogação ou manutenção temporária. Não transforme leitor em editor/membro automaticamente: isso amplia privilégios. Preserve, sujeito à confirmação do responsável, a consulta sem escrita em setores não privados como regra de visibilidade, não como um novo papel. Atualize cálculo de acesso efetivo, participantes do chat, convites com setores, vinculação de times, Drive, Calendar, filtros e schemas. Planeje rollout entre backend e frontend e rejeite novas entradas com o valor removido. Não remova a privacidade de setores para simplificar a mudança.

**Aceite:**

- Nenhum contrato ativo oferece `viewer` ou leitor como papel selecionável.
- Registros antigos são tratados por plano aprovado, com migration testada em banco contendo dados.
- Setor privado continua protegido; consulta não implica escrita.
- Só membros atuais da organização recebem conteúdo e eventos.
- Testes cobrem a matriz nova de acesso, sem apenas apagar testes antigos de segurança.

## P-05 — Retirar a hierarquia de subtarefas

**Estado observado:** `Task.parent`, `parent_id`, filtros `parent`/`parent__isnull` e desativação de filhos diretos continuam ativos. A sincronização com Calendar é de tarefa comum e não depende dessa hierarquia.

**Arquivos iniciais:** `core/modules/kanban/models/task.py`, `core/modules/kanban/serializers/task.py`, `core/modules/kanban/services/task.py`, `core/modules/kanban/views/task.py`, testes, fixtures e consumidores do contrato.

**Prompt:**

> Remova a funcionalidade de subtarefas do contrato de tarefas. Antes de remover campo ou relação, apresente quantidade de registros afetados e plano aprovado para preservar cada tarefa, seu responsável, prazo, coluna, comentários e links. Não exclua tarefas filhas por conveniência. Retire campos e filtros de hierarquia, comportamento de exclusão em cadeia no service e mensagens obsoletas. Não remova `parent` usado em estilos PDF, pasta do Drive ou resposta de chat: são outros conceitos. Preserve checklist e sincronização de prazo com Calendar, que não são subtarefas. Revise progresso, busca e fixtures para não depender de hierarquia. Remova o campo físico apenas com aprovação e compatibilidade de rollout.

**Aceite:** API e frontend não oferecem hierarquia de tarefas; dados existentes são preservados conforme o plano; cada tarefa continua gerenciável de forma independente; Calendar, comentários, checklist, busca e indicadores seguem funcionando.

## P-06 — Remoção de membros e referências privadas no chat

**Estado observado:** saída da organização não limpa todos os vínculos associados; alguns destinatários WebSocket são obtidos diretamente de vínculos de times ou conversas. Tarefa citada é validada para o autor, não para todos os leitores do canal.

**Arquivos iniciais:** services de `organizations`, `teams` e `kanban`; `core/modules/chat/services/chat.py`, `core/modules/chat/serializers/chat.py`, serviço de busca e notificações.

**Prompt:**

> Garanta RNF-04 ao remover membro, desfazer vínculo de time/setor ou citar tarefa no chat. Calcule destinatários com participação e estado atuais; nenhuma sobra de vínculo deve conceder acesso. Defina limpeza transacional ou filtragem equivalente sem apagar histórico necessário. Para referências a tarefas privadas, adote contrato aprovado: rejeitar citação incompatível com o canal ou ocultar dados por destinatário também em serialização, busca e eventos. Revalide no envio, não apenas na composição da mensagem. Coordene com a retirada de `viewer` e não restaure participação removida ao aceitar convite antigo.

**Aceite:** pessoa removida não consulta nem recebe mensagens/push de conteúdo inacessível; destinatário sem acesso não recebe título/status da tarefa privada; permissões são recalculadas após alterações de vínculos; histórico permitido permanece íntegro.

## P-07 — Revogação e distribuição de WebSocket

**Estado observado:** JWT e sessão são validados na conexão; o consumer não revalida continuamente. Camada de canais é local ao processo; presença usa contador com expiração de 12 horas.

**Arquivos iniciais:** `core/websockets/`, `core/modules/chat/presence.py`, `app/asgi.py`, configurações e Compose.

**Prompt:**

> Defina como conexões abertas perdem acesso após logout, revogação, expiração, desativação e remoção organizacional. Implemente fechamento ou revalidação suficiente para impedir novos envios e recebimentos indevidos. Não use apenas a reconexão do frontend como proteção. Se a operação exigir múltiplos processos ou eventos do worker, proponha camada compartilhada e obtenha aprovação de dependência/configuração. Avalie presença com expiração e atualização periódica, sem prometer precisão que o contador atual não oferece. Preserve reconexão sem misturar contas.

**Aceite:** revogação também bloqueia socket antigo; eventos só chegam aos destinatários atuais; teste entre processos é exigido antes de declarar suporte a múltiplas réplicas; falha de transporte não desfaz mensagem persistida.

## P-08 — Preferências e entrega de notificações

**Estado observado:** `push_enabled` é aplicado; outros campos são persistidos sem controlar todos os serviços. Não há agendamento de prazos/resumo semanal, registro durável de entrega ou retentativa FCM; envio pós-commit é síncrono.

**Prompt:**

> Mapeie cada preferência persistida para seu evento e canal de entrega. Aplique os campos pertinentes aos eventos já existentes; não suprima e-mails obrigatórios de segurança sem decisão explícita. Para novos lembretes, resumos ou avisos de status/pagamento, obtenha aprovação de escopo e defina destinatários, fuso, deduplicação e cancelamento. Proponha entrega assíncrona com retentativa e registro durável se necessário. Preserve caixa interna, evento em tempo real e preferência global push como comportamentos independentes. Mantenha teste push restrito ao próprio token e lotes do provedor.

**Aceite:** cada opção tem comportamento documentado e testado; rollback não entrega aviso; retentativa não duplica notificação; falha de FCM preserva caixa interna; aceite no provedor não é apresentado como confirmação de exibição.

## P-09 — Telas, indicadores e contratos de consulta

**Estado informado do frontend:** início, minhas tarefas, equipe, indicadores e relatórios usam demonstrações; Kanban, busca, arquivos e controles Drive/Calendar têm integração parcial. Existem serviços tipados, mas isso não prova persistência pelas telas.

**Prompt:**

> Escolha uma área por tarefa e conecte a tela à API real. Comece pela consulta e depois ações, com erros, carregamento, estado vazio, permissões e nova tentativa. Para “minhas tarefas”, filtros por autor, indicadores globais, atividade e relatórios CSV, confira o contrato atual antes de criar endpoint: nem todos esses agregados existem. Defina organização, período, fuso, paginação e autorização; não calcule totais a partir de uma página de resultados. Persistência deve sobreviver ao recarregamento e nunca usar dados simulados como confirmação de operação. Convites em lote podem coordenar chamadas individuais, preservando resultados por destinatário.

**Aceite:** dados pertencem à conta/organização selecionada; mudanças persistem; ações indisponíveis por papel são bloqueadas no servidor; números vêm de consultas autorizadas; falhas individuais podem ser retomadas sem duplicar sucessos.

## P-10 — Pagamentos e divergência de valor/cartão

**Estado observado:** servidor fixa R$ 9,99 e rejeita `amount`; cartão já tem API tokenizada. O levantamento do frontend ainda descreve valor editável e aprovação simulada de cartão. O webhook consulta o provedor, mas não verifica assinatura nem registra eventos em estrutura própria. CPF tem apenas validação de tamanho.

**Prompt:**

> Alinhe PIX e cartão com RN-69 a RN-71, sem transferir definição de preço para o navegador. Retire valor arbitrário do corpo e use preço autorizado pelo servidor. Substitua aprovação simulada por tokenização e chamada real em sandbox; nunca envie número completo do cartão ao backend. Defina validação de CPF e assinatura do webhook conforme documentação atual do provedor, com aprovação do contrato. Teste eventos repetidos, fora de ordem e falhas; confirme propriedade e referência do pagamento. Não implemente planos ou libere IA premium nesta correção. Antes de cobrar de verdade, exija validação explícita do responsável.

**Aceite:** interface não simula cobrança; valor não é manipulável pelo cliente; “Já paguei” só consulta; webhook inválido não muda pagamento; erros não retornam sucesso fictício; testes não cobram cartões reais.

## P-11 — Idempotência e concorrência

**Estado observado:** chave do middleware usa cookie ou IP, método e caminho; não inclui corpo ou usuário estável, retorna cache antes da autorização atual e não reproduz cookies/cabeçalhos. Há bloqueio de 60 segundos e cache de 24 horas. WIP e criação de evento Google não têm proteção geral contra chamadas simultâneas.

**Prompt:**

> Separe primeiro a correção de idempotência das mudanças de concorrência. Defina identidade estável, resumo do conteúdo e nova validação de autorização antes de reutilizar resposta. Mesma chave com corpo diferente deve ter resultado definido, não reutilização silenciosa. Exclua fluxos de autenticação e conteúdos sensíveis inadequados ao cache, ou preserve seus efeitos com contrato aprovado. Em tarefas separadas, proteja limite WIP e criação do evento externo usando estratégia compatível com PostgreSQL e falhas do provedor. Não mantenha transação longa aberta durante chamada externa sem justificativa. Não anuncie garantia absoluta de execução única.

**Aceite:** retentativa equivalente não repete efeito; chave de outro usuário não reutiliza resposta; sessão revogada não recebe resposta privada em cache; chamadas concorrentes não excedem limite acordado nem duplicam o vínculo externo; travamentos têm recuperação definida.

## P-12 — Segurança e integração de arquivos

**Estado observado:** upload genérico limita tamanho, sem política geral de conteúdo ou antimalware. Arquivos remotos podem ter URL pública. Logo PNG/SVG de 2 MB e foto recortada em 512 × 512 são regras informadas pelo frontend, não validações gerais do upload backend.

**Prompt:**

> Defina contratos por destino: logo, foto, anexo de chat, arquivo Drive e análise de IA. Valide no servidor tipo, tamanho e conteúdo quando o requisito exigir segurança, sem confiar em extensão ou MIME enviado pelo cliente. Trate SVG e URLs com política explícita. Decida quando arquivo deve ser privado e como autorizar download; não confunda acesso à API com privacidade de URL pública. Não reduza upload genérico por causa de regra de logo. Teste limpeza de arquivo órfão, falha parcial e salvamento confirmado do perfil. Não remova objeto remoto em uso por outra entidade.

**Aceite:** limites por destino são consistentes entre interface e API; cancelamento não salva foto indevidamente; falha de logo impede criação incompleta conforme contrato aprovado; arquivo privado não é exposto só por conhecer URL; análise mantém validação de conteúdo e não retorna PDF parcial.

## P-13 — Autorização e comportamento do frontend

**Estado informado:** proteção de rotas privadas não cobre todas as áreas; idioma/fuso não se aplicam globalmente; geolocalização não trata todos os erros. PWA e mapa dependem de suporte e configuração do navegador.

**Prompt:**

> No repositório frontend e em branch própria, revise rotas privadas, retorno ao destino interno, troca de conta, limpeza de stores e restauração de preferências. Preserve código/senha apenas em memória e use cliente HTTP compartilhado. Aplique formato de data/fuso sem mudar regras de prazo do servidor. Trate permissão negada, falta de suporte e falha em Google/Firebase/Mapbox/geolocalização. Não adicione persistência de coordenadas ao backend sem requisito específico. Informe claramente recursos de demonstração, verificação em duas etapas indisponível e ausência de operação offline da API. Verifique teclado, rótulos, contraste e tamanhos de tela.

**Aceite:** navegação não expõe área privada; backend continua sendo autoridade de acesso; destino externo de retorno é recusado; troca de conta não mistura estado; erro de localização não impede outras áreas; tema cancelado é revertido.

## P-14 — Código e documentação sem uso

**Estado observado:** documentos descrevem Checkout Pro, modelos ainda em preparação e planos futuros que não representam o código atual. Existem modelos P3 e aliases de compatibilidade; ausência de tela não prova código morto.

**Prompt:**

> Inventarie rotas, imports, referências em migrations, admin, testes, tarefas e consumidores frontend antes de classificar código como sem uso. Diferencie recurso legado, compatibilidade necessária, implementação sem tela e proposta não executada. Compare `PROJECT_GUIDE.md`, `PRODUCT_SCOPE.md`, `FEATURE_BRANCHES.md`, `RELATORIO-FINAL.md`, DBML, `SUBSCRIBER_AI_CHAT.md`, documentação de notificações e os requisitos novos. Apresente uma lista de manter, atualizar, arquivar e remover, com evidência de cada escolha. Só depois de aprovação execute limpeza, uma área por vez. Preserve migrations históricas e não apague tabelas ou dados apenas porque não há endpoints. Atualize referências e exemplos que usam papéis retirados, hierarquia de tarefas ou preço definido pelo cliente.

**Aceite:** documentação operacional coincide com o contrato; propostas estão identificadas; remoções têm referências verificadas e aprovação; imports, migrations e testes continuam válidos; nenhuma API consumida desaparece sem transição.

## P-15 — Operação e garantias de serviço

**Estado observado:** não há metas formalizadas de disponibilidade/carga, retenção e backup/restauração. Atualização de produção documentada acompanha `dev`; migrations não têm reversão automática.

**Prompt:**

> Defina com o responsável metas mensuráveis de operação, retenção, backup e recuperação. Documente segredos necessários sem seus valores, execução de migrations, verificações de saúde, falha de implantação e restauração de banco. Revise a promoção automática de `dev` e proteções de CI sem mudar implantação sem aprovação. Teste restauração em ambiente isolado; preserve volumes e não faça exclusão ampla para liberar espaço. Configure métricas e logs sem dados privados. Não trate verificações históricas ou teste simulado como validação atual de produção.

**Aceite:** procedimento reproduzível de backup e restauração; critérios de promoção e falha definidos; carga medida antes de prometer capacidade; problemas de build/migration não apagam dados ou segredos.

## P-16 — Decisões fora do escopo atual

**Estado observado:** planos, assinaturas, cotas pagas, IA privada com histórico/execução de tarefas, campos personalizados, visões salvas, dependências e tradução de documentos não têm contrato operacional completo. A tela de assistente descreve sugestões simuladas; isso não autoriza executar tarefas automaticamente.

**Prompt de decisão, não de implementação:**

> Confirme quais recursos permanecem fora do produto e quais terão Issue própria. Para assinatura, defina titular, período, preço, cancelamento/reembolso, limite e fontes de confirmação antes de codificar. Para sugestões de IA, defina revisão explícita, formato validado e execução pelo service de tarefas com permissões atuais. Nenhum pagamento deve conceder acesso organizacional nem liberar recurso não contratado. Recursos P3 sem uso podem continuar históricos ou ser retirados após inventário e plano aprovado; não implemente APIs apenas para justificar modelos existentes.

**Saída esperada:** decisões registradas por recurso, contratos mínimos e escopo aprovado, sem exclusão de dados ou implementação não solicitada.
