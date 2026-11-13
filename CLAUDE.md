# Guia para agentes — como adicionar ou atualizar uma app

Este repositório é a **fonte única** das apps da CloudLabs. Daqui saem o README (montra no GitHub) e o site **cloudlabsdev.com**. Segue este guia sempre que te pedirem para adicionar uma app nova ou mudar uma existente, para que todas fiquem iguais.

## Idiomas — exigência

- **O site cloudlabsdev.com é multilingue e tem de continuar a ser.** Hoje está em 14 idiomas: inglês (predefinido), chinês simplificado, hindi, espanhol, árabe, francês, bengali, português (Portugal), russo, indonésio, urdu, alemão, japonês e pidgin nigeriano. Nenhuma app aparece no site sem estar nos 14.
- **Neste repositório, cada app tem obrigatoriamente os textos em português (`pt`) e em inglês (`en`)** na sua ficha. Não basta só português. As outras 12 línguas são feitas pela conversa do site, a partir destes dois.
- **Os idiomas da própria app** vão no campo `languages` da ficha, porque há apps com mais do que um idioma e o site mostra isso. Tira-os do projeto (ex.: as pastas `*.lproj` ou o `Localizable.xcstrings` no Xcode, os ficheiros de tradução na web), nunca de memória.
- O README fica em português, com a tabela Resumo a indicar os idiomas de cada app.

## Estrutura — uma pasta por app

Cada app tem a **sua própria pasta** em `apps/<id>/`, com tudo o que lhe diz respeito:

```
apps/
  <id>/
    app.json        ← ficha da app (obrigatório)
    icon.png        ← ícone 1024×1024 (obrigatório)
    screenshots/    ← capturas de ecrã (opcional)
README.md           ← montra: uma secção por app + tabela Resumo
CLAUDE.md           ← este guia
```

`<id>` é o nome da app em minúsculas, sem acentos, com hífens: `rich-light`, `dare-for-two`, `openlater`, `sling`. Usa sempre o mesmo `id` na pasta e na ficha.

A pasta `assets/` é antiga: **não acrescentes lá nada**. Se a tua app ainda tem o ícone em `assets/<id>.png`, muda-o para `apps/<id>/icon.png` com `git mv` e corrige o caminho da imagem no README.

## O que cada app tem de ter

Tudo no mesmo commit:

### 1. Ícone — `apps/<id>/icon.png`

- PNG quadrado de **1024×1024 px** — o ícone verdadeiro da app, exportado do projeto (ex.: o `AppIcon` do Xcode), nunca um desenho improvisado.
- Fundo opaco e cantos **quadrados** (o site arredonda-os). Exceção: ícones de Mac que já trazem forma e margem transparente — nesse caso põe `"iconInset": true` na ficha.
- Sem texto, sem molduras, sem metadados (localização, autor). Peso máximo 500 KB (otimiza se for maior).
- Se ainda não houver ícone, **não inventes**: deixa `"icon": null` na ficha e diz ao Rafael que falta.

### 2. Ficha — `apps/<id>/app.json`

```json
{
  "id": "rich-light",
  "name": "Rich Light",
  "order": 4,
  "icon": "icon.png",
  "iconInset": false,
  "tint": "#ffc72e",
  "status": "em-breve",
  "category": "utilitarios",
  "topics": ["sem-anuncios"],
  "platforms": ["iPhone"],
  "languages": ["pt-PT", "en", "es", "fr", "de"],
  "pt": {
    "tagline": "Um botão. Luz.",
    "text": "2 a 3 frases sobre o que a app faz, em português de Portugal.",
    "platformsNote": "",
    "note": ""
  },
  "en": {
    "tagline": "One button. Light.",
    "text": "The same 2 to 3 sentences, in English.",
    "platformsNote": "",
    "note": ""
  },
  "links": {
    "site": "https://…",
    "webApp": null,
    "download": null,
    "appStore": null
  },
  "source": "fechado"
}
```

Regras de cada campo:

- **`order`** — a posição da app na montra (1 = primeira). Uma app nova fica no fim: usa o maior `order` que já existe + 1.
- **`icon`** — caminho dentro da pasta da app (normalmente `"icon.png"`), ou `null`.
- **`tint`** — a cor principal da marca da app, em hexadecimal (tira-a do ícone ou do site da app). O site usa-a para tingir o cartão.
- **`status`** — só um destes valores:
  - `disponivel` — já se pode usar (descarregar, abrir na web…);
  - `em-breve` — pronta, à espera de aprovação (App Store, TestFlight…);
  - `em-desenvolvimento` — ainda a ser feita.
- **`category`** — **obrigatório**, uma só, a que melhor descreve a app. O site usa-a no filtro por categorias. Só destes valores:
  - `produtividade` — trabalho, foco, organização, assistentes;
  - `jogos` — qualquer jogo;
  - `utilitarios` — ferramentas do dia a dia (lanterna, rede, ecrã, ficheiros…);
  - `saude` — bem-estar, sono, relaxamento, exercício;
  - `estilo-de-vida` — memórias, relações, hobbies, casa;
  - `educacao` — aprender, estudar;
  - `criatividade` — foto, vídeo, música, desenho, escrita;
  - `financas` — dinheiro, orçamento, faturação.
  Se nenhuma servir, não inventes uma: pergunta ao Rafael — acrescentar uma categoria nova também obriga a traduzi-la no site.
- **`topics`** — **obrigatório** (pode ficar `[]`), até 5 tópicos que ajudam a pesquisa e o filtro. Só desta lista, e só se for verdade pela descrição da app: `ia`, `foco`, `sono`, `relaxar`, `casais`, `memorias`, `arcada`, `pixel-art`, `rede`, `widgets`, `sem-anuncios`, `sem-rastreio`. Para um tópico novo, pergunta ao Rafael (também tem de ser traduzido no site).
- **`platforms`** — só destes nomes, por esta ordem: `Mac`, `Windows`, `iPhone`, `iPad`, `Android`, `Web`.
- **`languages`** — os idiomas em que a **app** está disponível, em códigos BCP 47 (`pt-PT`, `pt-BR`, `en`, `es`, `fr`, `de`, `it`, `ja`, `zh-Hans`, `ar`…), tirados do projeto. Se ainda não souberes, põe `null` e avisa o Rafael. Nunca adivinhes.
- **`pt` e `en`** — os dois são obrigatórios e dizem o mesmo; o `en` é tradução natural, não palavra a palavra.
  - **`tagline`** — a frase forte: curta (até ~8 palavras), concreta, sem pontos de exclamação.
  - **`text`** — 2 a 3 frases: o que faz e para quem. Só factos verdadeiros.
  - **`platformsNote`** — versões mínimas, se importarem (ex.: `"macOS 13 ou mais recente"`). Vazio se não houver.
  - **`note`** — avisos curtos (ex.: `"Para maiores de 18"`, `"Em testes no TestFlight"`). Vazio se não houver.
- **`links`** — `null` quando não existe. O link da App Store fica `null` até a Apple aprovar.
- **`source`** — `"aberto"` ou `"fechado"`.

### 3. Secção no README

Copia o formato das apps que já lá estão — ícone com `width="88"` a apontar para `apps/<id>/icon.png`, descrição, plataformas, links — e acrescenta a linha na tabela **Resumo**, com a coluna de idiomas. O texto é o mesmo da ficha em português, e a secção fica na posição do `order`.

## Regras que nunca se quebram

- **Não inventes nada**: funcionalidades, números, preços, prémios, testemunhos, datas de lançamento. Se não sabes, pergunta ao Rafael ou deixa o campo vazio.
- **Nada privado**: sem chaves, tokens, emails pessoais, moradas, nomes de clientes, links de painéis internos ou de repositórios privados.
- Texto em **português de Portugal** (tu, não você) e inglês simples.
- Não mexas nas outras apps, a não ser que te peçam.
- Antes de começar, faz `git pull`: há várias conversas a trabalhar neste repositório.
- Mensagem do commit: `Adicionar <Nome>` ou `<Nome>: <o que mudou>`.

## Depois do push

Diz ao Rafael que a app está no repositório. O site **cloudlabsdev.com** não se atualiza sozinho: é preciso pedir à conversa do site (pasta `cloudlabs-site/`) para:

1. correr `node ferramentas/sincronizar.mjs` (traz as pastas `apps/*/` para o site e reduz os ícones);
2. acrescentar os textos da app nos outros 12 idiomas em `dados/traducoes.json`;
3. correr `node build.mjs` — **o site recusa-se a ser gerado** se faltar alguma tradução, ou se a categoria/tópico não existir em `filtros.mjs`;
4. publicar.

No site, cada app aparece num cartão com pesquisa e filtros por categoria, plataforma e tópico — por isso `category` e `topics` têm de estar certos.

## Lista de verificação

- [ ] Pasta `apps/<id>/` criada; nada novo em `assets/`
- [ ] `apps/<id>/icon.png` com 1024×1024 (ou `"icon": null` e o Rafael avisado)
- [ ] `apps/<id>/app.json` com todos os campos, **PT e EN preenchidos**, JSON válido
- [ ] `category` (uma, da lista) e `topics` (da lista, até 5)
- [ ] `languages` com os idiomas reais da app (ou `null` e o Rafael avisado)
- [ ] Secção no README (imagem em `apps/<id>/icon.png`) e linha na tabela Resumo
- [ ] Nada inventado, nada privado
