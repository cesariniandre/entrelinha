# Entrelinha

Teclado de ditado para iOS com **agentes de IA**: você fala, o Entrelinha transcreve e, em seguida, um agente reescreve o texto do jeito que você configurou (limpar, enriquecer, formalizar, resumir em tópicos, traduzir…), antes de inseri-lo em qualquer app.

> Status: em desenvolvimento. O plano de implementação está em [`docs/PLANO-IA-AGENTES.md`](docs/PLANO-IA-AGENTES.md).

---

## Como funciona

```
🎙 Ditar  →  Transcrição  →  Agente (system prompt + modelo)  →  Texto inserido no campo
                                   │
                                   └─ erro, offline ou demora → insere a transcrição original
```

1. No teclado do Entrelinha, toque no microfone e fale.
2. Ao parar, o áudio vira texto.
3. O **agente ativo** (um *system prompt* escolhido por você) transforma esse texto.
4. O resultado é inserido onde o cursor estiver. O botão **↺ Original** desfaz a reescrita por alguns segundos.

Se a IA falhar, estiver indisponível ou passar do tempo-limite, o Entrelinha insere a transcrição original. **Um ditado nunca se perde.**

## Agentes

Um agente é um **nome + ícone + system prompt + provedor/modelo + criatividade (temperatura)**. Você pode usar os prontos, editá-los ou criar quantos quiser.

| Agente pronto | O que faz |
|---|---|
| Sem agente | Insere a transcrição exatamente como veio |
| Transcrição limpa *(padrão)* | Pontuação, acentos e maiúsculas; remove "é…", "tipo", repetições. Não muda palavras |
| Enriquecer escrita | Melhora fluidez, vocabulário e coesão mantendo sua voz |
| Profissional | Tom formal e objetivo |
| Mensagem rápida | Curto e natural, para WhatsApp/Slack |
| E-mail | Saudação, corpo em parágrafos e despedida |
| Tópicos | Lista de bullets |
| Inglês | Traduz para inglês natural |

Exemplos de agentes personalizados: "escrever no meu estilo", "português de Portugal", "transformar em post para LinkedIn", "ata de reunião", "resposta educada para cliente".

### Trocar de agente pelo teclado

Na barra do teclado, toque no ícone de menu (☰) ou no nome do agente. Um dropdown abre sobre as teclas com todos os agentes; toque em um para ativá-lo. A escolha vale para os próximos ditados, em qualquer app.

```
[ ☰ ]  [ ✨ Enriquecer escrita ▾ ]                 [ ↺ Original ]  [ 🎙 ]
```

## Modelos de IA

| Provedor | Onde roda | Precisa de chave | Observação |
|---|---|---|---|
| **Apple Intelligence** *(padrão)* | No próprio iPhone | Não | Rápido, gratuito e privado. Requer iOS 26+ e aparelho compatível com Apple Intelligence ativado |
| **OpenAI** | Nuvem | Sim | Modelo configurável |
| **OpenRouter** | Nuvem | Sim | Acesso a vários modelos com uma chave; informe o *slug* do modelo |
| **Anthropic (Claude)** | Nuvem | Sim | Padrão sugerido: `claude-haiku-5-5` (rápido) |

Cada agente pode usar um provedor diferente. As chaves ficam no **Keychain** do iPhone e são testadas na tela *Provedores* (botão **Testar** mostra a latência).

## Velocidade

A premissa do Entrelinha é ser rápido:

- O modelo é **pré-aquecido enquanto você fala**, não depois.
- Cada ditado usa uma sessão nova e curta, sem histórico.
- Tempo-limite por provedor (Apple ~4 s, nuvem ~6 s); estourou, entra o texto original.
- Sem internet, provedores de nuvem são pulados na hora.

Metas (ditado de ~30 palavras, do fim da fala ao texto inserido): sem agente < 150 ms · Apple < 1,2 s · nuvem < 2 s.

## Privacidade

- Com **Apple Intelligence**, o texto não sai do aparelho.
- Com **provedores externos**, o texto ditado é enviado ao provedor escolhido, usando a sua chave. O app avisa isso ao configurar.
- O Entrelinha não guarda o conteúdo dos ditados em logs ou analytics.
- O teclado pede **Acesso Total** apenas para compartilhar dados (agentes, resultado do ditado) com o app Entrelinha.

## Arquitetura (resumo)

| Componente | Responsabilidade |
|---|---|
| **App Entrelinha** | Gravação e transcrição, execução dos agentes (IA), configuração de agentes e provedores |
| **Teclado (Keyboard Extension)** | Barra com microfone, seletor de agentes e "↺ Original"; insere o texto final |
| **EntrelinhaKit** | Código compartilhado: modelo `Agent`, `AgentStore`, provedores de IA, `RewriteEngine`, Keychain, App Group |

O teclado não acessa o microfone nem faz chamadas de IA (limitações do iOS para extensões); ele conversa com o app por **App Group** e **Darwin notifications**. Detalhes em [`docs/PLANO-IA-AGENTES.md`](docs/PLANO-IA-AGENTES.md).

## Requisitos

- iOS 26 ou superior (para Apple Intelligence). Provedores de nuvem funcionam sem Apple Intelligence.
- Xcode 26+.

## Configuração para desenvolvimento

1. Abra o projeto no Xcode e defina seu Team nos targets **Entrelinha**, **EntrelinhaKeyboard** e **EntrelinhaKit**.
2. Ajuste o App Group (`group.com.entrelinha.shared`) e o grupo de Keychain para o seu Team ID.
3. Rode no iPhone, vá em **Ajustes › Geral › Teclado › Teclados › Adicionar** e escolha **Entrelinha**; ative **Permitir Acesso Total**.
4. No app, em *Provedores*, adicione chaves de API se quiser usar OpenAI, OpenRouter ou Anthropic.

## Documentação

- [Plano de implementação dos agentes de IA](docs/PLANO-IA-AGENTES.md)
- [Apresentação institucional](docs/README-INSTITUCIONAL.md)
- [Textos de interface (UX copy) dos agentes](docs/UX-COPY-AGENTES.md)
