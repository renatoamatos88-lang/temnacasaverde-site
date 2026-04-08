# Portal Casa Verde — Site Institucional & Diretório

Este arquivo é carregado automaticamente em toda conversa com o Claude neste
projeto. Serve como memória persistente e guia rápido.

## O que é
Site institucional + diretório de negócios do bairro **Casa Verde**
(São Paulo, Zona Norte), publicado como **Portal Casa Verde**.
Projeto irmão do **Tem no Limão** (`www.temnolimao.com.br`), reaproveitando
o mesmo formato estático para captar negócios e validar o modelo no novo
bairro enquanto a plataforma definitiva (v2 — Next.js + Supabase) é
construída em paralelo.

## Status
- **Fase:** v1 — site estático monolítico, captura amadora de mercado
- **Marca:** `Portal Casa Verde` (mudou de "Tem na Casa Verde" em abril/2026)
- **Hero:** novo hero animado full-bleed (`.cv-hero`) — parallax, blobs,
  gradient text, line reveal, contador. Carrossel A/B antigo foi removido.
- **Conteúdo:** 7 negócios reais pré-cadastrados em `_negociosDefault`
  (Panificadora Nova Zilda, Bar do Plínio, Churrasquinho Mu, Reims,
  Leão XIII, Panni, Av. Braz Leme). Resto do bairro a ser mapeado.
- **Vendas:** manuais, via Instagram (sem pagamento automático)

## Stack atual (fase 1 — site estático)
- **HTML puro** em um único arquivo monolítico: `index.html` (~800 KB)
- Sem build tools, sem framework, sem Node no client
- CSS e JS **inline** dentro do `index.html`
- Páginas auxiliares: `noticias.html`, `podcast.html`, `vagas.html`
- Imagens otimizadas em **WebP** via `sharp-cli` (quando houver arquivos locais;
  hoje hero e Quem Somos usam Unsplash via hot-link)
- Deploy: Git push → GitHub → Vercel (a ser configurado)

## Repositório & hospedagem
- **GitHub:** https://github.com/renatoamatos88-lang/temnacasaverde-site
- **Branch principal:** `master` (trabalhar direto, sem feature branches
  nem worktrees — ver seção "Fluxo de trabalho" abaixo)
- **Host:** Vercel (ainda não configurado)
- **Domínio:** a definir

## Fluxo de trabalho (IMPORTANTE)
**Trabalhar direto no `master`, sem worktrees nem feature branches.**

Tentativas anteriores de usar `git worktree` causaram descompasso entre
o arquivo servido pelo preview e o arquivo editado, gerando bugs invisíveis.
O repositório já teve a worktree `strange-lehmann` removida.

Fluxo padrão:
```bash
# editar arquivo
git add <arquivos>
git commit -m "tipo: descrição curta"
git push origin master
```

### Atenção a line endings
Os arquivos HTML estão em **CRLF** no disco. Qualquer script de
substituição (Node/Python) que use regex precisa de `\r?\n` em vez de
`\n`, senão a substituição falha silenciosamente.

### Edições grandes no `index.html`
O arquivo tem ~800 KB e contém imagens base64 inline, o que estoura o
limite de tokens da ferramenta `Read`. Estratégias que funcionam:
- `Grep` com padrões específicos para localizar trechos
- Scripts Node (`node _patch.mjs`) para substituições complexas
- `Edit` só quando o trecho a substituir é pequeno e único

## Convenções de commit
- Idioma: **português**
- Prefixos: `feat:`, `fix:`, `copy:`, `refactor:`, `docs:`, `chore:`

## Área admin
- Acesso: adicionar `/#admin` ao final da URL
- **Não é login real** — é um lock client-side para edições locais
- Persistência via `localStorage` do navegador
- **DB_KEY:** `tncv_db` (diferente do TNL para evitar conflito de localStorage)
- **SENHA_KEY:** `tncv_senha`
- **SENHA padrão:** `casaverde2026` (TROCAR antes de usar em produção)

## Identidade visual Casa Verde
Paleta aplicada em CSS vars no `:root`:
- `--dark: #0F1F12` · `--dark2: #182A1B`
- `--limao: #5BB85C` (primary verde folha)
- `--limao-dark: #3E8E41` · `--limao-light: #D7EFD9`
- `--amarelo: #F4B400` · `--accent: #E26B3A` (terracota)
- `--cream: #F5F1E6`

Nomes de variáveis mantêm `--limao*` por herança do TNL — **não renomear**
sem refatoração em cascata. Os valores é que mudaram.

## Hero novo (`.cv-hero`)
Markup e estilos com prefixo `.cv-*`. Principais pontos:
- `align-items: flex-start` (conteúdo ancorado no topo, não centralizado)
- `padding-top: 9rem` no `.cv-hero-inner` para ficar acima da nav fixa (~91px)
- `min-height: min(100vh, 780px)` para não esticar demais em telas altas
- Parallax via `requestAnimationFrame` no `scroll`
- Reveal on scroll via `IntersectionObserver` + `[data-reveal]`
- Contador animado nos stats via `data-count`
- Respeita `prefers-reduced-motion`

## Diferenças do TNL (importante!)
Este projeto começou como cópia do `temnolimao-site`. Pontos onde os dois
**não devem** ser sincronizados automaticamente:
- `DB_KEY` — `tncv_db` aqui, `tnl_db` no TNL
- `SENHA_KEY` — `tncv_senha` aqui, `tnl_senha` no TNL
- Hero: aqui é o `.cv-hero` novo; no TNL é o carrossel A/B antigo
- Paleta: aqui é verde Casa Verde; no TNL é o lime original
- Marca: aqui é "Portal Casa Verde"; no TNL é "Tem no Limão"
- Conteúdo dos arrays (negócios, notícias, vagas, podcasts, depoimentos)
- Textos institucionais (Quem Somos, hero, footer)

Correções de bug estrutural **podem** ser replicadas manualmente entre os
dois projetos durante a fase v1.

## Convenções de imagem
- **Sempre WebP** via `sharp-cli`:
  ```bash
  sharp -i "original.png" -o "nome-kebab-case.webp" -f webp -q 82
  ```
- PNG/JPG originais vão para o `.gitignore`
- Nomes em `kebab-case` sem espaços nem acentos
- Hero e Quem Somos atuais usam URLs do Unsplash (transitório — trocar
  por arquivos locais quando houver Fase 3 do admin com upload)

## Preview local
- Config em `.claude/launch.json` → `Static Server (npx serve)` na porta 3000
- Sirva sempre da raiz do projeto (`.`), **nunca** de worktree
- `npx serve` cacheia arquivos em memória — se editar e não ver mudança,
  reinicie o server (`preview_stop` + `preview_start`)

## Próxima fase (v2) — stack definitiva
Quando a v2 (Next.js + Supabase + Stripe) estiver pronta, este site deixa
de existir como projeto separado e vira um **workspace "Casa Verde"** dentro
da plataforma única. Todo o conteúdo cadastrado aqui será exportado e
migrado para o Supabase via seed script. Nada do trabalho de captura é
perdido.

## Documentação
- [`docs/ARQUITETURA.md`](docs/ARQUITETURA.md) — como o site funciona por dentro
- [`docs/GUIA-EDICAO.md`](docs/GUIA-EDICAO.md) — como adicionar/editar conteúdo
- [`docs/CLONAR-PARA-CASA-VERDE.md`](docs/CLONAR-PARA-CASA-VERDE.md) — guia de clonagem (foi assim que esse projeto nasceu)
- [`docs/ROADMAP.md`](docs/ROADMAP.md) — plano de evolução
