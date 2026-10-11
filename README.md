# Corujão da Safirion — 4ª Edição · Landing Page

Landing page com a arte oficial da 4ª edição (16 de outubro, a partir das 18 horas).
O botão "ENTRAR NO GRUPO" já vem desenhado no alto da arte; a página põe por cima dele
uma área de clique invisível. Abaixo da arte há uma segunda tela, do mesmo tamanho, com as
provas sociais em carrossel.

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

## Provas sociais (segunda tela)

Regra do dono (11/10/2026): **tudo fica na mesma proporção da hero, sempre para celular, e as
informações ficam em carrossel de provas sociais.**

- A segunda tela (`.painel`) tem a mesma largura e a mesma proporção 1080×1920 do palco da arte em
  qualquer aparelho. No computador e no tablet ela NÃO alarga: são duas telas em pé, centradas.
- Tudo dentro dela é medido em `cqw` (1% da largura do painel), com piso em `rem` para toque e
  leitura, então escala junto, como a arte. Seção nova nesta página segue o mesmo molde.
- As provas ficam num **carrossel** de 4 slides: o vídeo (em pé, com um trecho da fala por cima do
  pé) e os 6 prints, dois por slide. Troca pelas setas, por deslize lateral ou sozinho a cada 6 s
  (para quando a pessoa mexe; não anda com "menos movimento"). Não há rolagem lateral: os slides
  ocupam a mesma célula e a troca é por opacidade. Sem JavaScript, tudo fica um embaixo do outro.
- **Acrescentar prints:** salve em `assets/provas/` e crie mais um `<div class="slide sl-X">` com
  dois `<figure class="prova">` dentro de `#carrossel` (com `width`, `height`, `alt` curto e o
  texto do print na `<figcaption class="sr-only">`). Dê ao slide a regra
  `.js-ok .sl-X{grid-template-rows:minmax(0,Afr) minmax(0,Bfr)}`, com A e B = altura de cada print
  numa largura de 342px. Os pontos do carrossel se contam sozinhos.
- **Vídeo:** só é baixado quando a pessoa toca no play; pausa ao trocar de slide ou sair da tela.
- **Botão "ENTRAR NO GRUPO" da segunda tela:** mesmo link e mesmo evento `Lead` do botão da arte.
  Fica **grudado no pé da tela enquanto a pessoa rola** (`position:sticky`) e pousa no pé da segunda
  tela no fim da rolagem. Por isso o `.painel-in` é coluna em flex, e não grade: item de grade só
  gruda dentro da própria linha.

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
