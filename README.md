# Requisitos do backend e integração com o frontend

## 1. Requisitos funcionais

### Conta, perfil e preferências

| ID    | Requisito                                                                                                                                                                             |
| ----- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| RF-01 | Autenticar com credenciais locais ou Google. (Dado: e-mail; senha; credencial Google.)                                                                                                |
| RF-02 | Concluir cadastro com confirmação de e-mail. (Dado: nome; e-mail; senha; código de confirmação.)                                                                                      |
| RF-03 | Recuperar o acesso por redefinição de senha. (Dado: e-mail; código; nova senha.)                                                                                                      |
| RF-04 | Confirmar a troca do próprio e-mail. (Dado: novo e-mail; código; senha atual.)                                                                                                        |
| RF-05 | Renovar a sessão autenticada. (Dado: cookies de acesso e atualização.)                                                                                                                |
| RF-06 | Gerenciar (CRUD) as sessões próprias. (Dado: dispositivo; IP; localização registrada; datas; indicação de sessão atual.)                                                              |
| RF-07 | Encerrar a sessão atual ou revogar todas as sessões próprias. (Dado: sessão autenticada.)                                                                                             |
| RF-08 | Gerenciar (CRUD) o próprio perfil. (Dado: nome; biografia; foto de perfil.)                                                                                                           |
| RF-09 | Desativar a própria conta. (Dado: identificação da conta autenticada.)                                                                                                                |
| RF-10 | Buscar e ordenar usuários autorizados. (Dado: identificação; nome; e-mail; estado de ativação permitido.)                                                                             |
| RF-11 | Gerenciar (CRUD) preferências de apresentação e regionalização. (Dado: tema; idioma; fuso; formato de data; início da semana; densidade dos cartões; exibição de tarefas concluídas.) |
| RF-12 | Gerenciar (CRUD) preferências de notificação. (Dado: push; categorias no aplicativo; categorias de e-mail.)                                                                           |

### Organizações e participação

| ID    | Requisito                                                                                                                 |
| ----- | ------------------------------------------------------------------------------------------------------------------------- |
| RF-13 | Gerenciar (CRUD) organizações. (Dado: nome; endereço do espaço de trabalho; descrição; logo; faixa de tamanho da equipe.) |
| RF-14 | Resolver o endereço de uma organização acessível. (Dado: `slug`; ID; papel da pessoa na organização.)                     |
| RF-15 | Gerenciar (CRUD) vínculos de membros da organização. (Dado: usuário; organização; papel.)                                 |
| RF-16 | Permitir saída voluntária da organização. (Dado: vínculo da pessoa autenticada.)                                          |
| RF-17 | Gerenciar (CRUD) convites da organização. (Dado: e-mail; papel; setores; estado; expiração.)                              |
| RF-18 | Consultar os próprios convites válidos. (Dado: e-mail da conta; organização; setores; estado do convite.)                 |
| RF-19 | Aceitar explicitamente um convite. (Dado: token; conta autenticada correspondente ao destinatário.)                       |
| RF-20 | Gerenciar (CRUD) times. (Dado: organização; nome; descrição.)                                                             |
| RF-21 | Gerenciar (CRUD) vínculos de integrantes de times. (Dado: time; usuário; papel.)                                          |
| RF-22 | Permitir saída voluntária de um time. (Dado: vínculo da pessoa autenticada.)                                              |
| RF-23 | Gerenciar (CRUD) vínculos entre times e setores. (Dado: time; setor; papel de acesso.)                                    |

### Setores e tarefas

| ID    | Requisito                                                                                                                                                   |
| ----- | ----------------------------------------------------------------------------------------------------------------------------------------------------------- |
| RF-24 | Gerenciar (CRUD) setores. (Dado: organização; nome; descrição; ícone; cor; visibilidade.)                                                                   |
| RF-25 | Gerenciar (CRUD) participantes de setores. (Dado: setor; usuário; papel.)                                                                                   |
| RF-26 | Permitir saída voluntária de um setor. (Dado: vínculo direto da pessoa autenticada.)                                                                        |
| RF-27 | Gerenciar (CRUD) colunas do quadro. (Dado: setor; nome; categoria; cor; posição; limite de tarefas.)                                                        |
| RF-28 | Gerenciar (CRUD) tarefas. (Dado: setor; título; descrição; coluna; prioridade; prazo; responsável.)                                                         |
| RF-29 | Mover e ordenar tarefas no quadro. (Dado: tarefa; coluna de destino; posição.)                                                                              |
| RF-30 | Buscar, filtrar e ordenar tarefas do setor. (Dado: texto; responsável; coluna; prioridade; prazo; conclusão; atraso; posição; datas.)                       |
| RF-31 | Consultar tarefas atribuídas à pessoa ou registradas por ela no contexto autorizado. (Dado: usuário; organização; tarefas abertas, atrasadas e concluídas.) |
| RF-32 | Informar o estado de cada tarefa. (Dado: prazo; atraso; data de conclusão.)                                                                                 |
| RF-33 | Informar o progresso do setor. (Dado: total de tarefas; concluídas; atrasadas; percentual.)                                                                 |
| RF-34 | Gerenciar (CRUD) itens de checklist. (Dado: tarefa; conteúdo; posição; marcação de conclusão.)                                                              |
| RF-35 | Gerenciar (CRUD) comentários. (Dado: tarefa; conteúdo; autor; data.)                                                                                        |
| RF-36 | Gerenciar (CRUD) links de apoio. (Dado: destino; título; URL; autor; data.)                                                                                 |

### Chat e notificações

| ID    | Requisito                                                                                                                      |
| ----- | ------------------------------------------------------------------------------------------------------------------------------ |
| RF-37 | Consultar canais acessíveis. (Dado: organização; tipo de canal; participantes; última mensagem; quantidade de não lidas.)      |
| RF-38 | Abrir conversa direta com outra pessoa da organização. (Dado: organização; destinatário.)                                      |
| RF-39 | Abrir conversa compartilhada por times. (Dado: organização; times; nome do canal.)                                             |
| RF-40 | Gerenciar (CRUD) mensagens conforme autoria e moderação. (Dado: canal; texto; resposta; menções; referência a tarefa; anexos.) |
| RF-41 | Marcar canal como lido. (Dado: canal; data da última leitura.)                                                                 |
| RF-42 | Informar presença e digitação em tempo real. (Dado: usuário; canal; estado online; evento de digitação.)                       |
| RF-43 | Iniciar reunião Google Meet pelo chat. (Dado: canal; integração Google; link da reunião.)                                      |
| RF-44 | Notificar ações relevantes para seus destinatários. (Dado: atribuição de tarefa; comentário ao responsável; menção no chat.)   |
| RF-45 | Consultar notificações próprias com filtro e paginação. (Dado: título; mensagem; referência; data; estado de leitura.)         |
| RF-46 | Marcar uma ou todas as notificações próprias como lidas. (Dado: notificação; conta autenticada.)                               |
| RF-47 | Gerenciar (CRUD) registros push do próprio dispositivo. (Dado: conta; token FCM.)                                              |
| RF-48 | Enviar push de teste ao próprio dispositivo. (Dado: token registrado; título; mensagem.)                                       |

### Arquivos e integrações Google

| ID    | Requisito                                                                                                                                |
| ----- | ---------------------------------------------------------------------------------------------------------------------------------------- |
| RF-49 | Enviar arquivo ao serviço de armazenamento. (Dado: arquivo; URL; identificador remoto.)                                                  |
| RF-50 | Consultar arquivos acessíveis por origem. (Dado: organização; tarefa ou mensagem vinculada; nome; tipo; tamanho; autor; data; URL.)      |
| RF-51 | Buscar informações da organização. (Dado: texto; tarefas; pessoas; arquivos autorizados.)                                                |
| RF-52 | Conectar e desconectar a própria conta Google. (Dado: consentimento OAuth; código; `state`; permissões concedidas.)                      |
| RF-53 | Consultar e enviar arquivos do aplicativo no próprio Drive. (Dado: nome; arquivo; identificador remoto; URL; compartilhamento por link.) |
| RF-54 | Anexar arquivo Drive a uma tarefa como link. (Dado: tarefa; arquivo Drive; título; URL.)                                                 |
| RF-55 | Sincronizar manualmente o prazo de uma tarefa com a própria agenda. (Dado: tarefa; prazo; título; descrição; evento Google.)             |
| RF-56 | Consultar ou remover a sincronização com a agenda. (Dado: tarefa; usuário; evento; data da última sincronização.)                        |

### Pagamentos, IA e suporte técnico

| ID    | Requisito                                                                                                                          |
| ----- | ---------------------------------------------------------------------------------------------------------------------------------- |
| RF-57 | Solicitar pagamento PIX. (Dado: descrição; nome do pagador; CPF; código PIX; QR Code; estado.)                                     |
| RF-58 | Solicitar pagamento com cartão tokenizado. (Dado: descrição; CPF; e-mail; token; parcelas; método; emissor quando aplicável.)      |
| RF-59 | Acompanhar pagamentos próprios. (Dado: identificação; valor definido pelo servidor; estado confirmado pelo provedor.)              |
| RF-60 | Responder perguntas pelo assistente de IA. (Dado: solicitação; resposta.)                                                          |
| RF-61 | Analisar documentos com IA e fornecer relatório PDF. (Dado: imagens ou PDFs; resumo; observações; inferências; limitações.)        |
| RF-62 | Gerar QR Code PNG. (Dado: texto de origem.)                                                                                        |
| RF-63 | Disponibilizar administração e documentação da API. (Dado: recursos administrativos; parâmetros; corpos; respostas; autenticação.) |

### Comportamentos do frontend relacionados

Estes itens descrevem a interface. Não exigem, por si, novos endpoints no backend.

| ID    | Requisito                                                                                                                                         |
| ----- | ------------------------------------------------------------------------------------------------------------------------------------------------- |
| RF-64 | Oferecer navegação e tratamento de página inexistente. (Dado: rota; sessão; página solicitada.)                                                   |
| RF-65 | Retomar somente destinos internos válidos após autenticação. (Dado: endereço de retorno.)                                                         |
| RF-66 | Conduzir a configuração inicial da organização. (Dado: setores; e-mails convidados; papéis; resultado individual de cada convite.)                |
| RF-67 | Permitir captura, seleção e recorte de foto antes de salvar. (Dado: imagem; prévia; confirmação.)                                                 |
| RF-68 | Apresentar indicadores de trabalho e relatórios por período. (Dado: organização; setor; período; tarefas; atividade; desempenho; exportação CSV.) |
| RF-69 | Exibir localização obtida com consentimento. (Dado: latitude; longitude; permissão; estado de acompanhamento.)                                    |
| RF-70 | Apresentar mapa e busca de locais. (Dado: posição disponível ou inicial; local pesquisado; relevo; construções.)                                  |

## 2. Requisitos não funcionais

| ID     | Requisito                                                                                                                                           |
| ------ | --------------------------------------------------------------------------------------------------------------------------------------------------- |
| RNF-01 | Autenticar o navegador por cookies HttpOnly, sem tokens de acesso ou atualização no JSON, no armazenamento do navegador ou no estado JavaScript.    |
| RNF-02 | Enviar credenciais pelo cliente HTTP compartilhado e CSRF nas escritas autenticadas por cookie.                                                     |
| RNF-03 | Validar sessão e autorização no backend; proteger rotas privadas e refletir permissões nas ações da interface.                                      |
| RNF-04 | Impedir consulta e entrega HTTP, push ou WebSocket de conteúdo não autorizado entre contas, organizações, setores e canais.                         |
| RNF-05 | Usar HTTPS, cookies seguros, CORS restrito, validação de origem WebSocket e proteções de produção do Django.                                        |
| RNF-06 | Guardar senhas e códigos como hash; criptografar credenciais OAuth; manter segredos fora do Git, dos logs e do frontend.                            |
| RNF-07 | Manter segredos de serviços no servidor. Variáveis `VITE_*` são públicas e só recebem configurações próprias do navegador.                          |
| RNF-08 | Expor contratos OpenAPI versionados em `/api/v1/`, com JSON em `snake_case`, validação de entrada e documentação de permissões.                     |
| RNF-09 | Paginar consultas genéricas em 20 registros e documentar limites específicos das outras consultas.                                                  |
| RNF-10 | Aplicar limites de requisição por usuário, anônimo e operações sensíveis; comunicar excesso com resposta apropriada.                                |
| RNF-11 | Proteger operações compostas com transações e evitar entrega de eventos ou e-mails antes da confirmação da gravação.                                |
| RNF-12 | Evitar repetição de ações em tentativas de recuperação; idempotência precisa respeitar usuário, conteúdo da requisição e acesso atual.              |
| RNF-13 | Validar tamanho e conteúdo permitido de arquivos antes de processamento externo; tratar documentos e saída da IA como dados não confiáveis.         |
| RNF-14 | Manter a operação principal persistida quando a entrega posterior de push ou WebSocket falhar.                                                      |
| RNF-15 | Reconectar chat e notificações sem misturar estados entre contas ou organizações.                                                                   |
| RNF-16 | Informar carregamento, sucesso, erro e ausência de dados; permitir nova tentativa sem repetir etapas concluídas.                                    |
| RNF-17 | Tratar indisponibilidade de serviços externos sem impedir áreas independentes deles.                                                                |
| RNF-18 | Manter interface responsiva, navegação por teclado, rótulos de campos, contraste e indicação acessível dos estados.                                 |
| RNF-19 | Oferecer tema claro, escuro e do sistema; aplicar preferências e cancelar prévias não confirmadas.                                                  |
| RNF-20 | Apresentar a interface em português do Brasil e aplicar preferências de fuso e formato sem alterar o significado das datas no backend.              |
| RNF-21 | Oferecer instalação e atualização PWA; cache de recursos estáticos não implica operações de API disponíveis offline.                                |
| RNF-22 | Usar Python 3.14, Django, DRF e PDM, com dependências resolvidas no arquivo de lock e módulos separados por responsabilidade dentro de `core`.      |
| RNF-23 | Usar migrations e persistência relacional; PostgreSQL em produção, SQLite ou PostgreSQL no desenvolvimento configurado.                             |
| RNF-24 | Usar Redis/Celery para os fluxos configurados e ASGI para WebSocket; múltiplos processos exigem camada de canais compartilhada.                     |
| RNF-25 | Registrar erros e auditoria sem expor credenciais; validar integração com testes de isolamento, permissões e falhas de provedores simuladas.        |
| RNF-26 | Implantar com verificações de saúde e preservação de volumes; definir backup, restauração, retenção e metas de operação antes de assumir garantias. |

## 3. Regras de negócio

### Conta e preferências

| ID    | Regra                                                                                                                                                                                                    |
| ----- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| RN-01 | O login local usa e-mail único e conta ativa. (Dado: e-mail; senha; ativação.)                                                                                                                           |
| RN-02 | O cadastro por e-mail só ativa a conta após confirmação do código. O aceite de termos pertence à interface; persistir sua versão exige contrato específico. (Dado: nome; e-mail; senha; código; aceite.) |
| RN-03 | A conta deve receber uma organização pessoal automaticamente ao concluir cadastro local ou primeiro acesso Google, sem duplicação. (Dado: usuário; organização pessoal; vínculo proprietário.)           |
| RN-04 | Uma organização de equipe é uma escolha explícita; o cadastro não deve exigir redirecionamento para sua configuração. (Dado: tipo de organização; destino após cadastro.)                                |
| RN-05 | Código tem seis dígitos, vale por 15 minutos, é de uso único e bloqueia após cinco tentativas incorretas. Novo envio substitui e reinicia sua validade. (Dado: código; expiração; tentativas; consumo.)  |
| RN-06 | Verificar código não o consome; confirmar a operação o consome. A interface aguarda 45 segundos para oferecer reenvio, sem substituir os limites do servidor. (Dado: código; etapa; horário.)            |
| RN-07 | A recuperação não revela se o e-mail existe. Senhas respeitam mínimo de nove caracteres e os demais validadores do Django. (Dado: e-mail; nova senha.)                                                   |
| RN-08 | Troca de e-mail exige código no novo endereço e senha atual; após sucesso, revoga todas as sessões e exige novo login. (Dado: novo e-mail; código; senha; sessões.)                                      |
| RN-09 | Redefinir senha ou desativar conta também deve encerrar sessões. A desativação preserva o registro da pessoa. (Dado: ativação; sessões; cookies.)                                                        |
| RN-10 | Perfil permite edição de nome, biografia e foto, sem alteração direta de e-mail, senha ou privilégios. (Dado: `name`; `bio`; `avatar_url`.)                                                              |
| RN-11 | Só a própria conta acessa suas preferências e sessões. Sessão atual termina pelo logout; outra sessão termina pela revogação correspondente. (Dado: usuário; sessão; preferências.)                      |
| RN-12 | Login Google exige credencial válida e e-mail verificado; conta inativa com acesso anterior não pode ser reativada por esse login. (Dado: credencial; e-mail; histórico de acesso.)                      |
| RN-13 | Acesso dura 30 minutos e atualização dura um dia; renovar troca os tokens e bloqueia a atualização anterior. (Dado: cookies; expiração; sessão ativa.)                                                   |
| RN-14 | Preferências são individuais e alterações parciais preservam campos não enviados. (Dado: tema; idioma; fuso; notificações; exibição.)                                                                    |
| RN-15 | Os padrões são tema do sistema, idioma `pt-BR`, fuso `America/Sao_Paulo`, data `dd/mm/yyyy` e início da semana no domingo. (Dado: preferências padrão.)                                                  |

### Organizações, convites e times

| ID    | Regra                                                                                                                                                                                                     |
| ----- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| RN-16 | Quem registra uma organização recebe o papel proprietário. Organização pessoal não pode ser removida pelo fluxo comum. (Dado: organização; usuário; papel; tipo.)                                         |
| RN-17 | Endereço do espaço de trabalho é único, com até 63 caracteres, letras minúsculas, números e hífens simples entre partes. (Dado: `slug`.)                                                                  |
| RN-18 | Tamanho declarado é informativo, sem limitar assentos ou cobrança. (Dado: `1`; `2-10`; `11-50`; `51-200`; `201+`.)                                                                                        |
| RN-19 | Papéis organizacionais do escopo são membro, administrador e proprietário, nessa ordem. Papel global de staff não substitui participação organizacional. (Dado: `member`; `admin`; `owner`.)              |
| RN-20 | Membros consultam a organização; gestão organizacional e de membros exige administrador ou proprietário. Remoção da organização exige proprietário. (Dado: ação; organização; papel.)                     |
| RN-21 | Ninguém concede papel igual ou superior ao próprio, altera outro com papel igual ou superior, nem remove o proprietário. Proprietário não sai pelo fluxo de membros. (Dado: autor; destinatário; papéis.) |
| RN-22 | Um usuário tem um vínculo por organização, setor ou time; um time tem um vínculo por setor. Times não contêm outros times. (Dado: vínculos de participação.)                                              |
| RN-23 | Gestão de times exige administrador/proprietário; gestão interna também pode ser feita pelo líder. Só entram usuários da mesma organização. (Dado: time; integrante; `member` ou `lead`.)                 |
| RN-24 | Convite só é enviado por administrador/proprietário, para papel inferior ao próprio e sem conceder propriedade. (Dado: e-mail; papel; organização.)                                                       |
| RN-25 | Convite dura sete dias e admite um pendente por e-mail e organização, sem diferenciar maiúsculas. Destinatário já membro não recebe novo convite. (Dado: destinatário; estado; validade.)                 |
| RN-26 | Autenticar ou cadastrar não aceita convite automaticamente; a pessoa confirma o aceite com a conta do e-mail convidado. (Dado: token; e-mail da conta; confirmação.)                                      |
| RN-27 | Setores convidados pertencem à mesma organização; aceite concede participação de edição nos setores ativos, preservando papel já existente. (Dado: organização; setores; vínculos.)                       |
| RN-28 | Sair do time ou setor remove só o vínculo correspondente. O acesso concedido por outro vínculo válido deve ser recalculado. (Dado: participação direta; time; papel organizacional.)                      |

### Setores e tarefas

| ID    | Regra                                                                                                                                                                                                 |
| ----- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| RN-29 | Setores são privados por padrão. (Dado: `is_private=true`.)                                                                                                                                           |
| RN-30 | Acesso exige organização ativa e participação atual. Setor privado exige vínculo direto, time vinculado ou gestão organizacional. (Dado: organização; setor; vínculos.)                               |
| RN-31 | Os papéis atribuíveis de setor são editor e gestor. O maior papel recebido por vínculo direto, time ou gestão organizacional prevalece. (Dado: `editor`; `manager`.)                                  |
| RN-32 | Consulta em setor não privado pode ser concedida aos membros da organização sem atribuir papel adicional; esse acesso não autoriza escrita. (Dado: visibilidade; participação; ação.)                 |
| RN-33 | Editor gerencia tarefas e seus itens; gestor também gerencia setor, participantes e colunas. (Dado: ação; papel efetivo.)                                                                             |
| RN-34 | Novo setor exige membro ou superior na organização e recebe o autor como gestor e três colunas iniciais. (Dado: `To do`; `In progress`; `Done`.)                                                      |
| RN-35 | Colunas têm nome único no setor; a categoria define o significado, independentemente do nome exibido. (Dado: `todo`; `in_progress`; `done`.)                                                          |
| RN-36 | Coluna com tarefa ativa ou última categoria concluída não pode ser removida. (Dado: tarefas ativas; categoria `done`.)                                                                                |
| RN-37 | Coluna e responsável precisam pertencer ao contexto autorizado da tarefa. Responsável é opcional, ativo e participante efetivo. (Dado: setor; coluna; usuário.)                                       |
| RN-38 | Prioridade padrão é média. (Dado: `low`; `medium`; `high`; `urgent`.)                                                                                                                                 |
| RN-39 | Sem coluna informada, tarefa usa a primeira pendente por posição ou a primeira disponível. Limite positivo da coluna restringe inclusão e mudança de coluna. (Dado: posição; categoria; `wip_limit`.) |
| RN-40 | Movimento usa ação própria, sem trocar o setor; a coluna de destino recebe posições sequenciais. (Dado: tarefa; coluna; posição.)                                                                     |
| RN-41 | Entrada em categoria concluída preenche conclusão; saída limpa a conclusão. Alterar categoria da coluna também ajusta suas tarefas. (Dado: categoria; `completed_at`.)                                |
| RN-42 | Tarefa está atrasada quando prazo é anterior à data local e não há conclusão; vencer hoje não caracteriza atraso. (Dado: `due_date`; data local; `completed_at`.)                                     |
| RN-43 | Progresso usa tarefas ativas: concluídas ÷ total × 100, com uma casa decimal; total zero resulta em zero. (Dado: total; concluídas; percentual.)                                                      |
| RN-44 | Checklist organiza itens da tarefa; marcar todos não conclui a tarefa automaticamente. (Dado: itens; marcação; estado da tarefa.)                                                                     |
| RN-45 | Autor com acesso edita comentário; autor ou gestor com acesso o remove. (Dado: comentário; autor; setor.)                                                                                             |
| RN-46 | Cada link tem um único destino. Remoção exige autor com acesso ou gestão do destino. (Dado: organização, setor ou tarefa; URL; autor.)                                                                |

### Chat e notificações

| ID    | Regra                                                                                                                                                                                        |
| ----- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| RN-47 | Canal geral é acessível aos membros atuais da organização. (Dado: organização; participantes.)                                                                                               |
| RN-48 | Canal de setor exige acesso atual ao setor. (Dado: setor; usuário.)                                                                                                                          |
| RN-49 | Canal de time exige participação atual no time. (Dado: time; usuário.)                                                                                                                       |
| RN-50 | Canal entre times admite integrantes atuais dos times associados. (Dado: times; participantes.)                                                                                              |
| RN-51 | Conversa direta é restrita a duas pessoas da mesma organização e não pode ser aberta consigo mesmo. (Dado: organização; par de usuários.)                                                    |
| RN-52 | Existe uma conversa direta por par e organização. (Dado: par de usuários; organização; canal.)                                                                                               |
| RN-53 | Canais geral, de setor e de time são provisionados sem duplicação quando necessários. (Dado: organização; setor; time; canal.)                                                               |
| RN-54 | Canal entre times exige ao menos dois times ativos da organização e autor administrador/proprietário ou integrante de algum deles. (Dado: autor; times; organização.)                        |
| RN-55 | Mensagem exige texto ou anexo; resposta referencia o mesmo canal. (Dado: até 4.000 caracteres; cinco arquivos de até 10 MiB; 20 menções.)                                                    |
| RN-56 | Menções notificam contas ativas com acesso ao canal, exceto o próprio autor. Referências a tarefas não podem revelar dados a destinatários sem acesso. (Dado: destinatários; canal; tarefa.) |
| RN-57 | Só autor edita mensagem; autor ou gestão organizacional com acesso pode remover, sem moderação de conversa direta por terceiros. (Dado: autoria; canal; papel.)                              |
| RN-58 | Não lidas excluem mensagens próprias e removidas. (Dado: data da última leitura; mensagens posteriores.)                                                                                     |
| RN-59 | Notificações pertencem ao destinatário. Ações próprias não geram aviso de atribuição/comentário para o próprio autor. (Dado: autor; responsável; menções.)                                   |
| RN-60 | Push global, permissão do navegador e registro do dispositivo são independentes. Desativar push não remove a notificação interna. (Dado: preferência; permissão; token.)                     |
| RN-61 | Token FCM é único e associado à conta atual; só ela remove o registro ou envia teste para ele. Troca de conta limpa o estado local. (Dado: token; conta; notificações locais.)               |

### Arquivos, integrações, pagamentos e IA

| ID    | Regra                                                                                                                                                                                                                |
| ----- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| RN-62 | Logo aceita PNG/SVG de até 2 MB; foto aceita JPG/PNG/WebP de até 10 MB e é preparada em 512 × 512 na interface, antes do salvamento confirmado. (Dado: imagem; tipo; tamanho; recorte.)                              |
| RN-63 | Upload genérico admite até 10 MiB; regras do destino podem ser mais restritivas. (Dado: arquivo; destino; tamanho.)                                                                                                  |
| RN-64 | Busca exige ao menos dois caracteres e só retorna recursos autorizados, até dez por categoria no contrato atual. (Dado: texto; tarefas; pessoas; arquivos.)                                                          |
| RN-65 | Login Google não substitui consentimento para Calendar/Drive. OAuth exige estado assinado da conta, válido por dez minutos. (Dado: credencial de login; código OAuth; `state`; escopos.)                             |
| RN-66 | Calendar representa o prazo como evento de dia inteiro na agenda do próprio usuário; sincronização é manual e só ocorre do sistema para o Google. (Dado: tarefa; prazo; evento; usuário.)                            |
| RN-67 | Drive usa acesso limitado ao aplicativo; anexar arquivo exige autorização sobre arquivo e tarefa. Compartilhamento público por link depende de opção explícita. (Dado: `drive.file`; arquivo; tarefa; `share_link`.) |
| RN-68 | Meet exige integração autorizada e acesso ao canal; o link corresponde a evento de uma hora na agenda do autor. (Dado: autor; canal; reunião.)                                                                       |
| RN-69 | PIX e cartão usam R$ 9,99 definidos pelo servidor; a interface não envia valor escolhido pelo usuário. (Dado: descrição; valor de cobrança.)                                                                         |
| RN-70 | Pagamento usa CPF de 11 dígitos; cartão depende de token do provedor, nunca de armazenamento do número completo ou código de segurança. (Dado: CPF; token; parcelas.)                                                |
| RN-71 | Só estado obtido no provedor confirma pagamento; ação “Já paguei” não aprova cobrança. Pagamento não ativa assinatura automaticamente. (Dado: pagamento; estado; referência externa.)                                |
| RN-72 | Pergunta ao assistente é pontual e exige autenticação; sugestões não executam ações no Kanban sem validação e confirmação. (Dado: solicitação de até 10.000 caracteres; resposta.)                                   |
| RN-73 | Análise de documentos usa limites configuráveis, inicialmente cinco arquivos de até 5 MiB e 15 MiB no total. (Dado: JPEG; PNG; WebP; PDF.)                                                                           |
| RN-74 | Análise rejeita PDF criptografado, acima de 30 páginas, imagem animada ou acima de 20 milhões de pixels. (Dado: conteúdo; páginas; dimensões.)                                                                       |
| RN-75 | Falha em algum arquivo impede relatório parcial. Saída distingue observação, inferência e limitação e exige revisão humana. (Dado: análises; relatório PDF.)                                                         |
| RN-76 | QR Code usa texto não vazio de até 1.000 caracteres. (Dado: conteúdo textual.)                                                                                                                                       |
| RN-77 | Demonstrações não representam dados persistidos nem podem confirmar pagamento, assinatura ou ação de IA executada. (Dado: origem real ou simulada da informação.)                                                    |
| RN-78 | Localização depende de consentimento do navegador; sem posição, o mapa usa a coordenada inicial configurada. (Dado: latitude; longitude; permissão; posição inicial.)                                                |

