# Corujão da Safirion — 3ª Edição · Landing Page

Landing page com a arte oficial da 3ª edição (18 de setembro, a partir das 18 horas)
e um botão "Entrar no grupo" sobreposto na parte de baixo da imagem.

## Estrutura

```
index.html                 ← a landing page (auto-contida, responsiva)
assets/arte-edicao-3.webp  ← arte completa 1080×1920 (única imagem usada)
assets/fonts/              ← Montserrat (também embutida em base64 no HTML, usada no botão)
```

As camadas antigas (`assets/*.webp` e `*.png` da edição anterior) continuam no repositório,
mas não são mais usadas pelo `index.html`.

O palco mantém a proporção 1080×1920 e sempre cabe na tela (mobile e desktop).
O botão usa unidades `cqw`/`%`, então escala junto com a arte. Para ajustar a posição,
edite `bottom`, `width` e `height` da classe `.cta` no `index.html`.

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
