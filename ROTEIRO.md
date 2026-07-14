# 🎙️ Roteiro de Apresentação — LegendaViva

> Roteiro de fala para os 13 slides de `index.html`, alinhado ao que o projeto **realmente** entrega.
> Grupo 9 — Engenharia de Software com IA · App em produção: <https://legendaviva-ifrn.duckdns.org>

**Duração alvo:** ~12 min (com demo ao vivo de 3 min). Ao final há uma versão comprimida de ~8 min.

**Legenda das marcações:**
`[AÇÃO]` = o que fazer na tela · `[DEIXA]` = gancho para o próximo slide · `⏱` = tempo sugerido do slide.

---

## Antes de começar (checklist de 2 min)

- [ ] App aberto e logado numa aba: <https://legendaviva-ifrn.duckdns.org> (palestrante em **Chrome ou Edge**).
- [ ] Microfone testado e **permissão já concedida** (evita o pop-up na hora).
- [ ] Segundo dispositivo (celular) pronto para escanear o QR — de preferência espelhado na tela.
- [ ] Volume do notebook ligado (a intro tem trilha sintetizada; `Esc` pula).
- [ ] Apresentação em tela cheia (`F`). Avançar com `→`, `espaço` ou clique.
- [ ] Plano B da demo definido (ver seção "Se a demo falhar").

---

## Slide 1 · Capa — "LegendaViva" ⏱ 0:30

> **Fala:**
> "Boa tarde. Nós somos o Grupo 9 e criamos o **LegendaViva** — acessibilidade em tempo real para eventos ao vivo.
> Em uma frase: ele **transforma fala em legenda ao vivo — e em Libras — direto no navegador**. Sem instalar nada, sem login. A audiência só abre um link.
> E não é protótipo: está **no ar, em produção**, no endereço que vocês veem na tela."

`[DEIXA]` "Mas por que isso importa? Deixa eu mostrar o problema."

---

## Slide 2 · O Problema ⏱ 1:00

> **Fala:**
> "Palestras, aulas e reuniões acontecem **no momento**. E quase nunca têm legenda ao vivo. Quem não ouve simplesmente fica de fora da conversa.
> São cerca de **10 milhões de brasileiros** com algum grau de deficiência auditiva.
> O agravante é o 'ao vivo': o conteúdo falado **se perde na hora** — não dá para 'ler depois' uma aula que já passou.
> E hoje a audiência não tem nenhuma **solução instantânea** que ela mesma abra no celular. As alternativas profissionais custam de R$ 200 a 500 por hora e exigem agendamento."

`[DEIXA]` "A nossa proposta ataca exatamente esse atrito: zero fricção."

---

## Slide 3 · A Solução ⏱ 1:15

> **Fala:**
> "A ideia é simples: **um link, e legenda ao vivo para todo mundo**.
> O **palestrante** abre o painel e fala — a transcrição aparece em tempo real, usando a Web Speech API no próprio navegador.
> A **audiência** escaneia um QR code e lê no celular. Interface mobile-first, alto contraste, com fonte ajustável de 16 a 48 pixels.
> Para quem tem **Libras como primeira língua**, dá para ligar o avatar oficial do **VLibras** ao vivo.
> Ainda temos **palavras-chave em destaque** para os termos técnicos da palestra, **tradução simultânea** entre português, inglês e espanhol, um **modo karaokê** com rolagem inteligente, e **exportação da transcrição em .txt**."

`[DEIXA]` "Falar é fácil — vamos ver funcionando de verdade."

---

## Slide 4 · Demonstração ao vivo ⏱ 3:00 🔴

> **Fala (abertura):**
> "Este é o app **em produção, com HTTPS**. Vou seguir um roteiro de uns 3 minutos."

`[AÇÃO]` **Abrir <https://legendaviva-ifrn.duckdns.org> numa aba já pronta.**

**Passo 1 — Criar sessão** (~40s)
> "Primeiro eu **crio uma sessão**: dou um título, escolho o idioma e adiciono uma ou duas palavras-chave da palestra."
`[AÇÃO]` Criar a sessão → aparece o **link e o QR code**.

**Passo 2 — Entrar como audiência** (~40s)
> "A audiência **escaneia o QR** — sem login, sem cadastro. É uma sala pública identificada por um token curto."
`[AÇÃO]` Escanear o QR no celular (espelhado) → abre a tela da audiência.

**Passo 3 — Falar** (~60s)
> "Agora eu falo, e a legenda aparece **no palco e no celular em segundos**."
`[AÇÃO]` Falar 2–3 frases claras, incluindo uma **palavra-chave** para mostrar o destaque.

**Passo 4 — Ligar Libras** (~40s)
> "E para quem precisa de Libras, eu **ativo o avatar VLibras** na tela da audiência — as frases finalizadas alimentam o avatar, que sinaliza ao vivo."
`[AÇÃO]` Ativar Libras no celular → mostrar o avatar sinalizando.

> **Fala (fechamento):**
> "Reparem: **nada foi instalado**, a audiência não fez login, e tudo rodou pela internet, em produção."

`[DEIXA]` "Por baixo disso, existe uma decisão de arquitetura que torna esse 'sem atrito' possível."

---

## Slide 5 · Arquitetura & decisões técnicas ⏱ 1:00

> **Fala:**
> "O fluxo é: **microfone + Speech-to-Text no navegador** → **frontend em React** → **FastAPI com WebSocket** → **audiência no celular**.
> Três decisões guiaram tudo:
> **Por que WebSocket** — precisamos de broadcast de baixa latência, do palco para muitos celulares ao mesmo tempo.
> **Por que STT no cliente** — reconhecer a voz no navegador significa que o **áudio nunca sai do dispositivo**: mais privado e mais barato, sem servidor de áudio.
> **Por que uma origem só** — o FastAPI serve o próprio SPA atrás do Caddy com HTTPS. Sem CORS, deploy simples no VPS."

`[DEIXA]` "Vamos abrir esse motor de tempo real em três etapas."

---

## Slide 6 · O motor do projeto ⏱ 1:00

> **Fala:**
> "A esteira tem três estágios:
> **1 · Captura** — a Web Speech API transcreve a fala do palestrante ao vivo; o áudio nunca sai do dispositivo.
> **2 · Distribuição** — o FastAPI com WebSocket transmite cada frase em **broadcast** para todos os celulares da sessão, ao mesmo tempo.
> **3 · Sinalização** — as frases finalizadas alimentam o **avatar VLibras**, que sinaliza em Libras na tela da audiência.
> E três princípios sustentam isso: **client-side por decisão** (privacidade e custo zero de áudio), **salas por token** de 6 caracteres para o QR ser instantâneo, e **degradação suave** — se um serviço externo cair, a esteira não trava; a legenda continua chegando."

`[DEIXA]` "Nessa esteira, existem três serviços de IA trabalhando juntos."

---

## Slide 7 · Integração de IA no produto ⏱ 1:00

> **Fala:**
> "São **três serviços de IA** na cadeia da legenda:
> **Fala → Texto**: reconhecimento de voz da Web Speech API, transcrevendo o palestrante em PT-BR com resultados parciais e finais.
> **Tradução neural**: o LibreTranslate traduz entre PT, EN e ES, com **cache LRU** e **fallback** — se a API falhar ou expirar, devolvemos o texto original; a legenda nunca trava.
> **Texto → Libras**: o avatar VLibras alimentado por código, com as frases finalizadas.
> A decisão de robustez aqui é importante: **cada chamada externa tem tratamento de falha** — timeout de 10 segundos e degradação suave. Em produção, priorizamos **legenda em PT + Libras**, que funcionam mesmo sem o tradutor no ar."

`[DEIXA]` "E a IA não entrou só no produto — ela entrou no nosso jeito de desenvolver."

---

## Slide 8 · Como usamos IA para desenvolver ⏱ 1:00

> **Fala:**
> "Usamos IA como **par de programação, não como piloto automático**.
> No dia a dia, o **Claude Code** ajudou na implementação, em refactors e correções de bug — sempre guiados por nós.
> Estruturamos a **documentação viva** — PRD, ADRs e as 33 BLs — com apoio de IA.
> Geramos e revisamos uma **auditoria técnica do MVP**: viabilidade, acessibilidade WCAG e riscos.
> O **deploy foi assistido**: Dockerfile, Compose e Caddy montados com IA e validados de fora.
> Fizemos até **engenharia reversa da API interna do VLibras** para alimentar o avatar por código.
> Mas o ponto central: **sempre com revisão humana** — cada mudança passou por branch, PR e teste manual antes do merge."

`[DEIXA]` "E qualidade a gente não afirma — a gente verifica."

---

## Slide 9 · Testes & qualidade ⏱ 0:45

> **Fala:**
> "Qualidade verificada em cada camada, automatizada no CI:
> **Testes unitários** cobrem a lógica de negócio core — o ciclo de vida das sessões, o serviço de tradução (com cache e o **fallback** que garante a degradação suave) e o **broadcast em tempo real**.
> **Testes E2E com Playwright** cobrem os fluxos do usuário, incluindo o pipeline real-time: o palestrante fala e a legenda chega à audiência **via WebSocket**.
> Tudo roda no **CI do GitHub Actions** a cada push — lint e build do frontend, e lint e testes do backend.
> Sendo transparentes: o próximo passo declarado são testes unitários de frontend."

`[DEIXA]` "E nada disso vale se não estiver acessível de verdade, no ar."

---

## Slide 10 · Publicado & acessível ⏱ 0:45

> **Fala:**
> "Está **no ar, com HTTPS, para qualquer navegador**.
> Roda num **VPS com Docker Compose atrás do Caddy**, com certificado Let's Encrypt.
> Segue **WCAG nível AA**: alto contraste, fonte ajustável e área de toque de 48 por 48 pixels.
> É **mobile-first** — a audiência abre em qualquer navegador moderno.
> E fica **24 por 7** num domínio público, sem depender da máquina do palestrante. O áudio é processado em memória e **nunca é armazenado**."

`[DEIXA]` "Por trás disso, uma stack madura e alguns números que resumem o esforço."

---

## Slide 11 · Domínio do código (Stack & Números) ⏱ 0:45

> **Fala:**
> "Em números: **33 Business Logics** documentadas, **7 ADRs** de decisão de arquitetura, **5 sprints** concluídas e **4 personas** no PRD.
> A stack: no frontend, **React 19 com TypeScript, Vite, Material UI e Zustand**. No backend, **FastAPI, WebSockets, Alembic e PostgreSQL**. Na cadeia de IA, **Web Speech API, LibreTranslate e VLibras**. E na infra, **Docker Compose, Caddy com HTTPS e GitHub Actions**.
> Cada escolha tem um porquê registrado nos ADRs."

`[DEIXA]` "Para fechar, o motivo de tudo isso."

---

## Slide 12 · Obrigado ⏱ 0:30

> **Fala:**
> "**Legenda para todo mundo.**
> Acessibilidade não é um detalhe; é o direito de existir no mesmo mundo que todos os outros.
> O LegendaViva está **no ar agora** — sem instalar, sem login, em qualquer navegador. Apontem a câmera para o QR e abram no celular de vocês.
> Obrigado! Ficamos abertos a perguntas."

`[AÇÃO]` Deixar o slide com o QR visível durante o Q&A.

---

## Se a demo falhar (plano B)

- **Mic não capta / Speech API não inicia:** troque para Chrome/Edge; confirme a permissão do microfone no cadeado da barra de endereço. Se persistir, **narre sobre um print/gravação** da tela funcionando e siga.
- **QR não escaneia:** digite o link da sessão manualmente no celular, ou mostre a tela da audiência já aberta numa segunda aba.
- **Tradutor (LibreTranslate) fora do ar:** é o momento perfeito para mostrar a **degradação suave** — "reparem que a legenda em PT e a Libras continuam funcionando; o tradutor tem fallback". Vira ponto a favor.
- **Sem internet / produção fora do ar:** rode o app **completo** localmente com `docker compose up --build` (sobe UI + API + PostgreSQL + tradução em `http://localhost:8000`, com um comando só) e apresente em `localhost`. É um plano B sólido — o mesmo app, sem depender da rede.

## Perguntas prováveis (prep de Q&A)

- **"Por que não transcrever no servidor com Whisper?"** — Privacidade (áudio não sai do dispositivo), latência menor e custo zero de servidor de áudio. Está no ADR de estratégia de tempo real.
- **"E se forem 200 pessoas na sala?"** — O broadcast é por WebSocket a partir de uma sessão; o MVP mira dezenas de participantes por sessão (ver limite em `.env`). Escala horizontal é evolução futura (o ADR-001 justifica não usar Redis no MVP).
- **"O VLibras é oficial?"** — Sim, é o avatar oficial do governo; nós o alimentamos por código com as frases finalizadas.
- **"A transcrição fica salva? O que persiste?"** — O **áudio nunca é armazenado**. As **sessões e suas palavras-chave persistem no PostgreSQL** (SQLAlchemy async); já as transcrições ao vivo trafegam via WebSocket e não são persistidas (decisão do ADR-004). A transcrição pode ser exportada em .txt pelo palestrante.

## Versão comprimida (~8 min)

Corte para dar tempo, mantendo o essencial:
1. Capa (0:20) → 2. Problema (0:40) → 3. Solução (0:50) → **4. Demo (3:00)** →
6. Motor (0:50) → 7. IA no produto (0:50) → 8. IA no dev (0:40) → 10. Deploy & acessibilidade (0:30) → 12. Obrigado (0:20).

*Pule os slides 5 (Arquitetura), 9 (Testes) e 11 (Números) na versão curta — mencione 1 frase de cada dentro dos slides vizinhos, se sobrar tempo.*
