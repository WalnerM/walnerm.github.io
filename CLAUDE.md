# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## O que é este repositório

Site acadêmico pessoal de Walner Mendonça (professor de Matemática na UFC), publicado em
<https://walnerm.github.io/> via GitHub Pages. É um site Jekyll construído sobre um fork do
tema `jekyll-theme-console`.

**Atenção:** o `README.md` da raiz é o README *upstream do tema*, não deste site. Ele documenta
como instalar/customizar `jekyll-theme-console` e não descreve o conteúdo nem o fluxo deste site.
O mesmo vale para `jekyll-theme-console.gemspec` e `LICENSE.txt`, herdados do fork.

## Comandos

```bash
bundle install                       # instala as gems (Ruby 3.x; a CI usa 3.1)
bundle exec jekyll serve             # preview com live-reload em http://localhost:4000
bundle exec jekyll build             # gera _site/ (ignorado pelo git)
```

Build de produção (habilita o Google Analytics, que só é injetado quando
`jekyll.environment == 'production'`):

```powershell
$env:JEKYLL_ENV='production'; bundle exec jekyll build
```

Não há testes nem linters. `_config.yml` **não** é recarregado pelo `jekyll serve` — reinicie o
servidor após editá-lo.

## Arquitetura

### Tema local vence o tema-gem

O `Gemfile` traz a gem `jekyll-theme-console` e `_config.yml` declara `theme: jekyll-theme-console`,
mas o repositório também contém cópias locais de `_layouts/`, `_includes/`, `_sass/` e `assets/`.
Arquivos locais têm precedência sobre os da gem, portanto **é sempre nos arquivos locais que se
edita** — mexer no `Gemfile`/gemspec não muda a aparência do site.

Cadeia de layouts: `home`/`page`/`post` → `default.html` → `head.html` + `header.html` + `footer.html`.
`page.html` e `post.html` apenas repassam `{{ content }}`; praticamente todo o conteúdo usa
`layout: page`. Não há posts de blog (`site.posts` está vazio), então o loop em `home.html` não
produz nada.

### Estilos

`_sass/base.scss` concentra a estrutura e as variáveis Sass (fonte, tamanhos, largura do container).
As cores vivem como *CSS custom properties* em `_sass/_dark.scss` e `_sass/_light.scss`.

Existem três entradas locais em `assets/`: `main.scss` (dark + troca automática via
`prefers-color-scheme`), `main-dark.scss` e `main-light.scss` — o build compila também
`main-hacker.scss`, que vem da gem e não existe no repositório. **Qual delas é carregada é decidido
em `_includes/head.html`** a partir de `site.style` e `site.listen_for_clients_preferred_style` no
`_config.yml` — hoje `listen_for_clients_preferred_style: true`, ou seja, `main.css`. Mudanças de
cor precisam ser feitas no `_sass/` correspondente, não nas entradas.

`assets/collapsible.js` implementa blocos `.collapsible`; nenhum layout o inclui no momento.

### Navegação

O menu superior é gerado por `_includes/header.html` iterando `header_pages` do `_config.yml` e
casando cada entrada com `site.pages` pelo campo `path`. Consequências:

- uma página nova só aparece no menu se o caminho do arquivo for adicionado a `header_pages` **e**
  a página tiver `title` no front matter;
- entradas de `header_pages` que não correspondem a nenhum arquivo são ignoradas em silêncio
  (é o caso atual de `git.md`).

### Estrutura de conteúdo

Todas as páginas são Markdown com front matter `layout` + `title` + `permalink`; o `permalink`
explícito é que define a URL (a hierarquia de diretórios sozinha não define).

- `index.md` (`layout: home`), `publications.md`, `teaching.md` — páginas de topo.
- `publications/` — PDFs de teses, linkados de `publications.md`.
- `teaching/<ano>/<disciplina>/<disciplina>.md` — uma página por disciplina, com
  `permalink: /teaching/<ano>/<disciplina>/`, e os PDFs (plano, calendário, listas, provas) ao lado
  no mesmo diretório.
- `teaching/monitoria/` — material antigo de monitoria.

Todos os links para arquivos usam o prefixo `{{site.baseurl}}`, necessário porque a CI faz o build
com `--baseurl` vindo do `actions/configure-pages`.

Idioma: `index.md` e `publications.md` estão em inglês; todo o material de ensino está em português.

### Carimbo de "última atualização"

As páginas de disciplina podem exibir quando foram atualizadas pela última vez e o que mudou, para
que o aluno saiba se vale a pena rebaixar os PDFs. São dois campos opcionais no front matter da
página, renderizados por `_includes/last-updated.html`:

```yaml
last_updated: 2026-08-21 02:52:00 -0300
update_note: Notas da Aula 4 publicadas; Lista 1 e calendário atualizados.
```

O include é chamado explicitamente (`{% raw %}{% include last-updated.html %}{% endraw %}`) no ponto da página em que o
carimbo deve aparecer — hoje, logo abaixo do horário/local. Sem `last_updated` ele não imprime nada,
então páginas que não usam os campos ficam intactas. Ao publicar material novo, atualize os **dois**
campos junto com os PDFs, no mesmo commit.

A hora é formatada pelo filtro `date` do Liquid no fuso de `timezone` do `_config.yml`
(`America/Fortaleza`). Esse ajuste é necessário: sem ele o build da CI, que roda em UTC, mostraria
três horas a mais.

Hoje as páginas de Variável Complexa e de Introdução à Teoria dos Grafos usam os campos.

### Adicionar uma disciplina nova

1. Criar `teaching/<ano>/<disciplina>/<disciplina>.md` copiando uma página existente (ex.:
   `teaching/2026/topologia/topologia.md`) — front matter com `layout: page`, título
   "Nome (ANO.SEMESTRE)" e `permalink` correspondente; seções usuais: horário/local,
   *Informações*, *Listas de problemas*, *Datas das provas*.
2. Colocar os PDFs no mesmo diretório e linkar com `{{site.baseurl}}/teaching/<ano>/<disciplina>/<arquivo>.pdf`.
3. Registrar a página em `teaching.md`, em *Cursos em andamento* ou *Cursos anteriores*, movendo as
   disciplinas encerradas de uma lista para a outra.

Cada arquivo `.md` de disciplina precisa de um `permalink` único: duplicar uma página sem trocar o
`permalink` faz o Jekyll emitir aviso de conflito e publicar apenas uma delas.

## Deploy

`.github/workflows/jekyll.yml` faz build e deploy automáticos a cada push em `master`
(Ruby 3.1, `JEKYLL_ENV=production`, `--baseurl` do `configure-pages`). Não há passo de PR/preview e
`_site/` nunca é commitado (está no `.gitignore`, junto com `Gemfile.lock` e `.jekyll-cache`).
