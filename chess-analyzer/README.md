# Revisão de Partidas (analisador de xadrez)

Página única (`index.html`) que busca suas partidas do chess.com pelo nome de usuário e faz uma revisão lance a lance com o Stockfish, no estilo da "Revisão de Partida" do chess.com.

## Como usar

1. Abra `index.html` no navegador (dois cliques funciona; se o seu navegador bloquear, rode `python3 -m http.server` nesta pasta e abra `http://localhost:8000`).
2. Digite seu usuário do chess.com, escolha a profundidade e clique em **Buscar partidas**.
3. Escolha uma partida. A análise roda no próprio navegador; com profundidade 16, uma partida de 40 lances leva cerca de 1 a 3 minutos.

Também dá para colar um PGN qualquer em **Ou cole um PGN**.

## O que a revisão mostra

- Precisão de cada jogador (fórmula baseada na perda de chance de vitória por lance).
- Classificação de cada lance: Brilhante, Ótimo, Melhor, Excelente, Bom, Forçado, Imprecisão, Erro, Chance perdida e Capivarada.
- Seta com o melhor lance, a linha principal do motor e a ameaça depois de um erro.
- Barra e gráfico de avaliação (clique no gráfico para pular para o lance).

## Experiência

- Comentários em português comum ("Esse cavalo pegando o peão é uma capivarada! O melhor era capturar o peão com o bispo"), sem notação.
- Sons gerados no navegador (lance, captura, xeque, roque, mate e um som para cada classificação), com botão para desligar.
- Efeitos: peças deslizam, peças capturadas somem girando, partículas para lances brilhantes e ótimos, tremida e flash vermelho na capivarada, confete no mate, vibração no celular.
- Tela de resumo com anéis de precisão animados, contagem de lances e atalho para o momento mais caro.
- Botão estrela pula para o próximo momento importante; deslize no tabuleiro para navegar.

## Detalhes técnicos

- Dados: API pública do chess.com (`api.chess.com/pub/player/<user>/games/archives`).
- Motor: Stockfish 10 em WebAssembly (`stockfish@10.0.2`, via jsDelivr) dentro de um Web Worker, com MultiPV 2.
- Regras: `chess.js` 0.10.3.
- Peças: conjunto cburnett (Colin M.L. Burnett, CC BY-SA 3.0), embutido como SVG.
- Chance de vitória: `50 + 50 * (2 / (1 + e^(-0.00368208 * cp)) - 1)`. Limiares de perda: < 2 excelente, < 5 bom, < 10 imprecisão, < 20 erro, ≥ 20 capivarada.
