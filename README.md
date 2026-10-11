# Corujão da Safirion — 4ª Edição · Landing Page

Landing page com a arte oficial da 4ª edição (16 de outubro, a partir das 18 horas).
O botão "ENTRAR NO GRUPO" já vem desenhado no alto da arte; a página põe por cima dele
uma área de clique invisível.

## Estrutura

```
index.html                 ← a landing page (auto-contida, responsiva)
assets/arte-16-out.webp    ← arte completa 1080×1920 (única imagem usada)
assets/og-16-out.jpg       ← a mesma arte em 720×1280, para a prévia do link (WhatsApp, Telegram)
assets/fonts/              ← Montserrat (também embutida em base64 no HTML)
```

As artes e camadas das edições anteriores (`assets/arte-edicao-3.webp`, `assets/*.webp` e
`*.png`) continuam no repositório, mas não são mais usadas pelo `index.html`.

O palco mantém a proporção 1080×1920 e sempre cabe na tela (mobile e desktop).
A área de clique (`.cta`) é em `%` do palco, então escala junto com a arte. Ela é maior
que o botão desenhado, para o toque ter 44px ou mais no celular; o `.cta::after` fica
exatamente sobre o desenho (pulso, brilho e foco). Ao trocar a arte, meça onde o botão
fica nela e ajuste `top`, `width` e `height` da `.cta` e o `inset` do `.cta::after`.

## Como visualizar

- **Simples:** dê duplo clique em `index.html` (abre no navegador).
- **Servidor local:** `python3 -m http.server 8000` e acesse `http://localhost:8000`.

## Meta Pixels

Dois pixels instalados no `<head>` (client-side, com fallback `<noscript>`):

- Pixel 01: `988404074235952`
- Pixel 02: `2533569613744356`

Eventos: `PageView` no carregamento e `Lead` no clique do botão (nos dois pixels).

⚠️ Os **tokens da API de Conversões nunca devem ir no HTML** — são segredos de
servidor. Guarde-os fora deste projeto (o site é estático e todo o código-fonte é
público). Use-os apenas em backend próprio, Conversions API Gateway ou integração
de plataforma (ex.: eventos server-side do gerenciador de tags).

## Botão "Entrar no grupo"

Já aponta para o grupo do Telegram (configurado no final do `index.html`):

```js
var GRUPO_URL = "https://t.me/+bC0pRRqHmVpkYzhh";
```

Para trocar, basta editar essa linha. O clique também dispara o evento `Lead`
nos dois Meta Pixels antes de abrir o link.
