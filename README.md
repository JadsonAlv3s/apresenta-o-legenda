# Apresentação — LegendaViva 🎙️

Apresentação web (full-screen slider) do **LegendaViva**, projeto do Grupo 9 — acessibilidade em tempo real: fala → legenda ao vivo → Libras.

## Como usar

- Abra o `index.html` (ou a URL do deploy).
- **Clique** em "iniciar" para abrir com áudio (intro cinematográfica de 30s — `Esc` pula).
- Avance com **clique**, **← →**, **espaço** ou swipe. `F` = tela cheia.

## Stack

HTML/CSS/JS puros, num único arquivo. Sem dependências, sem build:

- Animação da intro em Canvas 2D (túnel, portal, glitch)
- Trilha sonora 100% sintetizada em WebAudio (sem arquivos de áudio)
- Slides com reveal escalonado, tema de closed-caption

## Deploy

Estático — funciona em qualquer host (Vercel, GitHub Pages, etc.) sem configuração.

App em produção: <https://legendaviva-ifrn.duckdns.org>
