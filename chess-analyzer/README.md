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

- Comentários que dizem exatamente qual peça: cada peça citada vira uma etiqueta com o desenho e as casas (ex.: cavalo e5→f7). Tocar nela destaca a peça no tabuleiro.
- **Tentar de novo**: nos seus erros, você joga no tabuleiro (tocar ou arrastar) para achar o lance certo, com dica e solução.
- **Treinar meus erros**: todos os seus erros da partida viram uma sequência de desafios com pontos e até 3 estrelas.
- **Ver o melhor lance** e **E agora?**: animam a sequência do motor no tabuleiro.
- Reprodução automática, botão para o próximo momento-chave, deslizar no tabuleiro para navegar.
- Resumo com precisão, desempenho estimado, contagem de lances, nota por fase (abertura, meio-jogo, final) e momentos-chave.
- Mascote treinador com expressões que mudam conforme o lance.
- Sons sintetizados (madeira, captura, xeque, roque, promoção, mate, sino de acerto, trombone triste na capivarada) com eco leve.
- Efeitos: peças deslizam e "pousam", poeira ao pousar, estilhaços na captura, estrelas e brilho no lance brilhante, tremida e flash na capivarada, rei tombando e confete no mate.
- Animação própria para cada classificação: Brilhante (cena de cinema de ~5 s: faixas pretas de filme, o mundo congela em preto e branco, a peça levita com energia orbitando, batidas de coração; impacto com flash, aberração cromática, terremoto e golpe de orquestra; abre um cosmos em velocidade da luz com um cristal "!!" girando em 3D, título letra por letra, subtítulo com o sacrifício, mascote comemorando, confete e fogos; toque para pular), Excelente (joinha gigante com feixes, chafariz de estrelas e acorde), Ótimo (selo azul com faíscas), Melhor (estrela que sobe), Bom (check que se desenha), Teoria (livro que abre e solta páginas), Forçado (seta que passa), Imprecisão (?! balançando e interrogações flutuando), Erro (? que cai e quica), Chance perdida (X riscado), Capivarada (vidro trincando, tela tremendo, tela vermelha e letras que desabam).
- Ajustes: 6 temas de tabuleiro, 2 conjuntos de peças, sons, efeitos e coordenadas.

## Detalhes técnicos

- Dados: API pública do chess.com (`api.chess.com/pub/player/<user>/games/archives`).
- Motor: Stockfish 10 em WebAssembly (`stockfish@10.0.2`, via jsDelivr) dentro de um Web Worker, com MultiPV 2.
- Regras: `chess.js` 0.10.3.
- Peças: Staunty (sadsnake1, CC BY-NC-SA 4.0, via cm-chessboard) e cburnett (Colin M.L. Burnett, CC BY-SA 3.0), embutidas como SVG.
- Chance de vitória: `50 + 50 * (2 / (1 + e^(-0.00368208 * cp)) - 1)`. Limiares de perda: < 2 excelente, < 5 bom, < 10 imprecisão, < 20 erro, ≥ 20 capivarada.
