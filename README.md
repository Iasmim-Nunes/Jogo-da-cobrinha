# 🐍 Cobrinha Neon

Um jogo da cobrinha no estilo **agar.io / slither.io**, feito em um único arquivo HTML,
sem nenhuma dependência externa. Você controla uma cobra neon que cresce ao coletar
esferas brilhantes enquanto disputa o mapa com sete cobras controladas pelo computador.

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
- **Recorde salvo** no navegador (`localStorage`), com fallback silencioso em contextos
  onde o armazenamento não está disponível.
- **Áudio procedural** via Web Audio API — nenhum arquivo de som é carregado.

## Estrutura do projeto

```
.
├── index.html   # o jogo inteiro: HTML, CSS e JavaScript
└── README.md    # este arquivo
```

Tudo vive em `index.html` de propósito: assim o jogo pode ser aberto com dois cliques,
enviado por e-mail ou hospedado em qualquer lugar sem etapa de build.

### Organização do código

O JavaScript fica dentro de uma IIFE e está dividido em blocos comentados:

| Bloco | Responsabilidade |
| --- | --- |
| `constantes` | tamanho do mundo, quantidade de comidas, número de rivais |
| `utilidades` | funções matemáticas e acesso protegido ao `localStorage` |
| `comida` | criação, reposição e partículas |
| `cobras` | criação, movimento, trilha (o corpo), crescimento e turbo |
| `colisões` | busca de corpos próximos e checagem do próprio corpo |
| `cérebro das rivais` | a IA: desviar, evitar parede, caçar comida |
| `partida` | preparar o mundo, começar, encerrar, voltar ao menu, HUD e ranking |
| `lógica` | o passo de simulação de cada quadro |
| `desenho` | chão, comidas, cobras, olhos, partículas e minimapa |

O corpo da cobra não é uma lista de peças independentes: a cabeça grava o caminho
percorrido (um ponto a cada 4 px) e cada segmento é apenas uma posição ao longo desse
rastro. É isso que dá o movimento fluido e faz o corpo crescer sem recalcular nada.

## Publicar no GitHub Pages

Como é um site estático de um arquivo só, dá para publicar direto:

1. Abra **Settings → Pages** no repositório.
2. Em *Source*, escolha **Deploy from a branch**.
3. Selecione a branch `main` e a pasta `/ (root)`, e salve.

Em alguns minutos o jogo fica disponível em
`https://iasmim-nunes.github.io/Jogo-da-cobrinha/`.

## Compatibilidade

Funciona em navegadores modernos de desktop e celular (Chrome, Edge, Firefox, Safari).
No celular o controle é por toque: arraste para guiar a cobra e mantenha o dedo na tela
para o turbo. O canvas se adapta à resolução da tela (`devicePixelRatio`).

## Licença

Uso livre para fins pessoais e educacionais.
