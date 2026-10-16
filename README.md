# GridironFoot 🏈

Gerenciador de futebol americano (NFL) inspirado no clássico **Brasfoot**, entregue como um **único arquivo `index.html`**.

Liga fiel à NFL: as 32 franquias reais nas 8 divisões, estrelas reais de cada elenco (início da temporada 2025; reservas gerados), calendário de 17 jogos em 18 semanas com bye pela fórmula de rotação da NFL e playoffs com 7 times por conferência (Wild Card → Divisional → Final de Conferência → Super Bowl). Jogo de fã, sem afiliação com a NFL.

Abra `index.html` no navegador (precisa de internet para o TailwindCSS via CDN). O progresso é salvo automaticamente no `localStorage`.

## Arquitetura (4 camadas dentro do `<script>`)

1. **Domain & Types** — `@typedef` JSDoc para `Player`, `Team`, `TacticConfiguration`, `MatchEvent`, `MatchState`, `SeasonPhase`, `GameState`, `GameSaveState`, `GameAction`. As fases da temporada e o status da partida são discriminated unions.
2. **Pure Engine** — `calculateUnitStrength`, `simulateMatchTick`, `processSalaryCap`, `generateDraftClass`, `processOffseasonAging`... Nenhuma função lê o DOM; a aleatoriedade entra por um `rng` injetado (Mulberry32, com seed).
3. **State & Store** — `validateAction` + `gameReducer(state, action, env)` puros; o store é o único lugar com efeitos colaterais (persistência com debounce).
4. **UI Renderers** — funções que montam strings a partir do `GameState`; os cliques viram `GameAction`s por event delegation (`data-game` / `data-ui`).

Máquina de estados: `REGULAR_SEASON(1..18) → PLAYOFFS_WILDCARD → PLAYOFFS_DIVISIONAL → PLAYOFFS_CONFERENCE → SUPER_BOWL → SEASON_RECAP → OFFSEASON_DRAFT → REGULAR_SEASON(ano+1)`, e de qualquer fase para `GAME_OVER` se a diretoria demitir o técnico.
