# 🐍 Cobrinha Neon

Um jogo da cobrinha no estilo **agar.io / slither.io**, feito em um único arquivo HTML,
sem nenhuma dependência externa. Você escolhe seu nome, veste uma cobrinha da loja e
cresce coletando esferas brilhantes enquanto disputa o mapa com sete cobras controladas
pelo computador.

> **Jogar:** basta abrir o arquivo [`index.html`](index.html) no navegador. Não precisa
> instalar nada, não precisa de servidor e funciona offline.

---

## Como jogar

| Ação | Controle |
| --- | --- |
| Virar | Mova o **mouse** para onde quer ir, ou use **W A S D** / **setas** |
| Turbo | Segure o **clique** ou a tecla **espaço** |
| Reiniciar | Botão **Jogar de novo** ou tecla **Enter** na tela de fim |
| Voltar ao menu | Botão **Voltar para casa** |

Na tela inicial você digita **seu nome** (ele aparece acima da sua cobra e no ranking) e
pode abrir a **loja de cobrinhas**. Tudo fica salvo no navegador.

### Regras

- Cada esfera coletada vale **10 pontos** e aumenta o comprimento da cobra.
- Quanto maior a cobra, mais grossa ela fica, mais devagar ela anda e mais aberta é a curva.
- O **turbo** deixa a cobra 85% mais rápida, mas consome o corpo — os pedaços perdidos
  caem no rastro como comida para quem vier atrás.
- **Encostar em qualquer parte de outra cobra encerra a partida.** O mesmo vale para elas:
  se uma rival bater em você, ela morre e o corpo dela vira um monte de comida.
- Bater no próprio corpo também encerra a partida (os primeiros segmentos são imunes,
  então curvas fechadas são seguras).
- As paredes do mundo apenas bloqueiam a passagem — nelas você não morre, só desliza.

## Loja de cobrinhas

Ao morrer você ganha **1 moeda por esfera comida** na partida (pontos ÷ 10). As moedas
compram visuais na loja, e cada visual muda a cor do corpo e adiciona um acessório
desenhado sobre a cobra:

| Cobrinha | Preço | Acessório |
| --- | ---: | --- |
| Neon Clássica | grátis | corpo arco-íris que muda de cor |
| Boné Esportivo | 40 | boné com aba |
| Touca de Inverno | 70 | touca com pompom |
| Cachecol Listrado | 110 | cachecol com pontas balançando |
| Chapéu de Festa | 150 | chapéu cônico listrado |
| Laço Fofo | 200 | laço na cabeça |
| Óculos Escuros | 260 | óculos sobre os olhos |
| Cartola Elegante | 320 | cartola com fita |
| Fone Gamer | 400 | headset com conchas |
| Chifrinhos | 520 | par de chifres |
| Coroa Dourada | 700 | coroa com pedra |

As miniaturas da loja são desenhadas pelo próprio motor do jogo — é o mesmo código que
renderiza a cobra em partida, então a prévia é exatamente o que você vai ver jogando.
As cobras rivais também sorteiam visuais do catálogo, então o mapa fica cheio de cobras
de boné, cartola e coroa.

## O que tem no jogo

- **Cobra neon sem cabeça:** o corpo é um tubo de espessura constante que só cresce em
  comprimento; a única parte "com rosto" são os olhinhos na ponta, com as pupilas
  acompanhando a direção do movimento.
- **Sete cobras rivais com IA:** elas caçam a comida mais próxima, desviam de corpos que
  aparecem à frente, evitam as paredes, usam turbo de vez em quando e renascem pequenas
  alguns segundos depois de morrer.
- **Visual 3D em canvas 2D:** cada corpo é desenhado em seis camadas — sombra projetada no
  chão, halo neon, corpo, luz superior, reflexo especular e brilho aditivo. As comidas são
  esferas com gradiente radial, reflexo e sombra.
- **Ranking ao vivo** das cinco maiores cobras e **minimapa** com a posição de todo mundo.
- **Câmera dinâmica:** segue a cabeça e afasta conforme a cobra cresce.
- **Efeitos:** partículas ao comer e ao morrer, tremor de tela na batida, grade em
  perspectiva, vinheta e paredes luminosas que mudam de cor.
- **Perfil salvo** no navegador (`localStorage`): nome, recorde, moedas, visuais comprados
  e o visual em uso. Se o armazenamento não estiver disponível, o jogo continua
  funcionando normalmente, só não guarda o progresso.
- **Áudio procedural** via Web Audio API — nenhum arquivo de som é carregado.

### Organização do código

O JavaScript fica dentro de uma IIFE e está dividido em blocos comentados:

| Bloco | Responsabilidade |
| --- | --- |
| `constantes` | tamanho do mundo, quantidade de comidas, número de rivais e o catálogo `VISUAIS` da loja |
| `utilidades` | funções matemáticas, acesso protegido ao `localStorage` e o `perfil` do jogador |
| `comida` | criação, reposição e partículas |
| `cobras` | criação, movimento, trilha (o corpo), crescimento e turbo |
| `colisões` | busca de corpos próximos e checagem do próprio corpo |
| `cérebro das rivais` | a IA: desviar, evitar parede, caçar comida |
| `partida` | preparar o mundo, começar, encerrar, voltar ao menu, HUD e ranking |
| `loja` | moedas, compra, equipar e as miniaturas dos cartões |
| `lógica` | o passo de simulação de cada quadro |
| `desenho` | chão, comidas, cobras, olhos, acessórios, partículas e minimapa |

Para acrescentar uma cobrinha nova basta adicionar um item ao array `VISUAIS`
(cor do corpo, preço e tipo de acessório) e, se for um acessório inédito, um bloco
correspondente em `desenhaAcessorio`.

O corpo da cobra não é uma lista de peças independentes: a cabeça grava o caminho
percorrido (um ponto a cada 4 px) e cada segmento é apenas uma posição ao longo desse
rastro. É isso que dá o movimento fluido e faz o corpo crescer sem recalcular nada.

Uso livre para fins pessoais e educacionais.
