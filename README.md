# Entrelinha

Teclado de ditado para iOS com **Boost por IA**: você dita normalmente e o texto entra no campo como sempre. Se quiser, toca em **Boost** e um agente reescreve aquele trecho do jeito que você configurou (limpar, enriquecer, formalizar, resumir em tópicos, traduzir…), em qualquer app.

> Status: o Boost está implementado no código do app, no repositório [`cesariniandre/mykey`](https://github.com/cesariniandre/mykey) (`Shared/BoostSettings.swift`, `Shared/BoostEngine.swift`, `Keyboard/Hex/FullKeyboard/BoostMenuView.swift`, `App/BoostAgentsView.swift`). O plano original está em [`docs/PLANO-IA-AGENTES.md`](docs/PLANO-IA-AGENTES.md).
>
> Diferenças da implementação em relação ao plano: a configuração fica no **Keychain compartilhado** (a conta de desenvolvimento gratuita não tem App Group); o ditado já roda dentro do teclado, então o Boost roda no teclado também; na barra, o Boost aparece ao lado de **Desfazer** e do chip do agente, e escolher um agente no menu já reescreve com ele.

---

## Como funciona

```
🎙 Ditar  →  Texto transcrito inserido (igual a hoje)  →  [⚡ Boost] aparece na barra
                                                              │ toque
                                                              ▼
                                   Agente ativo (system prompt + modelo) reescreve o trecho
                                                              │
                                                              ▼
                                   Trecho substituído no campo  →  [↺ Original] para desfazer
```

1. No teclado do Entrelinha, toque no microfone e fale. **O ditado funciona exatamente como antes**: ao parar, o texto transcrito é inserido no campo.
2. Assim que o texto entra, o botão **⚡ Boost** aparece na barra, ao lado do microfone.
3. Tocou em **Boost**: o **agente ativo** (um *system prompt* escolhido por você) reescreve o trecho que acabou de ser ditado, e o resultado substitui esse trecho no campo.
4. Não gostou? **↺ Original** devolve o texto ditado.

A IA só roda quando você pede. Se ela falhar ou demorar, o texto fica como estava e o teclado avisa. **Nada é apagado sem resultado para colocar no lugar.**

## Agentes

Um agente é um **nome + ícone + system prompt + provedor/modelo + criatividade (temperatura)**. Você pode usar os prontos, editá-los ou criar quantos quiser.

| Agente pronto | O que faz |
|---|---|
| Transcrição limpa *(padrão)* | Pontuação, acentos e maiúsculas; remove "é…", "tipo", repetições. Não muda palavras |
| Enriquecer escrita | Melhora fluidez, vocabulário e coesão mantendo sua voz |
| Profissional | Tom formal e objetivo |
| Mensagem rápida | Curto e natural, para WhatsApp/Slack |
| E-mail | Saudação, corpo em parágrafos e despedida |
| Tópicos | Lista de bullets |
| Inglês | Traduz para inglês natural |

Exemplos de agentes personalizados: "escrever no meu estilo", "português de Portugal", "transformar em post para LinkedIn", "ata de reunião", "resposta educada para cliente".

### A barra do teclado

```
Antes de ditar:        [ ☰ Enriquecer escrita ▾ ]                          [ 🎙 ]
Depois do ditado:      [ ☰ Enriquecer escrita ▾ ]              [ ⚡ Boost ]  [ 🎙 ]
Depois do Boost:       [ ☰ Enriquecer escrita ▾ ]           [ ↺ Original ]  [ 🎙 ]
```

- **⚡ Boost** aparece logo depois de cada ditado e reescreve só aquele trecho com o agente ativo. Some quando você digita, move o cursor ou dita de novo.
- **☰ / nome do agente**: abre um dropdown sobre as teclas com todos os agentes; toque em um para ativá-lo. A escolha vale para os próximos Boosts, em qualquer app.
- **Segurar o Boost** abre o mesmo dropdown: escolha o agente e o Boost já roda com ele.

## Modelos de IA

| Provedor | Onde roda | Precisa de chave | Observação |
|---|---|---|---|
| **Apple Intelligence** *(padrão)* | No próprio iPhone | Não | Rápido, gratuito e privado. Requer iOS 26+ e aparelho compatível com Apple Intelligence ativado |
| **OpenAI** | Nuvem | Sim | Modelo configurável |
| **OpenRouter** | Nuvem | Sim | Acesso a vários modelos com uma chave; informe o *slug* do modelo |
| **Anthropic (Claude)** | Nuvem | Sim | Padrão sugerido: `claude-haiku-5-5` (rápido) |

Cada agente pode usar um provedor diferente, e o Boost usa o do agente ativo. As chaves ficam no **Keychain** do iPhone e são testadas na tela *Provedores* (botão **Testar** mostra a latência).

## Velocidade

A premissa do Entrelinha é ser rápido:

- O ditado não espera IA nenhuma: o texto entra no campo assim que é transcrito.
- O modelo é **pré-aquecido no momento em que o botão Boost aparece**, antes do toque.
- Cada Boost usa uma sessão nova e curta, sem histórico.
- Tempo-limite por provedor (Apple ~4 s, nuvem ~6 s); estourou, o texto fica como estava.
- Sem internet, agentes de nuvem mostram o Boost desativado em vez de tentar e falhar.

Metas (trecho de ~30 palavras, do toque no Boost ao texto substituído): Apple < 1 s · nuvem < 2 s.

## Privacidade

- Com **Apple Intelligence**, o texto não sai do aparelho.
- Com **provedores externos**, só o trecho em que você tocou **Boost** é enviado ao provedor escolhido, usando a sua chave. O app avisa isso ao configurar.
- Ditar sem tocar em Boost nunca envia nada para IA.
- O Entrelinha não guarda o conteúdo dos ditados em logs ou analytics.
- O teclado pede **Acesso Total** para ler os agentes e as chaves compartilhadas com o app e para chamar provedores de nuvem.

## Arquitetura (resumo)

| Componente | Responsabilidade |
|---|---|
| **App Entrelinha** | Gravação e transcrição (como hoje), configuração de agentes e provedores |
| **Teclado (Keyboard Extension)** | Barra com microfone, **Boost**, seletor de agentes e "↺ Original"; executa o Boost e substitui o trecho |
| **EntrelinhaKit** | Código compartilhado: modelo `Agent`, `AgentStore`, provedores de IA, `RewriteEngine`, Keychain, App Group |

O teclado não acessa o microfone (limitação do iOS), então o ditado continua passando pelo app. O Boost roda no próprio teclado, porque é ali que o toque acontece; se o Apple Intelligence não puder rodar dentro da extensão, o teclado delega a reescrita ao app. Detalhes em [`docs/PLANO-IA-AGENTES.md`](docs/PLANO-IA-AGENTES.md).

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
