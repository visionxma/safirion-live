# Corujão de Traders — Landing Page (Safirion)

Landing page em camadas que reproduz **exatamente** a arte do Photoshop, empilhando cada grupo
de camada como PNG→WebP na mesma ordem do painel (de baixo para cima), com os mesmos
modos de mesclagem (`screen` para brilhos/estrelas/leaks, `soft-light` para FX/textura).

## Estrutura

```
index.html        ← a landing page (auto-contida, responsiva)
assets/           ← camadas otimizadas em .webp
  background.webp  sombras.webp  fotos.webp   estrela.webp
  fx.webp          brilho-azul.webp           leaks.webp
  logo.webp        textura.webp
  (texto.webp e botao.webp existem mas não são mais usados — o texto agora é HTML real)
assets/fonts/     ← fontes woff2 com subset (também embutidas em base64 no HTML)
  anton-400.woff2  montserrat-400/500/700/800.woff2  bebas-400.woff2
*.png             ← camadas originais exportadas do Photoshop (fonte, editáveis)
```

## Texto real (não imagem)

TEXTO e BOTÃO são HTML/CSS de verdade — selecionáveis, indexáveis e nítidos em
qualquer resolução — com tipografia casada com a arte por medição pixel a pixel:

| Elemento | Fonte | Ajuste |
|---|---|---|
| CORUJÃO / DE TRADERS | **Montserrat 800** | duas linhas igualadas na mesma largura (bloco alinhado) |
| 25 DE JULHO | Montserrat 700 | tracking largo |
| A PARTIR DAS 18 HORAS | Montserrat 400 + **18 HORAS** 800 | tracking largo, peso misto |
| ENTRAR NO GRUPO | Montserrat 500 + **NO GRUPO** 800 | pill `#006AC1`, peso misto |

> As fontes vão **embutidas em base64** dentro do `index.html`, então carregam em
> qualquer navegador (inclusive Safari via `file://`), sem requisição externa.

Os tamanhos usam unidades de container (`cqw`), então o texto escala junto com o
palco em qualquer tela, mantendo o alinhamento da arte (verificado por screenshot
headless: todas as linhas dentro de 1–2 px do original).

## Camadas (do painel do Photoshop → z-index no CSS)

| # | Camada | Papel no design | Blend |
|---|--------|-----------------|-------|
| 12 | **TEXTURA FOLHA** | Textura de papel/folha — profundidade e aspecto orgânico | `soft-light` 22% |
| 11 | **LOGO** | Identidade visual principal da marca (Safirion) | normal |
| 10 | **Leaks Light Fx** | Vazamento de luz — iluminação cinematográfica premium | `screen` 85% |
| 9 | _(removida)_ | camada de brilho azul 2 foi apagada do projeto | — |
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

## Botão "Entrar no grupo"

Já aponta para o grupo do Telegram (configurado no final do `index.html`):

```js
var GRUPO_URL = "https://t.me/+VnIXDh-4jTw2YjFh";
```

Para trocar, basta editar essa linha. O clique também dispara o evento `Lead`
nos dois Meta Pixels antes de abrir o link.

O palco mantém a proporção 1080×1920 e sempre cabe na tela (mobile e desktop).
