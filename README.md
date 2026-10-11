# Corujão da Safirion — 4ª Edição · Landing Page

Landing page com a arte oficial da 4ª edição (16 de outubro, a partir das 18 horas).
O botão "ENTRAR NO GRUPO" já vem desenhado no alto da arte; a página põe por cima dele
uma área de clique invisível. Abaixo da arte, a página continua com as provas sociais.

## Estrutura

```
index.html                 ← a landing page (auto-contida, responsiva)
assets/arte-16-out.webp    ← arte completa 1080×1920 (a primeira tela)
assets/og-16-out.jpg       ← a mesma arte em 720×1280, para a prévia do link (WhatsApp, Telegram)
assets/provas/             ← provas sociais: print-1…6.webp, depoimento-video.mp4 e a capa .jpg
assets/fonts/              ← Montserrat (também embutida em base64 no HTML)
```

As artes e camadas das edições anteriores (`assets/arte-edicao-3.webp`, `assets/*.webp` e
`*.png`) continuam no repositório, mas não são mais usadas pelo `index.html`.

O palco mantém a proporção 1080×1920 e sempre cabe na tela (mobile e desktop).
A área de clique (`.cta`) é em `%` do palco, então escala junto com a arte. Ela é maior
que o botão desenhado, para o toque ter 44px ou mais no celular; o `.cta::after` fica
exatamente sobre o desenho (pulso, brilho e foco). Ao trocar a arte, meça onde o botão
fica nela e ajuste `top`, `width` e `height` da `.cta` e o `inset` do `.cta::after`.

## Provas sociais

Abaixo da arte fica a seção "Quem já viveu o Corujão": um depoimento em vídeo (com um trecho da
fala escrito ao lado) e os prints de conversa, seguidos de um botão "ENTRAR NO GRUPO".

- **Celular (até 767px):** os prints viram um **carrossel**, um por vez num quadro 4:3. Troca
  pelas setas, por deslize lateral ou sozinho a cada 4,5 s (para de andar sozinho quando a
  pessoa mexe, e não anda com "menos movimento" ligado). Não há rolagem lateral: todos os
  prints ocupam a mesma célula e a troca é por opacidade. Sem JavaScript, ficam um embaixo do
  outro.
- **Tablet (768px) e computador (1024px):** mural com todos os prints à vista, em 2 e 3 colunas
  (no computador, o vídeo em pé é a primeira coluna).
- **Acrescentar um print:** salve a imagem em `assets/provas/` e copie um `<figure class="prova pN revelar">`
  dentro de `#mural`, no `index.html`, com `width`, `height`, um `alt` curto e o texto do print na
  `<figcaption class="sr-only">`. A ordem no HTML é a ordem do carrossel. No mural, os 3 primeiros
  formam uma coluna e os demais a outra (`.mural > :nth-child(...)`), e cada print precisa da sua
  regra `.pN{grid-row:span X}`, com X = altura ÷ largura × 200 + 8. Mantenha as duas colunas com
  somas parecidas.
- **Vídeo:** só é baixado quando a pessoa toca no play; pausa ao sair da tela.
- **Botão do fim:** mesmo link e mesmo evento `Lead` do botão da arte.

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
