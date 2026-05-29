# RAG de Suporte — Colabora PY

Base de conhecimento da IA de suporte ao usuário. Documento voltado para clientes finais (cidadãos, equipes de órgão público e equipes Colabora).

Idioma do produto: español paraguayo (es-PY). Quando uma mensagem do sistema é citada entre aspas, ela aparece literalmente assim na tela.

Como esta IA deve responder:
- Em pt-BR ou es-PY conforme o usuário escrever.
- Sem mencionar arquivos de código, endpoints, status HTTP, nomes de tabelas, variáveis de ambiente, infraestrutura ou qualquer detalhe técnico do produto.
- Quando o assunto sair deste material, encaminhar a um humano do Equipo Colabora.

---

## SEÇÃO 1 — VISÃO GERAL DO SISTEMA

### O que é

Colabora PY é uma plataforma de gestão de gabinetes públicos no Paraguai. O sistema tem três faces:

- App móvel "Colabora Ciudadano" — onde o cidadão cria solicitudes, conversa com o órgão, vê notícias, enquetes, avisos, eventos e vagas, e abre tickets de suporte.
- Site web do cidadão — onde o cidadão acompanha seus protocolos pelo navegador.
- Painel "Gerencia Mi Gabinete" — onde a equipe do órgão público (Master, Cidade, Secretaría Ministerial, Secretaría Municipal, Escritório) trabalha as solicitudes, publica conteúdo e administra o organograma.

O suporte aos órgãos é atendido pelo Equipo Colabora, uma equipe interna que opera uma bandeja própria de tickets.

### Público-alvo

- Cidadão paraguaio.
- Equipe do órgão público (Master e níveis hierárquicos abaixo).
- Agentes do Equipo Colabora.

### Funcionalidades principais

- Cadastro e autenticação de cidadãos (app e site).
- Abertura, triagem, encaminhamento e resposta de solicitudes.
- Geração de ofício em PDF e envio por e-mail a responsáveis externos.
- Comunicação ao público: avisos, notícias e enquetes.
- Mi Ciudad: guia de lugares úteis e agenda de eventos locais.
- Empleo Público: publicação de vagas e gestão de candidaturas.
- Productividad: agenda de compromissos e kanban de tarefas.
- Informes: análise de solicitudes com filtros, gráficos e exportação em CSV.
- Estructura: organograma de cidades, secretarías e suas unidades + convite de equipe.
- Contratos: gestão dos contratos comerciais com cada órgão (apenas Master).
- Suporte: tickets entre órgão e Equipo Colabora, com macros, anexos e pesquisa de satisfação.
- Notificações push (no celular) e na inbox do app.

### Idioma e fuso

- Idioma do produto: español paraguayo. Algumas telas têm partes em português; isso é normal.
- Fuso horário: America/Asuncion.

---

## SEÇÃO 2 — CRIAÇÃO DE CONTA

O sistema tem fluxos distintos para cada tipo de usuário.

## 2.1 Cadastro de cidadão pelo site web

#### Fluxo passo a passo

1. Cidadão abre a tela "Área do Cidadão" e clica em "Criar conta".
2. Preenche o passo "Sua conta": e-mail, senha, confirmação de senha, departamento e ciudad.
3. Preenche o passo "Dados pessoais": nome completo, CI, data de nascimento, sexo e celular.
4. Confirma. O sistema cria a conta como "não verificada" e envia um e-mail de verificação.
5. Aparece a tela "Cadastro realizado!" com a frase "Enviamos um link de verificação para [seu e-mail]. Verifique sua caixa de entrada e clique no link para ativar sua conta."
6. Cidadão abre o e-mail, clica no link, a conta é ativada e ele pode fazer login.

#### Campos obrigatórios

| Campo | Regra | Exemplo |
|---|---|---|
| E-mail | formato válido, único | juan@correo.py |
| Senha | mínimo 8 caracteres, com pelo menos uma letra e um número | exemplo: `MiClave2026` |
| Confirmar senha | igual à senha | — |
| Departamento | obrigatório | — |
| Ciudad | obrigatório, conforme departamento | — |
| Nome completo | mínimo 3 caracteres | María González |
| CI | 5 a 8 dígitos | 1234567 |
| Data de nascimento | data válida | — |
| Sexo | masculino ou feminino | — |
| Celular | exatamente 10 dígitos começando com 0 | 0987654321 |

#### Regras de negócio

- E-mail deve ser único no sistema.
- Se o e-mail já existe e ainda não foi verificado, o sistema reenvia o link automaticamente e responde "Correo de verificación reenviado.".
- Se o e-mail já existe e está verificado, responde "E-mail ya registrado".
- O distrito (ciudad) precisa ter atendimento público habilitado. Se não tiver, aparece "No hay servicio público activo para tu distrito todavía."
- A conta só fica ativa depois que o cidadão clica no link do e-mail de verificação. Antes disso, o login fica bloqueado.

#### Erros comuns

| Situação | Mensagem | Solução |
|---|---|---|
| E-mail já cadastrado e verificado | "E-mail ya registrado" | Recuperar senha ou usar outro e-mail |
| E-mail cadastrado mas não verificado | "Correo de verificación reenviado." | Conferir caixa de entrada e spam |
| Distrito sem atendimento habilitado | "No hay servicio público activo para tu distrito todavía." | Aguardar habilitação |
| Senhas diferentes | "As senhas não coincidem" | Repetir a senha |
| Senha curta | "La contraseña debe tener al menos 8 caracteres" | Aumentar |
| Senha sem letra | "La contraseña debe tener al menos una letra" | Incluir letra |
| Senha sem número | "La contraseña debe tener al menos un número" | Incluir número |
| Nome curto | "Nombre completo debe tener al menos 3 caracteres" | Preencher nome completo |
| Telefone com tamanho errado | "Teléfono debe tener 10 dígitos" | Usar exatamente 10 dígitos |
| Telefone sem 0 inicial | "Teléfono debe comenzar con 0" | Adicionar 0 no início |
| CI curta | "CI debe tener al menos 5 dígitos" | Conferir |
| CI longa | "CI debe tener hasta 8 dígitos" | Conferir |
| CI duplicada | "CI ya registrada" | Verificar se já tem conta |

## 2.2 Cadastro de admin ou membro do painel (por convite)

Esse fluxo só acontece quando alguém com permissão (Master, admin de Cidade ou admin de Secretaría Ministerial) envia um convite por e-mail.

#### Fluxo

1. O convidado recebe um e-mail com link de ativação.
2. Ao clicar no link, abre a tela "Criar sua conta — Complete seu cadastro para acessar o sistema".
3. Aparece o bloco "Dados do convite" mostrando o e-mail e o perfil.
4. Convidado preenche: Nome completo, CI (opcional), Telefone (opcional), Senha, Confirmar senha.
5. Confirma. A conta é criada como ativa e verificada, e o convidado é redirecionado para o login.

#### Regras

- Senha: mínimo 8 caracteres, com letra maiúscula, número e caractere especial (validação na tela).
- Telefone: 10 dígitos começando com 0.
- O convite tem prazo de validade. Se passar do prazo ou se já tiver sido usado, aparece "Token de invitación inválido o expirado" — pedir um novo convite.

#### Erros comuns

| Situação | Mensagem | Solução |
|---|---|---|
| Link velho ou já usado | "Token inválido o expirado" | Pedir um novo convite |
| Convite incompleto | "Invitación sin tipo de vínculo definido" | Falar com quem enviou |
| CI duplicada | "CI ya registrada" | Verificar se já tem conta |
| E-mail duplicado | "Este correo ya está en uso" | Idem |

## 2.3 Cadastro de agente do Equipo Colabora (por convite)

Esse fluxo só acontece quando o Master strict do Colabora envia um convite.

#### Fluxo

1. Agente recebe e-mail "Invitación al equipo de Soporte — Colabora".
2. Clica no link e abre a tela "Activar cuenta de agente".
3. Se o convite ainda é válido, aparece o box "Invitación verificada" com nome e e-mail.
4. Agente define a senha (mínimo 8 caracteres) e confirma.
5. Toast: "Cuenta activada. ¡Bienvenido!" — o agente entra direto na bandeja de suporte.

#### Mensagens da tela

- Validando o convite: "Validando invitación…"
- Convite inválido ou vencido: "Invitación no válida" + "Esta invitación ya no es válida." + "Pedile al Master que envíe una nueva. Los enlaces caducan por seguridad."
- Validação de senha: "La contraseña debe tener al menos 8 caracteres" / "Las contraseñas no coinciden".

#### Erros comuns

| Situação | Mensagem | Solução |
|---|---|---|
| Conflito de e-mail | "Este correo entró en conflicto. Pedile al Master una nueva invitación." | Pedir nova invitación |
| Convite inválido | "La invitación ya no es válida." | Idem |
| Falha genérica | "No pudimos activar la cuenta. Intentá de nuevo." | Tentar novamente; se persistir, contatar o Master |

Observação: agentes do Equipo Colabora têm identidade separada da equipe dos órgãos. Não usam o mesmo login.

## 2.4 Master

O Master inicial é criado pela equipe Colabora durante a configuração da instância. Não há tela de cadastro de Master.

---

## SEÇÃO 3 — LOGIN E AUTENTICAÇÃO

O sistema tem três logins distintos.

### Métodos disponíveis

- E-mail + senha (painel do órgão).
- CI + senha (site do cidadão).
- E-mail + senha (Equipo Colabora — agente de suporte).
- Não existe login com Google, Apple ou outros provedores externos.
- Não existe biometria nem "lembrar-me".

## 3.1 Login do painel do órgão

#### Fluxo

1. Equipe abre a tela "Iniciar sesión" do painel.
2. Informa e-mail e senha.
3. Clica em "Entrar" (em loading aparece "Entrando...").
4. Após o login bem-sucedido, é redirecionado para a primeira tela com permissão (geralmente o Dashboard).

#### Erros

| Situação | Mensagem | Solução |
|---|---|---|
| E-mail ou senha errados | "Credenciales inválidas" | Conferir digitação; se necessário, recuperar senha |
| Muitas tentativas seguidas | "Demasiados intentos. Probá de nuevo en unos minutos." | Esperar alguns minutos |
| E-mail não verificado | "Correo no verificado" | Abrir o e-mail e clicar no link de verificação |
| Conta inativa | "Cuenta inactiva" | Falar com o suporte |

## 3.2 Login do cidadão pelo site

#### Fluxo

1. Cidadão abre a tela "Área do Cidadão".
2. Informa CI e senha.
3. Clica em "Entrar".
4. Após o sucesso, é levado para "Meus Protocolos".

#### Mensagens

- Validação: "CI é obrigatório", "Senha é obrigatória".
- Sucesso: "Login realizado com sucesso!"
- Erro genérico: "Erro ao fazer login. Verifique suas credenciais."

Esta tela está em pt-BR.

## 3.3 Login do agente Equipo Colabora

#### Fluxo

1. Agente abre a tela "Equipo de Soporte — Iniciá sesión para acceder a la bandeja".
2. Informa correo e contraseña.
3. Clica em "Entrar".
4. Após o sucesso, entra na bandeja de suporte. Toast: "Bienvenido al Equipo de Soporte".

#### Erros

| Situação | Mensagem |
|---|---|
| Campos vazios | "Ingresá tu correo y contraseña" |
| Credenciais erradas | "Credenciales inválidas" |
| Instabilidade no servidor | "No pudimos conectarnos. Intentá de nuevo en unos minutos." |

Aviso no rodapé: "¿Olvidaste tu contraseña? Contactá al Master para que envíe una nueva invitación."

### Sessão

- O login mantém o usuário conectado por até 24 horas. Depois disso é preciso entrar de novo.
- O sistema permite manter ao mesmo tempo uma sessão de painel e uma sessão de Equipo Colabora em abas diferentes (são sessões independentes).

### Logout

Sair pelo menu do avatar no topo do painel (opção "Cerrar sesión"). O usuário é redirecionado para a tela de login.

### Conta sem vínculo

Se a equipe entra mas ainda não tem papel atribuído, aparece a tela "Esperando vinculación" com:
- "Tu cuenta fue creada con éxito, pero todavía está pendiente de vinculación a una ciudad o secretaría."
- "Contactá al administrador para que vincule tu acceso al equipo correspondiente."
- Botão "Salir".

Solução: falar com o administrador do órgão.

---

## SEÇÃO 4 — RECUPERAÇÃO DE SENHA

## 4.1 Recuperação de senha do painel

#### Passo a passo

1. Na tela de login, clicar em "Esqueceu a senha?".
2. Abre a tela "Recuperar senha — Informe seu e-mail para receber o link de redefinição".
3. Informar o e-mail e clicar em "Enviar link de recuperação".
4. Aparece "E-mail enviado" + "Se o e-mail [seu e-mail] estiver cadastrado, você receberá um link para redefinir sua senha." + "Verifique também sua caixa de spam." (a resposta é sempre neutra, por segurança).
5. Botão "Reenviar em [contador]" libera novo envio após o tempo.
6. Abrir o e-mail e clicar no link.
7. Se o link estiver vencido, aparece "Link inválido ou expirado — Este link de redefinição de senha é inválido, expirou ou já foi utilizado." — pedir um novo.
8. Se válido, abrir a tela "Nova senha". Definir a nova senha (mínimo 8 caracteres, uma letra maiúscula, um número, um caractere especial).
9. Sucesso: "Senha redefinida! Sua senha foi alterada com sucesso. Você pode fazer login com a nova senha." — todas as sessões anteriores são encerradas.

O link de recuperação vale 1 hora. Depois disso, é preciso pedir um novo.

## 4.2 Recuperação de senha do cidadão

Fluxo análogo ao do painel:
- Resposta neutra: "Si el correo estuviera registrado, enviaremos un enlace para restablecer la contraseña."
- E-mail: "Recuperación de acceso — Colabora Ciudadano".
- Sucesso da redefinição: "Contraseña restablecida con éxito".
- Erro de token: "Token inválido o expirado".

## 4.3 Recuperação de senha do agente Equipo Colabora

Não há fluxo automatizado. Se um agente perdeu acesso, o Master strict precisa enviar uma nova invitación pela aba "Equipo de Soporte" das Configurações.

### Casos de erro

| Situação | Mensagem | Solução |
|---|---|---|
| E-mail não cadastrado | Resposta neutra ("Si el correo...") | Conferir se o e-mail está certo; pode ser conta de outro endereço |
| Token vencido ou já usado | "Token inválido o expirado" | Pedir um novo link |
| Muitas tentativas seguidas | "Demasiados intentos. Probá de nuevo en unos minutos." | Aguardar |

---

## SEÇÃO 5 — PERFIS E PERMISSÕES

A plataforma tem hierarquia de papéis. Master e Master Team são equivalentes em quase tudo (a única exceção é Contratos: só Master strict mexe).

## 5.1 Master

- Descrição: dono da instância (único).
- Pode: tudo. Acessa todos os módulos do painel, troca e-mail de cidadão, gerencia Estructura, Contratos, Equipo Master e Equipo de Soporte.
- Não pode: enviar ofício de uma solicitud — apenas visualiza. Se tentar, aparece "El master solo visualiza solicitudes."

## 5.2 Master Team

- Descrição: pares globais autorizados pelo Master.
- Pode: tudo, exceto deletar Contratos. Não vê a aba "Equipo Master" nem "Equipo de Soporte" das Configurações.

## 5.3 Admin de Ciudad

- Descrição: responsável pela cidade e por suas Secretarías Municipales.
- Pode: triagem das solicitudes recebidas pela cidade (marcar como "Verificando" ou "No corresponde"); encaminhar solicitudes; criar/editar Secretarías Municipales; convidar Admin de Secretaría Municipal e Membros de Ciudad; revisar a fila de Revisión da cidade.
- Não pode: marcar solicitudes como "En proceso" ou "Cerrado" (isso é função da unidade resolvedora); acessar Ciudadanos, Contratos, Equipo Master.

## 5.4 Miembro de Ciudad

- Descrição: equipe da cidade.
- Pode: mesmas telas do Admin de Ciudad, com permissões da equipe.

## 5.5 Admin de Secretaría Ministerial

- Descrição: responsável pela secretaria ministerial e por seus Escritórios.
- Pode: triagem das solicitudes recebidas; encaminhar; criar/editar Escritórios; convidar Admin de Escritório e Membros de Secretaría; revisar Revisión da secretaria.

## 5.6 Miembro de Secretaría Ministerial

- Equipe operativa da secretaria.

## 5.7 Admin de Secretaría Municipal (SM)

- Descrição: responsável por uma SM (folha sob uma Ciudad).
- Pode: trabalhar solicitudes ("Verificando" → "En proceso" → "Cerrado") ou devolver com "No corresponde" + motivo (vai para a fila de Revisión da Ciudad); convidar membros da própria SM.
- Não pode: encaminhar para outras unidades.

## 5.8 Miembro de Secretaría Municipal

- Equipe operativa da SM.

## 5.9 Admin de Escritorio

- Descrição: responsável por um Escritorio (folha sob uma Secretaría Ministerial). Mesmo papel que Admin de SM, mas dentro de uma secretaria ministerial.

## 5.10 Miembro de Escritorio

- Equipe operativa do Escritorio.

## 5.11 Ciudadano

- Descrição: usuário do app e do site.
- Pode: criar e acompanhar solicitudes; reagir a notícias; votar em enquetes; cancelar a própria solicitud enquanto está em "Abierta"; abrir tickets de suporte com o Equipo Colabora; editar perfil.
- Não pode: trocar o próprio e-mail (apenas o Master altera); acessar painel; editar solicitud depois de mudada do estado inicial.

## 5.12 Agente do Equipo Colabora

- Descrição: identidade do Equipo Colabora que atende tickets dos órgãos.
- Pode: gerenciar a bandeja de soporte, usar macros, anexar arquivos nas respostas, "tomar atendimento humano" (desativando a IA no ticket).
- Não pode: acessar nenhum módulo do painel do órgão; recuperar a própria senha (precisa de nova invitación do Master).

## 5.13 Resumo de acesso por perfil

| Perfil | Ciudadanos | Estructura | Contratos | Equipo Master | Bandeja Soporte | Operar solicitudes |
|---|---|---|---|---|---|---|
| Master | Sim | Sim | Sim (cria/edita/deleta) | Sim | Acessa via ícone flutuante | Não (só visualiza) |
| Master Team | Sim | Sim | Cria/edita, não deleta | Não | Acessa via ícone flutuante | Não (só visualiza) |
| Admin de Ciudad | Não | Sim (sua cidade) | Não | Não | Acessa via ícone flutuante | Triagem |
| Miembro de Ciudad | Não | Vê | Não | Não | Acessa via ícone flutuante | Triagem |
| Admin de Secretaría Ministerial | Não | Sim (sua secretaria) | Não | Não | Acessa via ícone flutuante | Triagem |
| Miembro de Secretaría Ministerial | Não | Vê | Não | Não | Acessa via ícone flutuante | Triagem |
| Admin de SM | Não | Vê seu nível | Não | Não | Acessa via ícone flutuante | Resolve |
| Miembro de SM | Não | Vê seu nível | Não | Não | Acessa via ícone flutuante | Resolve |
| Admin de Escritorio | Não | Vê seu nível | Não | Não | Acessa via ícone flutuante | Resolve |
| Miembro de Escritorio | Não | Vê seu nível | Não | Não | Acessa via ícone flutuante | Resolve |
| Ciudadano | — | — | — | — | — | Abre/cancela só as próprias |
| Agente Colabora | — | — | — | — | É a bandeja | — |

---

## SEÇÃO 6 — FUNCIONALIDADES PRINCIPAIS

## 6.1 Dashboard

- O que é: tela inicial do painel com indicadores, gráficos e listas resumo.
- Quem pode usar: equipes do órgão, conforme permissão.
- Como acessar: sidebar → "Dashboard".
- O que mostra:
  - Saudação dinâmica ("Buenos días" / "Buenas tardes" / "Buenas noches") + nome.
  - Frase resumo: "Tienes [N] solicitud abierta/s y [N] vencidas esperando atención hoy."
  - Botões: "Nueva solicitud" (terracota) e "Revisión ([N])" se houver pendências.
  - Filtros de período: Hoy, Esta semana, Este mes, Mes anterior, Este año, Todo el período.
  - 4 cards de KPI: "Solicitudes activas", "Vencidas", "Cerradas", "No corresponde".
  - Para Master, linha extra com "Ciudadanos registrados", "Ciudades", "Secretarías ministeriales", "Solicitudes en el período".
  - Para Ciudad / Secretaría Ministerial, linha com "Recibidas por reenvío", "Reenviadas", "Delegadas a Sec. Municipal" (ou Escritorio), "Solicitudes en el período".
  - Donut "Solicitudes por Estado".
  - "Top categorías".
  - "Mapa de calor de solicitudes" com pontos georreferenciados.
  - "Origen de los ciudadanos" (quando há dados).
  - Listas "Últimas Solicitudes" e "Revisión pendiente".

Empty state geral: "Sin datos en el período".

## 6.2 Ciudadanos

- Quem pode usar: Master e Master Team.
- Como acessar: sidebar → "Ciudadanos".
- Header: "Ciudadanos" + contagem de registros.
- O que faz:
  - Lista todos os cidadãos cadastrados.
  - Filtros: busca por nome/e-mail/teléfono; Verificación (Todos / Verificados / Pendientes); Estado (Todos / Activo / Inactivo); Género (Todos / Masculino / Femenino / Otro / Prefiere no decir); Departamento; Distrito.
  - Ordenação: "Más recientes", "Más antiguos", "Nombre (A→Z)", "Nombre (Z→A)", "Verificados primero".
  - Colunas: Nombre, Contacto, Distrito, Estado, Registro, Acciones.
  - Ações por linha: "Editar e-mail" (ícone lápis) e "Ver detalle" (ícone olho).
  - Exportar (CSV) — gera arquivo com colunas: nome, e-mail, telefone, género, departamento, distrito, barrio, verificado, estado, registro.
- Empty: "Sin ciudadanos" + "Cuando una persona se registre por el aplicativo móvil, aparecerá aquí."

## 6.3 Solicitudes (gestão pelo painel)

- O que é: módulo principal de trabalho — receber, triagem, encaminhar e responder solicitudes do cidadão.
- Quem pode usar: papéis operacionais do órgão.
- Header: "Solicitudes" + total da página.
- Filtros (botão "Filtros"): Estado, Prioridad, Categoría.
- Busca: "Buscar por protocolo, título o descripción…"
- Colunas: Protocolo, Título, Categoría, Prioridad, Estado, Responsable (para triagem), Apertura, Acciones.
- Empty: "Sin solicitudes" + "Las solicitudes abiertas por la ciudadanía aparecerán aquí."

#### Ações por linha

| Ação | Quem pode | Quando | O que faz |
|---|---|---|---|
| Encaminar a responsable (avião verde) | Cidade-admin / Sec-admin | Solicitud não fechada | Abre o modal "Enviar solicitud" para escolher destino (Secretaría Municipal, Secretaría Ministerial, Otra Ciudad, Escritorio, etc.) |
| Marcar como no corresponde (X vermelho) | SM / Escritorio | Solicitud não fechada nem já como no corresponde | Abre o modal "Marcar como no corresponde" com campo "Motivo (obligatorio)" — a solicitud vai para a fila de Revisión |
| Ver detalle (ícone olho) | Todos | Sempre | Abre o detalhe |

#### Detalhe da solicitud

- Header: "Protocolo [N]" + título + badges de estado, prioridade e categoria + categoria · subcategoria.
- Card "Documento" (ofício PDF): permite gerar, regenerar, baixar, enviar por e-mail ou eliminar.
- "Descripción" da solicitud.
- Se estado for "no_corresponde", aparece um box vermelho "Enviada a Revisión — motivo" + data + texto do motivo.
- Campos: Ciudadano (nome, e-mail, teléfono), Responsable, Sub-responsable, Apertura, Última actualización, Ubicación del problema (link para Google Maps se houver), Ubicación del ciudadano al reportar.
- Anexos enviados pelo cidadão (cada um com link de download).
- Se houver resposta, mostra "Respuesta de conclusión" com texto e anexos da resposta.
- Se fechada, aparece o box verde "Solicitud cerrada. No puede cambiar de estado ni recibir nuevas respuestas."
- Bloco "Cambiar estado":
  - Para triagem: opção única "Verificando" (ou "No corresponde" com motivo).
  - Para resolvedor: "Verificando", "En proceso", "Cerrado", "No corresponde".
  - Hint para triagem: "Ciudad y Secretaría ministerial sólo triavan. Para resolver, asigne a una SM/Escritório."
  - Marcar "no_corresponde" exige preencher "Motivo (obligatorio)" — hint: "La solicitud va a Revisión, donde Ciudad/Secretaría ministerial puede redirigirla."
- Tabs da coluna esquerda: "Chat" (com o cidadão) e "Notas internas" (visíveis só pra equipe — banner amber: "Solo el equipo lo ve — invisible para el ciudadano.").

#### Gerar e enviar ofício

- Dialog "Generar documento": preencher Asunto, Contenido (com placeholders `{{variables}}`), e os valores das variáveis detectadas (Destinatario, Organización, Ciudad del destinatario, Departamento, Firmante, Cargo, etc.). Botão "Generar y guardar".
- Dialog "Enviar documento por e-mail": Destinatarios (separados por coma), Cambiar estado al enviar (em_aberto / verificando / em_proceso / fechado), Asunto, Contenido. Botão "Enviar". Toast: "Documento enviado".
- Master é bloqueado: "El master solo visualiza solicitudes."

#### Estados da solicitud

| Estado (mostrado no painel) | Significado |
|---|---|
| Abierto | Criada; ainda não olhada pelo órgão |
| Verificando | Em análise |
| En proceso | Aceita; sendo trabalhada |
| Cerrado | Resolvida ou respondida definitivamente |
| No corresponde | O órgão entende que não é responsabilidade dele |

O cidadão pode cancelar a própria solicitud (estado "cancelada"), mas o painel não cancela em nome do cidadão.

No site do cidadão, os mesmos estados aparecem em português ("Aberta", "Em Análise", "Em Andamento", "Concluída", "Indeferida").

#### Regras importantes

- "No corresponde" sempre exige motivo entre 3 e 500 caracteres.
- Não há reabertura de solicitud fechada. Se precisar, o cidadão abre uma nova.
- Chat com o cidadão: até 4000 caracteres por mensagem; limite anti-flood de 30 mensagens por minuto; sem anexos no chat (anexos vão na solicitud em si).

#### Erros comuns

| Situação | Mensagem |
|---|---|
| Tentar editar fora do estado inicial | "Solo se pueden editar solicitudes en estado 'em_aberto'." |
| Tentar cancelar fora do estado inicial | "Solo se pueden cancelar solicitudes en estado 'em_aberto'." |
| Enviar form vazio | "Envíe al menos un campo para actualizar." |
| Mensagem em solicitud fechada | "Esta solicitud ya está cerrada; el chat no acepta nuevas mensajes." |
| Flood do chat | "Demasiados mensajes en poco tiempo. Esperá un momento antes de seguir." |
| Master tenta enviar documento | "El master solo visualiza solicitudes." |
| SM/Escritorio sem destino direto | "Esta secretaría municipal no recibe solicitudes directas." / "Este escritorio no recibe solicitudes directas." |

## 6.4 Revisión

- O que é: fila de solicitudes marcadas como "No corresponde" pelas folhas (SMs e Escritorios), aguardando a Ciudad ou Secretaría Ministerial redirigir para o destino correto.
- Quem pode usar: Master, Admin de Ciudad, Admin de Secretaría Ministerial.
- Como acessar: sidebar → "Revisión".
- Header: "Revisión" + "X solicitud(es) pendiente(s) de redirigir".
- Empty: "Sin solicitudes pendientes" + "Cuando una solicitud sea marcada como 'no corresponde' aparecerá aquí para ser redirigida."
- Botão "Redirigir" abre o modal "Redirigir solicitud": escolher tipo de destino (cidade ou secretaría), categoria + subcategoria, destino. Toast: "Solicitud redirigida correctamente".

## 6.5 Productividad (Agenda + Tarefas)

- O que é: agenda de compromissos e kanban de tarefas operacionais.
- Como acessar: sidebar → "Productividad".
- Duas abas: "Agenda" (calendário) e "Tarefas" (kanban).
- Botão verde no canto superior direito: "Novo Compromisso" ou "Nova Tarefa".

#### Agenda

- Calendário mensual. Dias da semana: Dom, Lun, Mar, Mié, Jue, Vie, Sáb.
- Cada compromisso tem cor própria. Máximo 2 visíveis por dia + "+N mais".
- Painel lateral lista os compromissos do dia. Empty: "Nenhum compromisso agendado".
- Ações por compromisso: Visualizar, Editar, Excluir.
- Ao excluir: "¿Eliminar este compromiso? '[título]' será eliminado de la agenda."

#### Tarefas

- 3 colunas: "Por Hacer", "En Curso", "Concluida".
- Drag-and-drop entre colunas muda o status.
- Card: título, descrição, badge de prioridade, prazo (vermelho se vencido), responsável.
- Prioridades: "Baja", "Media", "Alta", "Urgente".
- Ações: Visualizar, Editar, Excluir.

## 6.6 Estructura

- O que é: organograma de Ciudades + Secretarías Ministeriales e suas unidades (Secretarías Municipales e Escritorios).
- Quem pode usar: Master, Admin de Ciudad (vê só sua cidade), Admin de Secretaría Ministerial (vê só sua secretaria).
- Header: "Estructura organizacional" + "Ciudades y secretarías, sus órganos internos y quién recibe solicitudes directo en el app."
- Duas abas: "Ciudades" (MapPin) e "Secretarías ministeriales" (Landmark).
- Layout master-detail. Lista à esquerda; detalhe à direita.
- Botão primário: "Nueva ciudad" ou "Nueva secretaría".

#### Ações no detalhe

| Estado | Ações disponíveis |
|---|---|
| Tem administrador | "Quitar administrador" |
| Tem invite pendente | "Reenviar invitación", "Cambiar e-mail", "Revocar invitación" |
| Sem administrador | "Asignar administrador" |
| Sempre | "Editar" e "Eliminar" |

#### Unidades-folha (Secretaría Municipal / Escritorio)

- Cada uma tem dropdown: "Gestionar equipo", "Editar", "Eliminar".
- Switch "Recibe solicitudes en el app" controla se a unidade aceita destino direto do cidadão sem passar pela triagem.
- Mensagem do switch: "Directo a [tipo], sin pasar por triaje." ou "Hoy llega por triaje. Activá para recibir directo."

#### Modal "Asignar administrador"

- Título: "Asignar administrador de la ciudad" ou "Asignar administrador de la secretaría".
- Texto: "El administrador de [nome] recibirá una invitación por correo."
- Campos: E-mail (obrigatório) e Nombre (opcional).
- Toast: "Invitación enviada".

#### Modal "Cambiar e-mail de la invitación"

- Texto: "La invitación actual para [e-mail] será cancelada y se enviará una nueva."
- Campo: Nuevo e-mail.
- Toast: "Invitación enviada al nuevo e-mail".

#### Chip de tempo do convite

Cada convite pendente mostra quanto tempo falta para expirar:
- Já vencido: cor vermelha, "Expirada".
- Menos de 24 horas restando: cor terracota, "[N]h restantes".
- 24 horas ou mais: cor neutra, "[N]h restantes".

O convite vale 48 horas. Depois disso é preciso reenviar ou criar um novo.

## 6.7 Contratos

- Quem pode usar: apenas Master strict.
- Como acessar: sidebar → "Contratos".
- Header: "Contratos" + "Contratos firmados con cada Ciudad o Secretaría Ministerial. Cada órgano puede tener como máximo un contrato activo a la vez."
- Stats: Total, Activos, Facturación mensual (em PYG).
- Estados: "Borrador", "Activo", "Suspendido", "Cerrado".
- Empty: "Aún no hay contratos" + "Cree el primer contrato vinculándolo a una ciudad o secretaría."
- Ações por contrato: Editar / Eliminar.
- Ao eliminar: "¿Eliminar contrato? '[nome]' será marcado como eliminado. Esta acción es reversible solo a través de la base de datos — desde la interfaz, el contrato no aparecerá más."

#### Form de contrato

- Nombre (placeholder "Ej: Contrato Gobernación Central 2026").
- Tipo de órgano: "Ciudad" ou "Secretaría Ministerial" (só ao criar).
- Plan (vem dos planos cadastrados em Configuración → Planes). Hint: "Gestioná los planes en Configuración › Planes."
- Inicio (data, obrigatória) e Vencimiento (opcional).
- Valor mensual em PYG.
- Estado.
- Notas (opcional).
- Toasts: "Contrato creado" / "Contrato actualizado".

## 6.8 Aplicativo (hub)

Tela que reúne os módulos de conteúdo do app móvel. Não é menu — é um conjunto de cards.

- Header: "Aplicación" + "Todo lo que la ciudadanía ve y usa en la app, en un solo lugar."
- Grupo "Comunicación" — "Lo que le comunicás a la ciudadanía en la app.":
  - "Avisos" — "Notificaciones y comunicados directos al app."
  - "Noticias" — "Publicaciones de novedades para la ciudadanía."
  - "Encuestas" — "Consultá a la ciudadanía y seguí los resultados."
- Grupo "Tu ciudad" — "Servicios y contenido local.":
  - "Mi Ciudad" — "Guía de lugares útiles y agenda de eventos locales."
  - "Empleo Público" — "Vacantes nacionales y municipales + candidaturas."

Cada card tem link "Abrir".

## 6.9 Avisos

- O que é: comunicados rápidos publicados no app do cidadão.
- Header: "Avisos" + "Comunicados rápidos para los ciudadanos del aplicativo".
- Stats: Total, Activos, Borradores, "Críticos activos", Inactivos.
- Filtros: busca, Estado (Todos los estados / Borrador / Activo / Cerrado), Prioridad (Todas las prioridades / Crítica / Alta / Media / Baja).
- Empty: "No hay avisos con estos filtros".

#### Formulário

- Título: 3 a 200 caracteres ("Ej: Mantenimiento de la red"). Contador "X/200".
- Descripción: mínimo 3 caracteres.
- Prioridad: Baja / Media / Alta / Crítica.
- "Publicar desde (opcional)" — em branco publica já; data futura agenda.
- "Expira en (opcional)" — sem data, não expira.
- Hint amplo: "Avisos con prioridad crítica disparan push inmediato a los ciudadanos del alcance. Sin publicar desde el aviso se publica ya. Sin expira en, no expira automáticamente. Si cerrás esta ventana sin cancelar, se guarda como borrador."
- Botões: "Cancelar", "Guardar como borrador", "Programar publicación" (data futura) ou "Publicar ahora".
- Validações: "El título debe tener al menos 3 caracteres" / "La descripción debe tener al menos 3 caracteres" / "La fecha de expiración debe ser posterior a la de publicación".
- Toasts: "Borrador guardado" / "Aviso programado" / "Aviso publicado" / "Error al guardar el aviso".

Auto-save: fechar sem cancelar grava como rascunho.

Confirmação ao eliminar: "¿Eliminar este aviso? '[título]' dejará de aparecer en el aplicativo. Podrás restaurarlo desde la lista de eliminados."

## 6.10 Noticias

- O que é: conteúdo editorial publicado no app.
- Header: "Noticias" + "Contenido editorial publicado en el aplicativo".
- Stats: Total, Activas, Borradores, Reacciones.
- Filtros: busca, Tipo ("Todos los tipos" + lista), Status (Borrador / Activa / Cerrada).
- Tipos disponíveis: Comunicados, Ciudad, Proyectos, Salud, Educación, Medio Ambiente, Infraestructura.
- Empty: "No hay noticias con estos filtros".
- Card: thumbnail, status, tipo, cidade (ou "Nacional"), chip "Bajo alcance" se ativa há mais de 24 h sem reações.

#### Formulário

- Tabs: Editor + Preview.
- Título, Descrição (resumo), Tipo, Status, "Programar publicación" (opcional).
- Hint de alcance: "Se publicará en [cidade] — visible a los ciudadanos de la región." ou "Se publicará a nivel nacional — visible a todos los ciudadanos."
- Bloco "Imagem de Capa".
- Bloco "Conteúdo (blocos)": permite combinar Texto (Markdown), Imagem ou Vídeo. Botões para adicionar cada tipo.
- Bloco "Arquivos para Download": aceita documentos (PDF, DOC, XLS, PPT, TXT, CSV, ZIP, etc.).
- Toast erro arquivo: "Apenas documentos são permitidos (PDF, DOC, XLS, PPT, TXT, CSV, ZIP...)".
- Botões rodapé: Cancelar / "Salvar como rascunho" / "Postar" (criar) ou "Salvar" (editar).

#### Ações por linha

- Vista previa, Editar, Duplicar.
- "Publicar" (se não ativa) ou "Cerrar" (se ativa).
- Eliminar.

Confirmação: "¿Eliminar esta noticia? '[título]' dejará de aparecer en el aplicativo."

Reações no app do cidadão: 1 emoji por cidadão por notícia. Tocar de novo no mesmo emoji remove.

## 6.11 Encuestas

- O que é: consultas de opinião publicadas no app.
- Header: "Encuestas" + "Consultas de opinión publicadas en el aplicativo".
- Stats: Total, Activas, Borradores, "Votos totales".
- Filtros: busca + Status (Borrador / Activa / Cerrada).

#### Formulário

- Título (obrigatório, ex.: "¿Cuál es tu prioridad para el barrio?").
- Descripción (opcional, "Contexto opcional para el ciudadano").
- Imagen (opcional).
- "Opciones de voto" (mínimo 2). Botão "Agregar" para mais. A partir da 3ª, pode remover.
- Hint de alcance (igual ao de notícia).
- Estado: Borrador / Activa / Cerrada.
- "Inicio" e "Cierre" (datas).
- Validações: "La fecha de cierre debe ser posterior a la de inicio" / "La encuesta debe durar al menos 15 minutos".
- Aviso se data futura: "Programada: la encuesta quedará oculta para los ciudadanos hasta [data]."
- Botões: Cancelar / "Guardar borrador" + "Publicar" (criar) ou "Guardar" (editar).

#### Ações por enquete

- Ver detalles, Vista previa, Copiar link (toast "Link copiado"), Duplicar, Editar, "Activar", "Cerrar", Eliminar.

No app, o cidadão vota uma única vez. Depois aparece o resultado em barras de porcentagem. Mensagens do cidadão:
- "Esta encuesta no está activa para votación"
- "La votación todavía no empezó"
- "La votación ya finalizó"
- "Opción de voto inválida"
- "Ya votaste en esta encuesta"

## 6.12 Mi Ciudad

- O que é: conteúdo local da cidade — Guia de lugares e Eventos. (Vagas ficam em Empleo Público, módulo separado.)
- Header: "Mi Ciudad" + "Guía y eventos locales que la ciudadanía ve en la app, organizados por ciudad."
- Empty (sem cidades): "Sin ciudades todavía" + "Creá una ciudad en Estructura para empezar a publicar su guía y eventos."
- Master-detail. Busca "Buscar ciudad…"
- Dois segmentos: "Guía de la Ciudad" e "Eventos".

#### Guia

- Empty: "Sin guía todavía" + "Creá un grupo (Salud, Servicios, Seguridad…) y agregá los lugares útiles de la ciudad."
- Cada grupo: nome, cor, contagem de lugares, botão "Lugar" para adicionar.
- Cada lugar: nome, barrio, telefone, chip "24h" quando aplicável.

#### Eventos

- Empty: "Sin eventos todavía" + "Publicá la agenda local: cultura, educación, shows y deporte de esta ciudad."
- Tipos: Cultura, Educación, Show, Deporte, Universitario, Institucional.
- Status: "Publicado" ou "Borrador".

Confirmações de delete: "¿Eliminar este grupo?" / "¿Eliminar este lugar?" / "¿Eliminar este evento?"

## 6.13 Empleo Público

- O que é: publicação de vagas e gestão de candidaturas.
- Header: "Empleo Público" + "Vacantes del sector público que la ciudadanía ve en la app."
- Filtros: "Todas", "Nacionales", "Municipales".
- Estados de vaga: "Borrador" (amber), "Publicada" (verde), "Cerrada" (azul).
- Empty: "Sin vacantes todavía" + "Publicá una vacante para que la ciudadanía pueda postularse desde la app."

#### Card de vaga

- Título, departamento, alcance ("Nacional" ou cidade), estado, contagem de candidatos, "cierra DD/MM" ou "sin fecha".
- Botão "Nueva vacante" (terracota) para quem pode criar.

#### Detalhe da vaga

- Botão "Editar" e dropdown com "Eliminar".
- Card "Candidatos (N)" com busca "Buscar candidato…".
- Empty: "Todavía no hay candidaturas para esta vacante."

#### Estados da candidatura

- "Nueva", "En revisión", "Aprobada", "Rechazada", "Cancelada".

#### Drawer do candidato

- Datos del candidato (e-mail, telefone, CI, "Postuló el [fecha]", vaga).
- Carta de presentación.
- Currículum (botão "Ver / descargar CV").
- Estado da candidatura: pills clicáveis para mudar o estado.

#### Mensagens do app do cidadão (candidatura)

- Sucesso: "Candidatura enviada con éxito."
- Tipo de arquivo errado: "El currículum debe ser PDF, DOC o DOCX".
- Vaga fechada: "Vacante cerrada — no se aceptan candidaturas".
- Vaga inexistente: "Vacante no encontrada".

Currículo aceito: PDF, DOC ou DOCX, até 10 MB.

Confirm delete: "¿Eliminar esta vacante? '[título]' será movida a eliminadas."

## 6.14 Informes

- O que é: análise das solicitudes, com filtros, gráficos e exportação.
- Como acessar: sidebar → "Informes".
- Header: "Informes" + "Análisis y exportación de solicitudes".
- Botões: "Actualizar" e "Exportar CSV" (terracota).
- Filtros:
  - Período: Hoy, Esta semana, Este mes, Mes anterior, Este trimestre, Este año, Todo el período, Personalizado.
  - Estado, Prioridad, Categoría.
  - Filas por página: 25, 50, 100, 200.
  - Dimensões (quando aplicáveis): Cidade, Secretaría Ministerial, Secretaría Municipal, Escritório, Departamento.
  - Chips "Contabilizar en el informe": Ciudades / Sec Ministeriales / Sec Municipales / Escritórios.
- KPIs: "Total filtrado", "Activas", "Vencidas", "Cerradas".
- Gráficos: Por estado (pie), Línea temporal — abiertas vs cerradas, Top categorías, Top destinos, Mapa de calor, Origen — departamentos.
- Tabela final: Protocolo, Estado, Título, Categoría, Prioridad, Ciudadano, Destino, Plazo, Creado, Resol. (d).
- Exportar CSV gera arquivo `informes_[data].csv`. Toast: "Exportación lista — descarga iniciada".

## 6.15 Configurações

- Header: "Configurações" + "Gerencie as configurações do sistema".
- Abas variam por papel:

| Aba | Visível para | O que faz |
|---|---|---|
| Mi Ciudad / Mi Secretaría / Mi Secretaría Municipal / Mi Escritorio | Equipe do órgão (não Master) | Mostra dados do próprio órgão (read-only — edição via Estructura) |
| Plantilla de e-mail | Equipe do órgão (não Master) | Configura o modelo do e-mail de ofício |
| Equipo | Equipe do órgão (não Master) | Convida e gerencia membros do próprio órgão |
| Aplicativo | Master | Configura o menu do app móvel |
| Meu País | Master | Edita dados do país exibidos no app |
| Categorías | Master | Gerencia categorias e subcategorias de solicitud |
| Planes | Master | Gerencia planos comerciais |
| Equipo de Soporte | Master strict | Convida agentes do Equipo Colabora |

#### Mi Ámbito (read-only)

- Mostra parent (se SM ou Escritorio), administrador, status, departamento, distrito, descripción.
- Sem administrador: "Sin administrador asignado."

#### Plantilla de e-mail

- Editor com variáveis arrastáveis: `{{numero}}`, `{{destinatario}}`, `{{asunto}}`, `{{responsavel}}`, `{{cargo}}`, `{{dia}}`, `{{mes}}`, `{{ano}}`.
- Campos: "Asunto del Correo" e "Cuerpo del Correo" (com placeholder "Ej: Estimado(a) {{destinatario}}, Adjunto el documento {{numero}} referente a '{{assunto}}'.").
- Botão "Guardar Plantilla".
- Toasts: "Plantilla de correo guardada con éxito" / "Error al guardar".

#### Equipo

- Quem é admin: aparece com badge "Admin" + "Eres tú" quando for o próprio usuário.
- Empty: "Sin administrador asignado. Sin equipe, no se podrán encaminar solicitudes."
- Botão "Invitar miembro" (só com permissão de gerenciar).
- Modal: "Invitar miembro" + "El nuevo miembro va a recibir una invitación por correo. Al aceptar, podrá ayudar a trabajar las solicitudes y demás recursos." Campos E-mail + Nombre (opcional). Toast: "Invitación enviada".
- AlertDialog remover: "¿Quitar del equipo? [nome] dejará de tener acceso. La cuenta sigue activa, solo se remueve el vínculo con este ámbito."
- Quando o usuário não tem âmbito: "No tienes ámbito asociado" + "Pídale al administrador del nivel superior que lo asocie a una ciudad / secretaría / SM / escritorio."

#### Aplicativo (menu do app móvel)

Itens configuráveis (drag-and-drop + switch on/off):
- "Mis Solicitudes" — "Listado de solicitudes y nuevas peticiones".
- "Noticias" — "Últimas noticias y actualizaciones".
- "Encuestas" — "Encuestas y consultas públicas".
- "Mi Ciudad" — "Guía, vagas y eventos de la ciudad".
- "Servicios" — "Catálogo de servicios disponibles".
- Toast: "Configurações do menu salvas com sucesso!"

#### Meu País

- Cabeçalho h2 "Meu País" + "Informações públicas exibidas no aplicativo na tela 'Sobre o País'".
- Campos: Nome do País (obrigatório), Descrição, Endereço, Telefone, E-mail.
- Preview do celular ao lado.
- Toast: "Meu País salvo com sucesso!"

#### Categorías

- CRUD de categorias e subcategorias de solicitud.
- Header: "Categorías de solicitudes" + botão "Nueva categoría".
- Cada categoria: nome + badge "inactiva" se não ativa + contador de subcategorías + ações ("Agregar subcategoría", "Editar", "Eliminar").
- Form: Nombre (até 120 caracteres), Orden, Estado (checkbox "Activa").
- Confirm: "Se eliminará '[nome]'. Esta acción no se puede deshacer."

7 categorias chegam configuradas por padrão:
1. Infraestructura
2. Servicios Básicos
3. Seguridad Vial
4. Espacios Públicos
5. Medio Ambiente
6. Trámites y Servicios
7. Otros

#### Planes

- CRUD de planos comerciais usados em Contratos.
- Header: "Planes" + "Planes comerciales que se pueden asignar a un contrato."
- Botão "Nuevo plan" (terracota).
- Empty: "Todavía no hay planes" + "Creá el primer plan para empezar a vincularlo a los contratos."
- Cada plano: Nombre + Detalle.
- Modal: Nombre (placeholder "Ej.: Estándar, Pro, Premium…"), Detalle.
- Validações: "Ingresá un nombre." / "Ingresá el detalle."
- Toasts: "Plan creado." / "Plan actualizado." / "Plan eliminado."

#### Equipo de Soporte

- CRUD de agentes do Equipo Colabora. Aba só visível para Master strict.
- Header: "Equipo de Soporte" + "Gestioná los agentes del Equipo Colabora que atienden la bandeja de soporte. Cada agente tiene su propia identidad y entra por una URL dedicada."
- Botão "Invitar agente".
- Lista "Agentes activos": empty "Aún no hay agentes en el equipo" + "Invitá a alguien para empezar a atender la bandeja de soporte."
- Cada agente: nome, badge "Agente", e-mail, "Activo [tempo]" ou "Nunca ingresó", "Desde [data]", "Invitado por [nome]". Dropdown "Quitar del equipo".
- Lista "Invitaciones pendientes": empty "Sin invitaciones pendientes." + "Las que envíes aparecerán acá mientras esperan respuesta."
- Modal Invitar: "Invitar agente al Equipo de Soporte" + "El nuevo agente va a recibir una invitación por correo. Al aceptar, podrá iniciar sesión en la bandeja del Equipo Colabora." Campos: Correo + Nombre (opcional). Toast: "Invitación enviada a [e-mail]".
- AlertDialog remover: "¿Quitar agente del equipo? [nome] dejará de tener acceso a la bandeja de soporte. La acción es inmediata." Toast: "Agente removido del equipo".
- AlertDialog revogar invite: "¿Revocar invitación? El enlace enviado a [e-mail] dejará de funcionar. Podés enviar una nueva en cualquier momento." Toast: "Invitación revocada" / "Invitación reenviada".

## 6.16 Equipo Master

- Apenas Master strict.
- Header: "Quien tiene las llaves" + "Los miembros listados aquí comparten el control total de la plataforma con el Master. Cada acceso queda registrado."
- Botão "Invitar miembro".
- Cada membro: avatar, nome, badge Master ou "Eres tú", e-mail, telefone, "Activo [tempo]" ou "Nunca ingresó", "Desde [data]". Dropdown (não para si nem para o Master): "Quitar del equipo".
- Lista de invites pendentes (mesmo padrão de outras seções).
- Modal Invitar: "Invitar al equipo Master" + "El invitado recibirá un correo con un enlace de activación. Al aceptar, tendrá permisos totales sobre la plataforma — igual que el Master." Campos E-mail + Nombre (opcional). Toast: "Invitación enviada".
- AlertDialog remover: "¿Quitar del equipo Master? [nome] dejará de tener permisos totales en la plataforma. La cuenta del usuario sigue activa, solo se remueve el rol."

## 6.17 Suporte (lado órgão — cliente do suporte)

- Acesso: ícone flutuante "Headset" no canto da tela. Não é item de menu.
- Quem vê: qualquer usuário autenticado do painel (exceto agente puro do Equipo Colabora).
- Janela:
  - Lista de tickets do próprio órgão, em duas seções: "Em aberto" e "Encerrados".
  - Empty: "Tudo em dia!" + "Nenhum ticket em aberto".
  - Botão "Novo Ticket" (terracota).
- Form de novo ticket:
  - Assunto, Mensagem.
  - Categoria: Dúvida, Erro no Sistema, Financeiro, Contrato, Sugestão, Implantação, Outro.
  - Prioridade: Baixa, Média, Alta, Urgente.
  - Toast sucesso: "Ticket [código] creado". Erro: "Erro ao criar ticket".
- Chat com o suporte: mensagens em bolhas; anexos suportados; aviso "Ticket [estado]" se o ticket estiver fechado ou cancelado.
- Estados visíveis: Aberto, Em Andamento, Aguardando, Resolvido, Fechado, Cancelado.

## 6.18 Equipo de Soporte (lado agente)

- Acesso: o agente entra pela URL dedicada do Equipo Colabora.
- Layout em 3 colunas: filtros / lista de tickets / conversa.

#### Filtros

- Tabs: "Míos", "Todos", "Sin asignar".
- Estado: Abierto, En atención, Esperando cliente, Resuelto.
- Canal: App ciudadano, Panel operador.
- Prioridad: Crítico, Alto, Medio, Bajo.
- Etiquetas: Duda, Error, Financiero, Contrato, Sugerencia, Onboarding, Otro.
- SLA: "Próximos a vencer", "SLA vencido".

#### Lista de tickets

- Busca: "Buscar #código, asunto, cliente…"
- Ordenar: "SLA + prioridad", "Más recientes", "Más antiguos".
- Empty: "Sin tickets que coincidan con los filtros" + "Probá ajustando estado, canal o prioridad."
- Erro: "No se pudo cargar la bandeja. Reintentando…"

#### Conversa

- Mensagens em bolhas. Bolha violeta com badge "IA" indica resposta automática; amber indica nota interna ("Nota interna — Solo agentes ven esto"); branca à esquerda é cliente; petrol à direita é agente.
- Recibo: "✓ entregado" e "✓✓ leído".
- Empty: "Sin mensajes todavía."

#### Composer

- Tabs: "Responder" e "Nota interna" (com aviso "Solo agentes ven esto").
- Contador "[N] / 4000".
- Macros (atalhos de resposta pronta) — empty: "Sin macros configuradas."
- Botão "Atención humana" (com hint "Tomar la conversación y desactivar la IA en este ticket") — toast "Atención humana iniciada" / erro "No se pudo iniciar la atención humana".
- Botão Send: "Enviar respuesta" ou "Guardar nota" — "Enviando…" em curso.

#### Estados (lado agente)

- "Abierto", "En atención", "Esperando cliente", "Resuelto", "Cerrado".

#### Pesquisa de satisfação (CSAT)

Quando o ticket é resolvido ou fechado, abre automaticamente um dialog pedindo avaliação de 1 a 5 estrelas e um comentário opcional (até 500 caracteres). Não dá para avaliar duas vezes ("Ya enviaste tu evaluación").

## 6.19 Configuração de notificações do agente

- Cards:
  - "Mi perfil": foto (até 5 MB — "Imagen muy grande. Máximo: 5MB"), nome, e-mail (não editável).
  - "Sonidos": switch "Reproducir sonido" + slider Volumen + botão "Probar".
  - "Notificaciones del navegador (web push)": switch + indicador de estado ("Activadas", "Permiso negado", "No soportado por este navegador", etc.). Aviso quando indisponível: "Tu navegador no soporta web push, o falta configuración del servidor. Probá con Chrome, Firefox o Edge actualizados. En Safari, requiere macOS 13+ o iOS 16.4+."
  - "Toast en pantalla": switch "Mostrar toast en pantalla".

## 6.20 Troca de e-mail de cidadão

Quem altera é o Master, na tela Ciudadanos:

1. Master abre o cadastro do cidadão e clica em "Editar e-mail".
2. Dialog "Editar e-mail del ciudadano" com hint: "Se enviará un correo de verificación al nuevo e-mail. El cambio entra en vigor cuando el destinatario lo autorice."
3. Master informa o novo e-mail e clica em "Enviar verificación". Toast: "Enviamos un correo a [destino] para confirmar el cambio."
4. O dono do novo e-mail recebe um e-mail "Autorizá el cambio de e-mail — Colabora Ciudadano" e clica no link.
5. Página abre e confirma automaticamente. Estados:
   - Em andamento: "Confirmando el cambio de e-mail..."
   - Sucesso: "¡E-mail actualizado! Nuevo e-mail: [e-mail]. Ya podés iniciar sesión en la aplicación con el nuevo e-mail."
   - Erro: "Enlace inválido o vencido" + detalhe.

O link vale 1 hora. A senha não muda. Não dá para usar um e-mail já cadastrado em outro cidadão.

Mensagens possíveis:
- "El nuevo e-mail es igual al actual."
- "Ya existe un ciudadano con ese e-mail."
- "Se envió un correo de verificación al nuevo e-mail."
- "Token requerido." / "El enlace es inválido o ya expiró."
- "El cambio de e-mail fue confirmado."

## 6.21 Meus Protocolos (site do cidadão)

- Cabeçalho gradiente verde: "Olá, [nome]" + título "Meus Protocolos" + botão de sair.
- Contador "Solicitações ([N])".
- Empty: "No se encontraron solicitudes".
- Cada card: chip de protocolo, badge de status, assunto, prévia da descrição, data, chip verde "Respondida" se houver resposta.
- Detalhe ao clicar:
  - Cabeçalho gradiente verde com "Solicitud" + protocolo + status.
  - Dados: assunto, descrição, data, prioridade, localização, vereador responsável.
  - Card "Resposta da Prefeitura" quando houver.
  - Card "Interações ([N])" com as mensagens (cidadão à esquerda, órgão à direita). Empty: "Nenhuma interação ainda". É somente leitura — não há envio pelo site.
  - Card "Anexos ([N])" com nomes dos arquivos enviados.

Estados mostrados no site (em pt-BR): Aberta, Em Análise, Em Andamento, Concluída, Indeferida.

## 6.22 Meu perfil (modal no topbar do painel)

- Avatar + nome + e-mail + telefone + badge do papel.
- Botão "Editar":
  - Botão "Quitar foto de perfil" (se há foto).
  - Campos: Nombre (obrigatório), Teléfono.
  - Botões: "Guardar cambios" / "Cancelar".
- Toasts: "Perfil actualizado", "Foto actualizada", "Foto eliminada".
- Validação: "El nombre es obligatorio", "Imagen muy grande. Máximo: 5MB".

#### Seção "Seguridad"

- "Cambiar e-mail": informar nuevo e-mail + senha atual. Toast: "Verificá el nuevo e-mail para confirmar el cambio".
- "Cambiar contraseña": senha atual + nova senha + confirmação. Toast: "Contraseña cambiada. Iniciá sesión nuevamente." (faz logout automático).

---

## SEÇÃO 7 — INTEGRAÇÕES E SERVIÇOS EXTERNOS

Esta seção descreve apenas o que o usuário precisa saber sobre comportamentos do sistema. Detalhes técnicos são responsabilidade da equipe Colabora.

## 7.1 E-mail

O sistema envia e-mails automaticamente em vários momentos (verificação de cadastro, recuperação de senha, troca de e-mail, convites de equipe, ofícios). Detalhes na Seção 8.

Se o usuário não recebe um e-mail, recomendar:
- Conferir se digitou o endereço certo.
- Olhar a pasta de spam.
- Aguardar alguns minutos.
- Se ainda assim não chegar, encaminhar ao Equipo Colabora.

## 7.2 Notificações push (celular)

Para o cidadão, o sistema envia notificações push para o celular (status da solicitud, resposta do órgão, novos avisos/notícias/enquetes/vagas/eventos).

Limite: 10 dispositivos por cidadão. Mensagem em excesso: "Límite de 10 dispositivos alcanzado."

Se o cidadão não recebe push:
1. Conferir permissão de notificações no sistema do celular.
2. Conferir Configuración > Notificaciones no app (toggles de atualizações de solicitudes, notícias, avisos, enquetes, votações).
3. Se trocou de celular muitas vezes, pode ter passado do limite — falar com o suporte para limpar tokens antigos.

## 7.3 Notificações no navegador (Equipo Colabora)

A bandeja do agente pode mostrar notificações no sistema operacional. Em alguns navegadores ou ambientes pode aparecer "No soportado por este navegador" — usar Chrome, Firefox ou Edge atualizados; Safari só funciona em macOS 13+ ou iOS 16.4+.

## 7.4 Atendimento inicial por IA (suporte)

Os tickets de suporte podem receber resposta automática inicial de uma IA. O agente humano pode "tomar atendimento" a qualquer momento, desativando a IA naquele ticket — o histórico registra: "Atención humana iniciada — IA desactivada en este ticket".

## 7.5 Mapa (geolocalização)

Solicitudes guardam a localização atual no envio (obrigatória) e, opcionalmente, a localização do problema. No detalhe, o painel oferece link para abrir o ponto no Google Maps. Anexos com informação de GPS (foto com EXIF) mostram um chip "Ubicación" verde com link para o mapa.

## 7.6 Possíveis falhas que o usuário percebe

| Situação | O que o usuário vê | O que fazer |
|---|---|---|
| Push não chega | Sem notificação | Conferir permissões; falar com suporte se persistir |
| Web push do navegador indisponível | Toggle "No soportado por este navegador" | Atualizar navegador ou usar outro |
| E-mail não chega | Caixa vazia | Conferir spam; aguardar; falar com suporte |
| Anexo não sobe | Erro de upload | Conferir tipo e tamanho; reduzir; tentar de novo |
| IA do suporte não responde | Ticket fica em aberto | Aguardar; agente humano vai atender |

---

## SEÇÃO 8 — NOTIFICAÇÕES E E-MAILS

## 8.1 E-mails enviados pelo sistema

| Evento | Quem recebe | Assunto | Botão / link |
|---|---|---|---|
| Cidadão se cadastra ou pede reenvio de verificação | Cidadão | "Confirme su e-mail — Colabora Ciudadano" | "Confirmar e-mail" |
| Cidadão pede recuperação de senha | Cidadão | "Recuperación de acceso — Colabora Ciudadano" | "Restablecer contraseña" |
| Equipe do painel pede recuperação de senha | Usuário do painel | "Recuperación de acceso — Colabora" | "Restablecer contraseña" |
| Master pede troca de e-mail de cidadão | Cidadão, no novo e-mail | "Autorizá el cambio de e-mail — Colabora Ciudadano" | "Autorizar el cambio de e-mail" |
| Usuário do painel pede troca do próprio e-mail | Usuário, no novo e-mail | "Confirmación de cambio de e-mail — Colabora" | "Confirmar nuevo e-mail" |
| Convite de Admin de Ciudad | Convidado | "Invitación como Administrador de Ciudad — Colabora" | "Activar mi cuenta" |
| Convite de Admin de Secretaría Ministerial | Convidado | "Invitación como Administrador de Secretaría — Colabora" | "Activar mi cuenta" |
| Convite de Master Team | Convidado | "Invitación al equipo Master en Colabora" | "Activar mi cuenta" |
| Convite de Miembro de Ciudad | Convidado | "Invitación al equipo de la Ciudad — Colabora" | "Activar mi cuenta" |
| Convite de Miembro de Secretaría Ministerial | Convidado | "Invitación al equipo de la Secretaría — Colabora" | "Activar mi cuenta" |
| Convite de Admin de Secretaría Municipal | Convidado | "Invitación como Administrador de Secretaría Municipal — Colabora" | "Activar mi cuenta" |
| Convite de Miembro de Secretaría Municipal | Convidado | "Invitación al equipo de la Secretaría Municipal — Colabora" | "Activar mi cuenta" |
| Convite de Admin de Escritorio | Convidado | "Invitación como Administrador de Escritorio — Colabora" | "Activar mi cuenta" |
| Convite de Miembro de Escritorio | Convidado | "Invitación al equipo del Escritorio — Colabora" | "Activar mi cuenta" |
| Reenvio de convite | Mesmo destinatário | Assunto igual ao do papel original | "Activar mi cuenta" |
| Troca de e-mail de convite pendente | Novo e-mail | Assunto igual ao do papel | "Activar mi cuenta" |
| Convite de agente do Equipo Colabora | Agente | "Invitación al equipo de Soporte — Colabora" | "Activar mi cuenta" |
| Envio de ofício pela equipe do órgão | Destinatário externo informado pelo agente | Texto livre (definido pela equipe) | PDF anexado |

Validade do convite de equipe: 48 horas. Validade do link de redefinição de senha: 1 hora.

## 8.2 Push (celular) e inbox (app do cidadão)

| Evento | Push | Inbox |
|---|---|---|
| Status da solicitud muda | Sim | Sim |
| Órgão responde a solicitud | Sim | Sim |
| Equipo Colabora responde a ticket de suporte | Sim | Sim |
| Conta verificada | "Cuenta verificada" / "Tu cuenta fue verificada con éxito. ¡Bienvenido/a a Colabora!" | Sim |
| Senha alterada | "Contraseña actualizada" / "Tu contraseña fue cambiada. Si no fuiste tú, contáctanos enseguida." | Sim |
| Novo aviso / notícia / enquete / vaga / evento na cidade do cidadão | Sim | Sim |
| Aviso "Crítica" | Sim, imediato | Sim |

## 8.3 O que o cidadão recebe por canal — resumo

| Evento | E-mail | Push | Inbox |
|---|---|---|---|
| Verificação de cadastro | Sim | — | — |
| Recuperação de senha | Sim | — | — |
| Troca de e-mail autorizada pelo Master | Sim (no novo) | — | — |
| Resposta ou mudança de status da solicitud | — | Sim | Sim |
| Resposta do Equipo Colabora em ticket | — | Sim | Sim |
| Conta verificada / senha alterada | — | Sim | Sim |
| Aviso, notícia, enquete, vaga, evento | — | Sim | Sim |
| Ofício enviado a destinatário externo | Sim (PDF anexo) | — | — |

---

## SEÇÃO 9 — PLANOS E ASSINATURAS

## 9.1 Planos

Os planos comerciais são gerenciados pelo Master em Configurações → Planes. Cada plano tem Nombre e Detalle (texto livre descrevendo o que está incluído).

## 9.2 Contratos

Cada Ciudad ou Secretaría Ministerial pode ter no máximo um contrato ativo. Quem gerencia é apenas o Master.

Campos do contrato: Nombre, Tipo de órgano (Ciudad ou Secretaría Ministerial), Plan, Início, Vencimento (opcional), Valor mensal em PYG, Estado, Notas.

Estados: Borrador, Activo, Suspendido, Cerrado.

Stats no topo da tela: Total, Activos, Facturación mensual (em PYG).

## 9.3 Cancelamento e upgrade

Para cancelar um contrato: mudar o estado para Cerrado ou eliminar o contrato. A eliminação remove da interface mas pode ser revertida pela equipe Colabora.

Para fazer upgrade ou downgrade: editar o contrato e mudar o Plan ou o Valor mensal.

---

## SEÇÃO 10 — PERGUNTAS FREQUENTES (FAQ)

### Para cidadãos

**P: Como faço para criar minha conta?**
R: Pelo app móvel Colabora Ciudadano ou pelo site, opção "Criar conta" da tela "Área do Cidadão". O sistema pede e-mail, senha (mínimo 8 caracteres com letra e número), departamento, ciudad, nome, CI, data de nascimento, sexo e celular. Depois de criar, abra o e-mail de verificação e clique no link. Antes disso, o login fica bloqueado com "E-mail no verificado".

**P: Não recebi o e-mail de verificação. O que faço?**
R: Confira o endereço (sem espaços) e olhe a pasta de spam. Tente se cadastrar de novo com o mesmo e-mail — se a conta ainda não foi verificada, o sistema reenvia o link e responde "Correo de verificación reenviado.". Se ainda assim não chegar, fale com o suporte.

**P: Esqueci minha senha. Como recupero?**
R: Clique em "Esqueceu a senha?" na tela de login, informe seu e-mail e clique em "Enviar link de recuperação". Se o e-mail está cadastrado, você recebe um link válido por 1 hora. Defina a nova senha (mínimo 8 caracteres). Suas sessões serão encerradas após a redefinição.

**P: Tentei fazer login e apareceu "Credenciales inválidas". Por quê?**
R: O sistema usa essa mensagem para qualquer falha de login (proteção de segurança). Confira a digitação. Se está certo, pode ser que a conta esteja desativada — fale com o suporte.

**P: Apareceu "Demasiados intentos. Probá de nuevo en unos minutos." Quanto preciso esperar?**
R: Cerca de 5 a 10 minutos. O bloqueio é temporário e protege contra tentativas automáticas.

**P: Como troco meu e-mail?**
R: O e-mail é trocado pelo administrador do sistema (Master). Você fala com o suporte e ele troca; um link de verificação vai para o novo e-mail e a troca só vale depois que você abre o link. O link vale 1 hora.

**P: Como abro uma solicitud?**
R: Pelo app móvel Colabora Ciudadano. O site só permite acompanhar; criar é só pelo app. Você escolhe o destino (cidade ou secretaría), preenche título, descrição, categoria e prioridade, anexa fotos/PDFs (até 10 MB cada, limite de 30 envios em 24 h), confirma com a localização atual e envia. Recebe um protocolo no formato com 4 dígitos / 2 dígitos do ano (ex.: 0042/26).

**P: Por que preciso ativar o GPS para enviar?**
R: A localização atual é obrigatória para o órgão saber de onde veio o reporte. Ative GPS e dê permissão ao app nas configurações do celular.

**P: Quanto tempo até o órgão responder?**
R: Não há prazo fixo. O ciclo passa por Abierta → Verificando → En proceso → Cerrada (ou No corresponde). Você é avisado por push e na inbox do app sempre que houver mudança.

**P: Posso cancelar minha solicitud?**
R: Sim, apenas enquanto está em "Abierta". Você precisa informar um motivo (3 a 500 caracteres). Depois que o órgão muda o status, não dá mais para cancelar nem editar.

**P: O chat da solicitud não aceita mais mensagens. Por quê?**
R: Duas situações bloqueiam: (1) a solicitud está em estado final ("Esta solicitud ya está cerrada; el chat no acepta nuevas mensajes."); (2) você mandou muitas mensagens em pouco tempo ("Demasiados mensajes en poco tiempo. Esperá un momento antes de seguir.").

**P: Posso anexar fotos no chat da solicitud?**
R: Não. O chat é só texto (até 4000 caracteres por mensagem). Para mandar fotos, anexe-as na própria solicitud enquanto ela ainda está em "Abierta".

**P: Como acompanho minha solicitud pelo site?**
R: Entre no site, abra "Meus Protocolos". Lista com protocolo, assunto, status e data. No detalhe aparece a resposta do órgão quando houver, junto com as interações e os anexos.

**P: Por que o status aparece em português no site e em espanhol no app?**
R: É como o produto está hoje — não é erro. Internamente é o mesmo status; só a tradução visível muda.

**P: Não estou recebendo notificações push.**
R: (1) Confira a permissão de notificações no sistema do celular. (2) Veja em Configuración > Notificaciones do app se os toggles estão ligados. (3) Se trocou de celular muitas vezes, pode ter passado de 10 dispositivos — fale com o suporte para limpar tokens antigos.

**P: Quero excluir minha conta. Como faço?**
R: Fale com o suporte. O sistema tem o procedimento, e ele aciona para você.

**P: Como me candidato a uma vaga?**
R: Pelo módulo Empleo Público no app. Envie nome, e-mail, telefone e currículo (PDF, DOC ou DOCX, até 10 MB). Mensagem é opcional. Só dá para se candidatar enquanto a vaga está "Publicada". Sucesso: "Candidatura enviada con éxito."

### Para a equipe do órgão

**P: Como entro no painel?**
R: Na tela "Iniciar sesión", informe e-mail e senha. Se ainda não tem conta, é porque ninguém te convidou — fale com o Master ou o administrador do seu órgão.

**P: Recebi um convite mas o link diz "Token inválido o expirado".**
R: Os convites valem 48 horas. Peça ao Master ou ao administrador do seu nível para reenviar ou criar um novo. Em Estructura ou em Configurações → Equipo o convite aparece com um chip de tempo; o dropdown tem "Reenviar invitación", "Cambiar e-mail" e "Revocar invitación".

**P: Não consigo acessar Ciudadanos / Contratos / Equipo Master.**
R: Esses módulos são exclusivos do Master strict. Master Team vê quase tudo, mas não mexe em Contratos. Se você é Master e não está vendo, peça à equipe Colabora para verificar seu cadastro.

**P: Onde gerencio as categorias de solicitudes?**
R: Em Configurações → Categorías (somente Master). Você pode criar, editar e desativar. As solicitudes existentes continuam coerentes mesmo se a categoria for renomeada depois.

**P: Sou Admin de Ciudad. Como atribuo responsável a uma solicitud?**
R: Na lista, clique no ícone do avião verde da linha ("Encaminar a responsable"). O modal abre com as opções (Secretaría Municipal, Secretaría Ministerial, Otra Ciudad, Escritorio, etc.) conforme seu papel.

**P: Sou SM/Escritorio e quero indicar que essa solicitud não é comigo.**
R: Clique no ícone X vermelho da linha ("Marcar como no corresponde — enviar a Revisión"). Preencha o motivo obrigatório. A solicitud vai para a fila de Revisión da Ciudad ou Secretaría Ministerial, que redige para o destino correto.

**P: Como respondo ao cidadão?**
R: No detalhe da solicitud, no card "Documento", você gera ou regenera o PDF do ofício e depois clica "Enviar". O dialog pede destinatários, assunto e conteúdo do e-mail; você pode escolher mudar o estado da solicitud junto. A resposta também aparece para o cidadão em "Meus Protocolos".

**P: Por que o Master não consegue enviar documento?**
R: Por design. O Master apenas visualiza solicitudes — a operação é da equipe do órgão. Se tentar, aparece "El master solo visualiza solicitudes."

**P: Como crio uma enquete?**
R: No Aplicativo → "Encuestas" → "Nueva encuesta". Preencha título, descrição (opcional), opções (mínimo 2), imagem (opcional), datas de início e cierre, estado. O cierre deve ser pelo menos 15 minutos depois do início. Se você for um papel de cidade, a enquete fica visível só para essa cidade; sendo Master, é nacional.

**P: Como crio um aviso?**
R: No Aplicativo → "Avisos" → "Nuevo aviso". Preencha título (3-200 caracteres), descrição (mínimo 3), prioridade (Baja, Media, Alta, Crítica). Pode programar ou publicar já. Avisos "Crítica" disparam push imediato. Sem "Expira en", o aviso não expira automaticamente.

**P: Como crio uma notícia?**
R: No Aplicativo → "Noticias" → "Nueva noticia". Preencha título, descrição-resumo, tipo, imagem de capa, blocos de conteúdo (texto, imagem ou vídeo), arquivos para download. "Programar publicación" é opcional. Use "Salvar como rascunho" ou "Postar".

**P: Como convido um novo agente para o Equipo Colabora?**
R: Apenas o Master strict pode. Em Configurações → "Equipo de Soporte" → "Invitar agente". Informe correo e nome. O agente recebe e-mail com link, define a própria senha e entra na bandeja.

**P: O agente esqueceu a senha. Como recupero?**
R: Não há recuperação automática para agentes do Equipo Colabora. O Master precisa enviar uma nova invitación para o mesmo e-mail.

**P: Como exporto a lista de cidadãos?**
R: Em Ciudadanos (só Master), clique em "Exportar (CSV)". O arquivo é gerado a partir da listagem visível — ajuste a paginação se quiser exportar todos.

**P: Como exporto solicitudes para CSV?**
R: Em Informes, aplique os filtros desejados e clique em "Exportar CSV". Toast: "Exportación lista — descarga iniciada".

**P: Como vejo se um agente respondeu meu ticket?**
R: Pelo ícone flutuante "Headset" no canto do painel. Abre a janela do suporte com seus tickets. Quando o agente responde, você recebe aviso no badge do widget.

---

## SEÇÃO 11 — GLOSSÁRIO

| Termo | Definição |
|---|---|
| Colabora PY | Nome do projeto |
| Gerencia Mi Gabinete | Como o painel se identifica para a equipe do órgão |
| Colabora Ciudadano | Como o app móvel e o site se identificam para o cidadão |
| Equipo Colabora | Time de suporte que atende tickets dos órgãos |
| Master | Dono da instância — único, tem acesso total |
| Master Team | Pares globais autorizados pelo Master |
| Ciudad / Cidade | Município (unidade que recebe solicitudes via triagem) |
| Secretaría Ministerial | Ministério nacional (recebe solicitudes via triagem) |
| Secretaría Municipal (SM) | Unidade-folha sob uma Ciudad, resolve solicitudes |
| Escritorio | Unidade-folha sob uma Secretaría Ministerial, resolve solicitudes |
| Triagem | Etapa de análise inicial feita por Ciudad ou Secretaría Ministerial |
| Resolvedor | SM ou Escritorio que trabalha a solicitud até concluí-la |
| Solicitud / Solicitação | Demanda aberta pelo cidadão ao órgão público |
| Protocolo | Identificador único da solicitud, com 4 dígitos / 2 dígitos do ano (ex.: 0042/26) |
| Estado / Status | Etapa do ciclo: Abierta → Verificando → En proceso → Cerrada / No corresponde / Cancelada |
| No corresponde | Quando o órgão entende que não é responsabilidade dele; vai para Revisión |
| Revisión | Fila das solicitudes "No corresponde", aguardando Ciudad ou Sec Ministerial redigir |
| Destino directo | Quando uma SM ou Escritorio recebe solicitudes do cidadão direto, sem passar pela triagem |
| Cidadão (Ciudadano) | Usuário final que abre solicitudes pelo app ou site |
| Convite | Link de ativação enviado por e-mail para alguém entrar na equipe; vale 48 horas |
| Ofício | Documento PDF gerado a partir da solicitud, enviado por e-mail a um destinatário externo |
| Plantilla de e-mail | Modelo do corpo do e-mail de ofício, com variáveis como {{numero}}, {{destinatario}}, etc. |
| Macro | Resposta pronta que o agente de suporte pode inserir no chat |
| CSAT | Pesquisa de satisfação ao final do ticket: nota de 1 a 5 + comentário opcional |
| Push | Notificação enviada para o celular |
| Inbox | Bandeja de notificações dentro do app/site do cidadão |
| Departamento / Distrito | Divisões administrativas do Paraguai usadas no cadastro |
| CI | Cédula de Identidad paraguaia (5 a 8 dígitos) |
| PYG | Guaraní paraguaio, moeda dos contratos |

---

## SEÇÃO 12 — MENSAGENS DE ERRO — REFERÊNCIA COMPLETA

## 12.1 Cadastro e validação de cidadão

| Mensagem | Causa | Como resolver |
|---|---|---|
| "E-mail ya registrado" | E-mail já cadastrado e verificado | Recuperar senha ou usar outro e-mail |
| "Correo de verificación reenviado." | E-mail já cadastrado mas não verificado | Conferir caixa de entrada e spam |
| "No hay servicio público activo para tu distrito todavía." | Distrito sem atendimento habilitado | Aguardar |
| "As senhas não coincidem" | Confirmação diferente da senha | Repetir |
| "Nombre completo debe tener al menos 3 caracteres" | Nome curto | Aumentar |
| "Género inválido. Valores: masculino, femenino, otro, prefiero_no_decir" | Valor inválido | Escolher opção válida |
| "Teléfono debe tener 10 dígitos" | Tamanho errado | Usar exatamente 10 dígitos |
| "Teléfono debe comenzar con 0" | Sem 0 inicial | Adicionar 0 (ex.: 0987654321) |
| "La contraseña debe tener al menos 8 caracteres" | Senha curta | Aumentar |
| "La contraseña debe tener al menos una letra" | Senha sem letra | Incluir letra |
| "La contraseña debe tener al menos un número" | Senha sem dígito | Incluir número |
| "La contraseña no puede tener más de 128 caracteres" | Senha exagerada | Reduzir |
| "CI debe tener al menos 5 dígitos" | CI curta | Conferir |
| "CI debe tener hasta 8 dígitos" | CI longa | Conferir |
| "CI ya registrada" | CI duplicada | Verificar se já tem conta |
| "Este correo ya está en uso" | E-mail duplicado em convite | Idem |

## 12.2 Login

| Mensagem | Causa | Como resolver |
|---|---|---|
| "Credenciales inválidas" | E-mail/senha errados | Conferir digitação; recuperar senha se necessário |
| "Demasiados intentos. Probá de nuevo en unos minutos." | Muitas falhas seguidas | Esperar 5 a 10 minutos |
| "Correo no verificado" | E-mail não verificado | Abrir o link de verificação |
| "Cuenta inactiva" | Conta desativada | Falar com o suporte |
| "Cuenta desactivada" | Conta desativada (mensagem do app) | Falar com o suporte |

## 12.3 Recuperação de senha

| Mensagem | Causa |
|---|---|
| "Si el correo estuviera registrado, enviaremos un enlace para restablecer la contraseña." | Resposta neutra do app |
| "Si el correo está registrado, se enviará un enlace para restablecer la contraseña." | Resposta neutra do painel |
| "Token inválido o expirado" | Link já usado ou vencido |
| "Usuario no encontrado" | Token válido mas usuário não existe mais |
| "Contraseña restablecida con éxito" | Sucesso (app) |
| "Contraseña restablecida correctamente" | Sucesso (painel) |
| "Contraseña cambiada. Vuelve a iniciar sesión." | App cidadão após cambio logado |

## 12.4 Verificação de e-mail

| Mensagem | Significado |
|---|---|
| "E-mail verificado con éxito" | Sucesso |
| "E-mail ya verificado" | Conta já estava verificada |
| "Token inválido o expirado" | Link inválido ou vencido |
| "Usuario no encontrado" | Conta removida |
| "Si el correo estuviera registrado y pendiente de verificación, enviaremos un nuevo enlace." | Resposta neutra do reenvio |

## 12.5 Troca de e-mail de cidadão

| Mensagem | Significado |
|---|---|
| "Ciudadano no encontrado" | Cidadão não existe |
| "El nuevo e-mail es igual al actual." | Sem mudança |
| "Ya existe un ciudadano con ese e-mail." | E-mail já está em uso |
| "Se envió un correo de verificación al nuevo e-mail." | Sucesso ao iniciar a troca |
| "Token requerido." | Falta o token na URL |
| "El enlace es inválido o ya expiró." | Link inválido ou vencido |
| "El cambio de e-mail fue confirmado." | Sucesso ao confirmar |

## 12.6 Solicitudes

| Mensagem | Significado |
|---|---|
| "Solicitud no encontrada" | Solicitud inexistente |
| "Adjunto inválido (verifique que el upload pertenece a esta cuenta)." | Tentativa de anexar arquivo de outra conta |
| "Solo se pueden editar solicitudes en estado 'em_aberto'." | Tentativa de editar solicitud não inicial |
| "Solo se pueden cancelar solicitudes en estado 'em_aberto'." | Tentativa de cancelar fora do estado inicial |
| "Adjunto ya está agregado." | Anexo duplicado |
| "Adjunto no encontrado." | Anexo inexistente |
| "Envíe al menos un campo para actualizar." | PUT vazio |
| "Campo no puede ser vacío." | Campo obrigatório em branco |
| "Latitude y longitude deben ser informadas juntas o ambas vacías." | Coordenadas incompletas |
| "Prioridad inválida. Valores: alta, baixa, media, urgente" | Valor errado |
| "Secretaría municipal destino no encontrada" | Destino inválido |
| "Esta secretaría municipal no recibe solicitudes directas." | SM não habilitada para destino direto |
| "Escritorio destino no encontrado" | Destino inválido |
| "Este escritorio no recibe solicitudes directas." | Escritório não habilitado |
| "Ciudad destino no encontrada" | Destino inválido |
| "Secretaría destino no encontrada" | Destino inválido |
| "El master solo visualiza solicitudes." | Master tentou enviar documento |

## 12.7 Chat da solicitud

| Mensagem | Significado |
|---|---|
| "Esta solicitud ya está cerrada; el chat no acepta nuevas mensajes." | Solicitud em estado final |
| "Demasiados mensajes en poco tiempo. Esperá un momento antes de seguir." | Flood do chat |

## 12.8 Anexos

| Mensagem | Significado |
|---|---|
| "Tipo de archivo no permitido. Aceptados: application/pdf, image/heic, image/heif, image/jpeg, image/png, image/webp" | Tipo errado para anexo de solicitud |
| "Límite diario de adjuntos alcanzado. Probá mañana." | Estourou cota de 30 anexos em 24 horas |
| "Formato de imagen no soportado" | Tipo errado para foto de perfil |
| "Foto no encontrada" | Foto não existe |
| "Foto original no encontrada" | Foto original não existe |

## 12.9 Push

| Mensagem | Significado |
|---|---|
| "Este dispositivo está vinculado a otra cuenta. Cerrá sesión en el otro app primero." | Mesmo celular em outra conta |
| "Límite de 10 dispositivos alcanzado." | Excedeu 10 tokens ativos |
| "Token no encontrado." | Token inválido |

## 12.10 Reações em notícias

| Mensagem | Significado |
|---|---|
| "Noticia no encontrada o no disponible" | Notícia removida ou inativa |
| "Esta noticia no acepta reacciones" | Reações desativadas |
| "Emoji no permitido. Usá uno de: 👍, ❤️, 😂, 🤔, 😲, 😢, 👎, 👏, 🙏, 🔥, 🎉, 💯, 😍, 😎, 😡, 😭, 😱, 💪, ✅, ❌, 💚, 💙, ⭐, 💡" | Emoji fora da lista permitida (24 opções) |

## 12.11 Enquetes

| Mensagem | Significado |
|---|---|
| "Encuesta no encontrada" | Enquete não existe |
| "Esta encuesta no está activa para votación" | Enquete em rascunho ou cerrada |
| "La votación todavía no empezó" | Antes do início |
| "La votación ya finalizó" | Depois do cierre |
| "Opción de voto inválida" | Opção fora das listadas |
| "Ya votaste en esta encuesta" | Cidadão já votou |

## 12.12 Empleo Público

| Mensagem | Significado |
|---|---|
| "Vacante no encontrada" | Vaga inexistente |
| "Vacante cerrada — no se aceptan candidaturas" | Vaga não está mais "Publicada" |
| "El currículum debe ser PDF, DOC o DOCX" | Tipo de arquivo errado |
| "Candidatura enviada con éxito." | Sucesso |

## 12.13 Localidades

| Mensagem | Significado |
|---|---|
| "Departamento no encontrado" | Departamento inválido |
| "Código postal no encontrado" | CP inválido |
| "Información de la ciudad no encontrada" | Cidade sem cadastro de informações |
| "Local no encontrado" | Lugar inexistente na guia |
| "Evento no encontrado" | Evento inexistente |

## 12.14 Suporte (cidadão)

| Mensagem | Significado |
|---|---|
| "Ticket no encontrado" | Ticket inexistente |
| "Solo se puede evaluar un ticket cuando ya fue resuelto o cerrado." | CSAT antes da hora |
| "Ya enviaste tu evaluación" | CSAT duplicado |
| "Este ticket ya fue cerrado y no acepta nuevos mensajes." | Ticket terminal |
| "El mensaje no puede estar vacío." | Conteúdo vazio |
| "Prioridad inválida: [valor]" | Valor errado |

## 12.15 Suporte (agente Colabora)

| Mensagem | Significado |
|---|---|
| "Status requerido" | Sem status |
| "Status inválido: [valor]" | Valor errado |
| "Prioridad inválida: [valor]" | Valor errado |
| "Ticket no encontrado" | Inexistente |
| "Solo agentes de soporte pueden tomar tickets para sí" | Tentativa indevida |
| "Agente no encontrado" | Agente inexistente |
| "Ticket ya está resuelto o cerrado" | Tentativa redundante |
| "Solo se puede evaluar tickets resueltos" | CSAT antes da hora |
| "Ticket ya está cerrado" | Tentativa redundante |
| "Ticket no puede ser reabierto en este estado" | Reabertura inválida |
| "Mensaje vacío" | Envio sem conteúdo |
| "No es posible enviar mensajes en ticket cerrado/cancelado" | Ticket terminal |
| "Mensaje no encontrado" | Inexistente |
| "Tipo de archivo no permitido: [tipo]" | Anexo inválido |
| "Tipo de archivo no permitido (extensión bloqueada)" | Extensão bloqueada |
| "Tipo de archivo no aceptado: [tipo]" | Tipo bloqueado |
| "No tenés acceso a este ticket" | Sem permissão |
| "Límite de 10 adjuntos por mensaje. Ya hay [N]." | Excedeu anexos |
| "Adjunto no encontrado" | Inexistente |
| "Archivo no encontrado en storage" | Falha no storage |
| "Credenciales inválidas" | Login do agente errado |
| "Demasiados intentos. Probá de nuevo en unos minutos." | Bloqueio temporário |
| "El nombre no puede estar vacío." | Nome em branco |
| "Invitación inválida o expirada" | Convite inválido |
| "Invitación no encontrada" | Inexistente |
| "Invitación ya fue aceptada" | Convite já consumido |
| "Invitación ya fue revocada" | Convite já revogado |
| "Invitación fue revocada. Cree una nueva." | Sem como reusar |

## 12.16 Cadastro de admin do painel

| Mensagem | Significado |
|---|---|
| "Token de invitación inválido o expirado" | Link inválido ou vencido |
| "Token inválido o expirado" | Idem |
| "Invitación sin tipo de vínculo definido" | Convite incompleto |
| "CI ya registrada" | CI duplicada |
| "Este correo ya está en uso" | E-mail duplicado |
| "Datos duplicados" | Genérico |
| "Sesión cerrada" | Logout efetuado |
| "Contraseña actual incorrecta" | Erro ao trocar senha |
| "Contraseña restablecida correctamente" | Sucesso |
| "Si el correo está registrado, se enviará un enlace para restablecer la contraseña." | Resposta neutra |

## 12.17 Estructura e convites

| Mensagem | Significado |
|---|---|
| "Invitación no encontrada" | Convite inexistente |
| "Invitación ya fue aceptada" | Já consumido |
| "Invitación ya fue revocada" | Já cancelado |
| "Invitación fue revocada. Cree una nueva." | Precisa criar novo |

## 12.18 Limites visíveis ao usuário

| Item | Valor |
|---|---|
| Senha | mínimo 8 caracteres, com letra e número |
| Telefone | 10 dígitos começando com 0 |
| CI | 5 a 8 dígitos |
| Foto de perfil (cidadão, app) | até 10 MB; JPEG, PNG, WebP, HEIC, HEIF, GIF ou BMP |
| Foto de perfil (painel e agente Colabora) | até 5 MB |
| Anexo de solicitud | até 10 MB cada; PDF, JPG, PNG, WebP, HEIC ou HEIF |
| Anexos por dia (solicitud) | 30 envios em 24 horas por cidadão |
| Anexos em ticket de suporte (agente) | até 10 anexos por mensagem, 20 MB cada |
| Currículo (Empleo Público) | PDF, DOC ou DOCX, até 10 MB |
| Texto do chat da solicitud | até 4000 caracteres por mensagem |
| Rate-limit do chat da solicitud | 30 mensagens por minuto |
| Dispositivos para push por cidadão | 10 ativos |
| Validade do link de redefinição de senha | 1 hora |
| Validade do link de verificação de e-mail | 24 horas |
| Validade do link de troca de e-mail | 1 hora |
| Validade do convite de equipe | 48 horas |
| Emojis aceitos em reação a notícia | 24 (fixos) |

---

Fim do documento. Este RAG cobre o uso do produto Colabora PY. Qualquer dúvida fora deste material — especialmente questões 
legais (termos, privacidade), dados pessoais sensíveis, problemas que envolvam alteração no funcionamento do sistema ou suspeita de erro grave — deve ser 
encaminhada a um agente humano do Equipo Colabora.
