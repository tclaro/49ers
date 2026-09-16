# Calendário NFL 2026 — 49ers 🔴🟡 · Eagles 🦅💚 · Chargers ⚡💙

Página estática com o calendário da temporada 2026/27, horários já convertidos
para Brasília e o guia de transmissão no Brasil. Dá para alternar entre três
times pelos escudos no cabeçalho — a página inteira (cores, calendário, elenco,
divisão) se ajusta ao time escolhido.

## O que tem

- **Seletor de times** — 49ers, Eagles e Chargers. O escudo escolhido troca a paleta (cabeçalho, botões, cards, selos), o calendário, o elenco e a divisão. A escolha fica salva no navegador e na URL (`?time=phi`), então dá para compartilhar o link já no time certo.
- **Calendário** — os 17 jogos + bye, com adversário, horário de Brasília, onde assistir e marcações (⚔️ divisão, ⭐ imperdível, 🌙 prime time, 🌏 jogo internacional).
- **Filtros** — todos, em casa, fora, jogos da divisão e imperdíveis.
- **Contagem regressiva** para o próximo jogo, com adversário e canal.
- **Box score no hover** — passe o mouse (ou toque, no celular) nos jogos já encerrados, marcados com 🔍: abre placar por quarto, comparativo de estatísticas e os líderes da partida.
- **Elenco** — todos os jogadores por grupo, com foto, número, posição, altura/peso, idade, faculdade e status de lesão. Tem busca por nome, posição ou faculdade.
- **Divisão** — classificação atualizada da divisão do time selecionado (NFC Oeste, NFC Leste ou AFC Oeste).

## Como os dados chegam aqui

A página é **100% estática** — não tem backend, e não precisa.

1. Para os **49ers** ela desenha primeiro o calendário fixo que está dentro do
   próprio `index.html` (constante `CAL_SF`), então esse time funciona mesmo
   offline.
2. Depois busca os dados ao vivo na **API pública da ESPN**, que não exige chave e
   responde com `Access-Control-Allow-Origin: *` — por isso funciona a partir do
   GitHub Pages. Placares, horários remarcados (flex scheduling), elenco e
   classificação passam a vir de lá.
3. Para **Eagles** e **Chargers** não existe tabela fixa no arquivo: o calendário
   inteiro é montado a partir da resposta da ESPN. Os selos são deduzidos ali
   mesmo — jogo de divisão vira ⚔️, o horário nos EUA vira 🌙 TNF/SNF/MNF, estádio
   fora dos EUA vira 🌏, e a emissora americana é traduzida para o equivalente
   brasileiro (NBC → SporTV, ESPN/ABC → ESPN·Disney+, e assim por diante). A bye
   week é a semana que falta na sequência.
4. Se a API estiver fora do ar, a página avisa no rodapé. Nos 49ers ela continua
   exibindo os dados fixos; nos outros dois times pede para recarregar online.

Endpoints usados (`{time}` = `sf`, `phi` ou `lac`):

| O quê | URL |
|---|---|
| Calendário | `site.api.espn.com/apis/site/v2/sports/football/nfl/teams/{time}/schedule?season=2026` |
| Box score | `site.api.espn.com/apis/site/v2/sports/football/nfl/summary?event={id}` |
| Elenco | `site.api.espn.com/apis/site/v2/sports/football/nfl/teams/{time}/roster` |
| Classificação | `site.api.espn.com/apis/v2/sports/football/nfl/standings?season=2026` |

⚠️ É uma API pública mas **não documentada** pela ESPN — ela pode mudar sem aviso.
Se um dia parar de responder, os 49ers voltam a mostrar os dados fixos.

## Publicar no GitHub Pages

```bash
git init
git add .
git commit -m "Calendário NFL 2026"
git branch -M main
git remote add origin https://github.com/SEU-USUARIO/SEU-REPO.git
git push -u origin main
```

Depois, no GitHub: **Settings → Pages → Source: Deploy from a branch → Branch: `main` / `root`**.
Em cerca de um minuto a página fica no ar em `https://SEU-USUARIO.github.io/SEU-REPO/`.

Como o arquivo se chama `index.html`, ele é servido na raiz automaticamente.

## Adicionar outro time

Basta acrescentar uma entrada na constante `TEAMS`, dentro do `index.html`:

```js
dal:{abbr:'DAL',nome:'Dallas Cowboys',curto:'Cowboys',lema:'How Bout Them Cowboys ⭐',
     kicker:'Dallas Cowboys • NFL 2026',conf:'NFC',div:'NFC Leste',
     divTimes:['DAL','PHI','NYG','WSH'],
     rosterUrl:'…',schedUrl:'…',schedNome:'dallascowboys.com — Schedule',
     cor:{red:'#041e42',dark:'#02132a',gold:'#869397',sand:'#eef1f4',sand2:'#dde3e9',
          evtime:'#d7e0ea',evscore:'#ffd98a'}}
```

A chave (`dal`) é a sigla que a ESPN usa na URL. As cores viram variáveis CSS:
`red` é a cor principal (fundo de botões e cards, precisa de contraste com texto
branco), `dark` o tom escuro do cabeçalho, `gold` o detalhe, `sand`/`sand2` os
fundos claros e `evtime`/`evscore` o horário e o placar dentro dos cards.

## Atualizar o calendário dos 49ers na mão

Se precisar mexer sem depender da API, edite a constante `CAL_SF` no
`index.html`. Cada jogo é um objeto:

```js
{w:5, d:'2026-10-11', t:'17:25', opp:'SEA', ha:'away', tags:'rival must', badge:'⚔️ ⭐', br:'DAZN • TV BR a confirmar'}
```

- `w` semana · `d` data (AAAA-MM-DD, horário de Brasília) · `t` horário de Brasília
- `opp` sigla do adversário · `ha` `home` ou `away`
- `tags` `rival` (divisão) e/ou `must` (imperdível) — alimentam os filtros
- `badge` selo exibido no card · `br` onde assistir no Brasil

## Fontes

- [49ers.com](https://www.49ers.com/schedule/) · [philadelphiaeagles.com](https://www.philadelphiaeagles.com/schedule/) · [chargers.com](https://www.chargers.com/schedule/) — tabelas oficiais
- [NFL — Ways to Watch Brasil](https://www.nfl.com/international/ways-to-watch/brazil) — quem transmite no Brasil
- [ESPN Brasil](https://www.espn.com.br/nfl/) e [ge · Futebol Americano](https://ge.globo.com/futebol-americano/) — jogo da rodada na TV brasileira
