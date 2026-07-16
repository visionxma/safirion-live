# Corujão de Traders — Landing Page (Safirion)

Landing page em camadas que reproduz **exatamente** a arte do Photoshop, empilhando cada grupo
de camada como PNG→WebP na mesma ordem do painel (de baixo para cima), com os mesmos
modos de mesclagem (`screen` para brilhos/estrelas/leaks, `soft-light` para FX/textura).

## Estrutura

```
index.html        ← a landing page (auto-contida, responsiva)
assets/           ← camadas otimizadas em .webp (16 MB → 2,4 MB, −85%)
  background.webp  sombras.webp  brilho-azul2.webp  fotos.webp
  estrela.webp     fx.webp       brilho-azul.webp   leaks.webp
  logo.webp        textura.webp
  (texto.webp e botao.webp existem mas não são mais usados — o texto agora é HTML real)
assets/fonts/     ← fontes self-hosted em woff2 com subset (~42 KB no total)
  barlow-400.woff2  montserrat-500.woff2  montserrat-700.woff2  opensans-400.woff2
*.png             ← camadas originais exportadas do Photoshop (fonte, editáveis)
```

## Texto real (não imagem)

TEXTO e BOTÃO são HTML/CSS de verdade — selecionáveis, indexáveis e nítidos em
qualquer resolução — com tipografia casada com a arte por medição pixel a pixel:

| Elemento | Fonte | Ajuste |
|---|---|---|
| CORUJÃO / DE TRADERS | Barlow 400 | cap-height 10% da largura, tracking levemente negativo |
| 25 DE JULHO | Montserrat 700 | tracking largo (≈0,45 em) |
| A PARTIR DAS 18 HORAS | Montserrat 500 | tracking largo |
| ENTRAR NO GRUPO | Open Sans 400 | pill `#006AC1` 464×101, cantos totalmente arredondados |

Os tamanhos usam unidades de container (`cqw`), então o texto escala junto com o
palco em qualquer tela, mantendo o alinhamento da arte (verificado por screenshot
headless: todas as linhas dentro de 1–2 px do original).

## Camadas (do painel do Photoshop → z-index no CSS)

| # | Camada | Papel no design | Blend |
|---|--------|-----------------|-------|
| 12 | **TEXTURA FOLHA** | Textura de papel/folha — profundidade e aspecto orgânico | `soft-light` 22% |
| 11 | **LOGO** | Identidade visual principal da marca (Safirion) | normal |
| 10 | **Leaks Light Fx** | Vazamento de luz — iluminação cinematográfica premium | `screen` 85% |
| 9 | **BRILHO AZUL (2º)** | Glow azul difuso iluminando por cima da composição | `screen` |
| 8 | **FX** | Efeitos especiais — partículas, reflexos, gradientes | `soft-light` 50% |
| 7 | **ESTRELA BRILHANTE BRANCA** | Sparkles — destaca pontos específicos do layout | `screen` |
| 6 | **BOTÃO** | CTA "Entrar no grupo" (clicável, hover + pulso) | normal |
| 5 | **TEXTO** | Títulos e informações (Corujão de Traders · 25 de Julho) | normal |
| 4 | **FOTOS** | Collage dos traders | normal |
| 3 | **BRILHO AZUL** | Glow ciano intenso atrás das fotos | `screen` |
| 2 | **SOMBRAS PRETAS** | Sombras escuras — contraste e profundidade na base | normal |
| 1 | **BACKGROUND** | Fundo principal, base de toda a composição | normal |

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

## Configurar o botão "Entrar no grupo"

No final do `index.html`, troque a linha:

```js
var GRUPO_URL = "#";
```

pela URL real do grupo (WhatsApp/Telegram), ex.:

```js
var GRUPO_URL = "https://chat.whatsapp.com/SEU-CODIGO";
```

O palco mantém a proporção 1080×1920 e sempre cabe na tela (mobile e desktop).
