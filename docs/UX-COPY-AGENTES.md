# UX Copy — Agentes de IA no ditado

Textos de interface para o fluxo descrito em [`PLANO-IA-AGENTES.md`](PLANO-IA-AGENTES.md). Idioma: português do Brasil.

**Voz:** próxima, direta e calma. Fala como uma pessoa que ajuda, não como um sistema. Trata por "você". Sem exclamações em erros; no máximo uma em sucesso.

**Contexto do usuário:** está no meio de uma conversa em outro app (WhatsApp, e-mail), quer mandar a mensagem e seguir. Pouca paciência, atenção dividida. No teclado, cada palavra custa espaço.

---

## 1. Glossário (use sempre os mesmos termos)

| Use | Não use | Observação |
|---|---|---|
| Boost | melhorar, IA, mágica, turbo | Nome da ação de reescrever. Mantido em inglês: curto, cabe na barra. Como verbo, use "tocar em Boost" (não "boostar") |
| agente | assistente, modo, persona, bot | Define *como* o Boost reescreve |
| ditado / ditar | gravação, áudio, voz (como substantivo do conteúdo) | "Gravação" só em contexto técnico de permissão |
| texto original | transcrição bruta, versão crua | "Transcrição" é jargão para o usuário |
| reescrever | processar, gerar, melhorar com IA | "Refinar" só no estado de carregamento |
| instruções | system prompt, prompt | "System prompt" aparece só como subtítulo no editor avançado |
| provedor | API, serviço, backend | |
| chave de API | token, secret, credencial | |
| modelo | engine, LLM | |
| Acesso Total | acesso completo, full access | Termo oficial do iOS em pt-BR |
| Apple Intelligence | IA da Apple, Siri | Nome próprio, não traduzir |

---

## 2. Barra do teclado

Espaço máximo do chip do agente: **~18 caracteres** (o nome trunca com "…").

| Elemento | Texto | Limite |
|---|---|---|
| Ícone de menu (acessibilidade) | `Trocar agente` | VoiceOver |
| Chip do agente | `{Nome do agente}` + ▾ | 18 car. |
| Botão Boost | `Boost` (ícone ⚡) | 6 car. |
| Botão Boost (acessibilidade) | `Reescrever com {Nome do agente}` | VoiceOver |
| Dica do Boost (acessibilidade) | `Toque duas vezes e segure para escolher outro agente` | VoiceOver |
| Botão desfazer | `Original` (ícone ↺) | 8 car. |
| Botão desfazer (acessibilidade) | `Voltar ao texto ditado` | VoiceOver |
| Microfone (acessibilidade, parado) | `Ditar` | VoiceOver |
| Microfone (acessibilidade, gravando) | `Parar e inserir texto` | VoiceOver |

### Estados do botão ao lado do microfone

O ditado não tem estado novo: ele continua como hoje. Os estados abaixo são só do botão Boost.

| Estado | Texto no botão | Quando |
|---|---|---|
| Oculto | — | Antes de ditar, ou depois que a pessoa digita/move o cursor |
| Pronto | `⚡ Boost` | Logo depois que o ditado é inserido |
| Reescrevendo | `Reescrevendo…` (com spinner) | Depois do toque, até o texto chegar |
| Feito | `↺ Original` | Depois de um Boost bem-sucedido |
| Desativado | `⚡ Boost` em cinza | Agente sem configuração ou sem internet (agente de nuvem) |

**Por quê:** o rótulo "Boost" fica curto e fixo; o *que* ele faz aparece no chip do agente ao lado, então a pessoa lê "Enriquecer escrita · ⚡ Boost" e entende. "Reescrevendo…" diz exatamente o que acontece com o texto; "Processando" é vago. Depois do Boost, o mesmo lugar vira "Original", então desfazer está onde o dedo já está.

---

## 3. Dropdown de agentes (sobre as teclas)

| Elemento | Texto |
|---|---|
| Título | `Agente` |
| Linha de agente | `{Nome}` / legenda: `{Provedor}` → `Apple · no iPhone`, `Claude`, `OpenAI`, `OpenRouter` |
| Agente indisponível — legenda | `Configurar no app` |
| Título quando aberto segurando o Boost | `Boost com…` (ao escolher, já reescreve) |
| Último item | `Gerenciar agentes` |
| Fechar (acessibilidade) | `Fechar lista de agentes` |

**Feedback ao trocar** (chip, 1,5 s): `Agente: {Nome}`

---

## 4. Erros e avisos no teclado

Estrutura: o que aconteceu + o que fazer. No teclado, uma linha só, no lugar do chip do agente por ~3 s; o "porquê" vai para o app. Em toda falha do Boost, o texto do campo **não muda** e o Boost continua disponível para tentar de novo.

| Situação | Texto no teclado | Ação |
|---|---|---|
| Sem Acesso Total (toque no Boost) | `Ative o Acesso Total para usar o Boost` | `Como ativar` |
| Sem internet (agente na nuvem) | `Sem internet. Seu texto não mudou.` | — |
| Timeout | `Demorou demais. Seu texto não mudou.` | `Tentar de novo` |
| Chave inválida | `Chave do {Provedor} recusada. Seu texto não mudou.` | `Corrigir` |
| Agente sem configuração | `Configure {Nome do agente} no app` | `Abrir app` |
| Apple Intelligence indisponível | `Apple Intelligence está desligada` | `Como ativar` |
| Texto editado durante o Boost | `Você editou o texto, então o Boost não foi aplicado` | — |
| Microfone sem permissão | `Permita o microfone para ditar` | `Abrir Ajustes` |
| Não entendeu o áudio | `Não consegui ouvir. Tente de novo mais perto do microfone.` | — |

**Por quê:** "Seu texto não mudou" responde a única dúvida que importa depois de uma falha: *perdi alguma coisa?* Nenhum código de erro aparece para o usuário.

---

## 5. App — lista de agentes

| Elemento | Texto |
|---|---|
| Título da tela | `Agentes` |
| Subtítulo | `Escolha como o Boost reescreve o que você dita.` |
| Botão principal | `Novo agente` |
| Selo no agente ativo | `Em uso` |
| Selo nos presets | `Pronto` → **usar** `Do Entrelinha` (evita confundir com o estado "Pronto") |
| Ação de deslizar | `Duplicar` · `Ocultar` (presets) · `Excluir` (seus agentes) |
| Rodapé | `Restaurar agentes do Entrelinha` |

**Estado vazio** (todos os agentes ocultos/excluídos):
> **Nenhum agente por aqui**
> O ditado continua funcionando normalmente. Crie um agente para o Boost limpar, enriquecer ou mudar o tom do seu texto.
> `Criar agente` · `Restaurar agentes do Entrelinha`

### Descrições dos agentes do Entrelinha (1 linha, ≤ 50 car.)

| Agente | Descrição |
|---|---|
| Transcrição limpa | Pontuação certa, sem "é…" e repetições. |
| Enriquecer escrita | Mais fluidez e vocabulário, com a sua voz. |
| Profissional | Tom formal e direto para o trabalho. |
| Mensagem rápida | Curto e natural para conversas. |
| E-mail | Saudação, parágrafos e despedida. |
| Tópicos | Vira uma lista objetiva. |
| Inglês | Traduz para um inglês natural. |

---

## 6. App — editor de agente

| Campo | Rótulo | Placeholder / ajuda |
|---|---|---|
| Nome | `Nome` | `Ex.: Meu estilo` |
| Ícone | `Ícone` | — |
| Instruções | `Instruções` (subtítulo pequeno: `system prompt`) | `Descreva como o texto deve ficar. Ex.: "Reescreva de forma calorosa e breve, sem emojis, mantendo as informações."` |
| Ajuda abaixo das instruções | — | `Escreva como se explicasse para uma pessoa. O trecho em que você tocar Boost é enviado junto, você não precisa incluí-lo.` |
| Provedor | `Modelo de IA` | opções: `Apple Intelligence (no iPhone)`, `Claude (Anthropic)`, `OpenAI`, `OpenRouter`, `Nenhum` |
| Modelo | `Modelo` | `Ex.: claude-haiku-5-5` |
| Temperatura | `Criatividade` | extremos: `Fiel` ↔ `Criativo` |
| Botão salvar | `Salvar agente` | |
| Contador | `{n}/2.000` | |

**Por quê:** "Instruções" é compreensível para qualquer pessoa; "system prompt" fica como subtítulo para quem já conhece. "Criatividade" com polos "Fiel/Criativo" explica a temperatura sem número.

### Testar agente (playground)

| Elemento | Texto |
|---|---|
| Título da seção | `Testar` |
| Campo | placeholder: `Digite ou dite uma frase de exemplo` |
| Botão | `Testar agente` |
| Colunas | `Original` · `Resultado` |
| Rodapé | `Respondeu em {1,2} s com {Modelo}` |
| Erro | `O teste não funcionou: {motivo curto}. Confira o provedor e tente de novo.` |

### Confirmações

**Excluir agente**
> **Excluir "{Nome}"?**
> As instruções deste agente serão apagadas. Não dá para desfazer.
> `Excluir agente` · `Manter`

**Sair sem salvar**
> **Descartar as alterações?**
> As mudanças em "{Nome}" não foram salvas.
> `Descartar` · `Continuar editando`

**Restaurar agentes do Entrelinha**
> **Restaurar os agentes do Entrelinha?**
> Os agentes originais voltam a aparecer. Os seus agentes não são alterados.
> `Restaurar` · `Agora não`

---

## 7. App — provedores

| Elemento | Texto |
|---|---|
| Título | `Provedores` |
| Subtítulo | `Use o Apple Intelligence no iPhone ou conecte um modelo na nuvem com a sua chave.` |
| Card Apple — status ok | `Ativo · roda no iPhone, sem enviar seu texto` |
| Card Apple — indisponível | `Indisponível neste iPhone. Requer iOS 26 e Apple Intelligence ativado em Ajustes.` |
| Campo de chave | `Chave de API` / placeholder `Cole sua chave aqui` |
| Ajuda da chave | `Fica guardada no Keychain deste iPhone.` |
| Botão | `Testar conexão` |
| Sucesso | `Conectado · respondeu em {0,9} s` |
| Erro 401 | `Chave recusada. Confira se copiou a chave inteira.` |
| Erro modelo | `Modelo "{modelo}" não encontrado. Confira o nome no site do provedor.` |
| Erro rede | `Sem conexão com {Provedor}. Verifique a internet e tente de novo.` |
| Remover chave | `Remover chave` |

**Aviso de privacidade** (aparece ao salvar a primeira chave de nuvem — sheet):
> **Seu texto vai para {Provedor}**
> Quando você toca em Boost com um agente que usa {Provedor}, aquele trecho é enviado para os servidores dele, usando a sua chave. Ditar sem tocar em Boost não envia nada. Com o Apple Intelligence, o texto não sai do iPhone.
> `Entendi` · `Ler a política do {Provedor}`

---

## 8. Onboarding (uma ideia por tela)

| Tela | Título | Texto | Botão |
|---|---|---|---|
| 1 | `Fale do seu jeito` | `Dite como fala. O Entrelinha entrega o texto pronto para enviar.` | `Continuar` |
| 2 | `Toque em Boost para reescrever` | `Depois de ditar, o Boost deixa o texto limpo, profissional, em tópicos ou do jeito que você descrever.` | `Continuar` |
| 3 | `Ative o teclado` | `Ajustes › Geral › Teclado › Teclados › Adicionar › Entrelinha.` | `Abrir Ajustes` |
| 4 | `Permita o Acesso Total` | `Assim o teclado recebe o texto do app. O Entrelinha não lê o que você digita.` | `Abrir Ajustes` |
| 5 | `Tudo pronto` | `Abra qualquer conversa, toque no microfone e fale. Quer ajustar o texto? Toque em ⚡ Boost.` | `Começar` |

---

## Notas de localização

- **Expansão de texto:** do pt-BR para o inglês o texto encolhe ~10–15%; para alemão e francês cresce até 30%. O chip da barra (18 car.) precisa truncar com "…" em qualquer idioma.
- **"Seu texto não mudou"** em inglês: "Your text wasn't changed." Evite "Nothing happened", que soa como falha do app.
- **"Boost"** não é traduzido em nenhum idioma (é o nome da ação, como uma marca).
- **"é…", "tipo", "né"** nas descrições são exemplos de vícios do português. Em outros idiomas, troque pelos equivalentes locais ("um", "like" no inglês), não traduza literalmente.
- **"Acesso Total"** e o caminho de Ajustes devem seguir os termos oficiais do iOS em cada idioma.
- **Separador decimal:** "1,2 s" em pt-BR; "1.2 s" em inglês. Formatar com `MeasurementFormatter`, nunca com string fixa.
- **Nomes de provedor e de modelo** (Apple Intelligence, Claude, OpenAI, OpenRouter, `claude-haiku-5-5`) não são traduzidos.
