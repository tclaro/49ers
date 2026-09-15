# 49ers 2026 — Calendário NFL 🔴🟡

Página estática com o calendário da temporada 2026/27 do San Francisco 49ers,
horários já convertidos para Brasília e o guia de transmissão no Brasil.

## O que tem

- **Calendário** — os 17 jogos + bye, com adversário, horário de Brasília, onde assistir e marcações (⚔️ NFC Oeste, ⭐ imperdível, 🌙 prime time, 🌏 jogo internacional).
- **Contagem regressiva** para o próximo jogo, com adversário e canal.
- **Box score no hover** — passe o mouse (ou toque, no celular) nos jogos já encerrados, marcados com 🔍: abre placar por quarto, comparativo de estatísticas e os líderes da partida.
- **Elenco** — todos os jogadores por grupo, com foto, número, posição, altura/peso, idade, faculdade e status de lesão. Tem busca por nome, posição ou faculdade.
- **NFC Oeste** — classificação da divisão atualizada.

## Como os dados chegam aqui

A página é **100% estática** — não tem backend, e não precisa.

1. Ela desenha primeiro o calendário fixo que está dentro do próprio `index.html`
   (constante `GAMES`). Isso garante que a página funcione mesmo offline.
2. Depois busca os dados ao vivo na **API pública da ESPN**, que não exige chave e
   responde com `Access-Control-Allow-Origin: *` — por isso funciona a partir do
   GitHub Pages. Placares, horários remarcados (flex scheduling), elenco e
   classificação passam a vir de lá.
3. Se a API estiver fora do ar, a página avisa no rodapé e continua exibindo os
   dados fixos.

Endpoints usados:

| O quê | URL |
|---|---|
| Calendário | `site.api.espn.com/apis/site/v2/sports/football/nfl/teams/sf/schedule?season=2026` |
| Box score | `site.api.espn.com/apis/site/v2/sports/football/nfl/summary?event={id}` |
| Elenco | `site.api.espn.com/apis/site/v2/sports/football/nfl/teams/sf/roster` |
| Classificação | `site.api.espn.com/apis/v2/sports/football/nfl/standings?season=2026` |

⚠️ É uma API pública mas **não documentada** pela ESPN — ela pode mudar sem aviso.
Se um dia parar de responder, a página não quebra: volta a mostrar os dados fixos.

## Publicar no GitHub Pages

```bash
git init
git add .
git commit -m "Calendário 49ers 2026"
git branch -M main
git remote add origin https://github.com/SEU-USUARIO/SEU-REPO.git
git push -u origin main
```

Depois, no GitHub: **Settings → Pages → Source: Deploy from a branch → Branch: `main` / `root`**.
Em cerca de um minuto a página fica no ar em `https://SEU-USUARIO.github.io/SEU-REPO/`.

Como o arquivo se chama `index.html`, ele é servido na raiz automaticamente.

## Atualizar o calendário na mão

Se precisar mexer sem depender da API, edite a constante `GAMES` no fim do
`index.html`. Cada jogo é um objeto:

```js
{w:5, d:'2026-10-11', t:'17:25', opp:'SEA', ha:'away', tags:'rival must', badge:'⚔️ ⭐', br:'DAZN • TV BR a confirmar'}
```

- `w` semana · `d` data (AAAA-MM-DD, horário de Brasília) · `t` horário de Brasília
- `opp` sigla do adversário · `ha` `home` ou `away`
- `tags` `rival` (NFC Oeste) e/ou `must` (imperdível) — alimentam os filtros
- `badge` selo exibido no card · `br` onde assistir no Brasil

## Fontes

- [49ers.com — Schedule](https://www.49ers.com/schedule/) — tabela oficial
- [NFL — Ways to Watch Brasil](https://www.nfl.com/international/ways-to-watch/brazil) — quem transmite no Brasil
- [ESPN Brasil](https://www.espn.com.br/nfl/) e [ge · Futebol Americano](https://ge.globo.com/futebol-americano/) — jogo da rodada na TV brasileira
