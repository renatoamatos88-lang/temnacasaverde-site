# Tem na Casa Verde — Site Institucional & Diretório

Este arquivo é carregado automaticamente em toda conversa com o Claude neste
projeto. Serve como memória persistente e guia rápido.

## O que é
Site institucional + diretório de negócios da **Casa Verde** (São Paulo, Zona Norte).
Projeto irmão do **Tem no Limão** (`www.temnolimao.com.br`), reaproveitando
o mesmo formato estático para captar negócios e validar o modelo no novo bairro
enquanto a plataforma definitiva (v2 — Next.js + Supabase) é construída em paralelo.

## Status
- **Fase:** v1 — site estático monolítico, captura amadora de mercado
- **Vendas:** manuais, via Instagram (sem pagamento automático)
- **Conteúdo:** começa vazio, será cadastrado conforme bairro for sendo mapeado

## Stack atual (fase 1 — site estático)
- **HTML puro** em um único arquivo monolítico: `index.html` (~1.9 MB)
- Sem build tools, sem framework, sem Node no client
- CSS e JS **inline** dentro do `index.html`
- Páginas auxiliares: `noticias.html`, `podcast.html`, `vagas.html`
- Imagens otimizadas em **WebP** via `sharp-cli`
- Deploy: Git push → GitHub → Vercel (auto-deploy em ~30s)

## Repositório & hospedagem
- GitHub: a definir (criar repo `temnacasaverde-site` em https://github.com/renatoamatos88-lang)
- Host: Vercel (a configurar após criar o repo)
- Domínio: a definir (sugestão: `www.temnacasaverde.com.br`)
- SSL: automático (Let's Encrypt via Vercel)

## Fluxo de deploy
```bash
git add .
git commit -m "tipo: descrição curta"
git push
# Vercel detecta e publica em ~30s
```

## Convenções de commit
- Idioma: **português**
- Prefixos: `feat:`, `fix:`, `copy:`, `refactor:`, `docs:`, `chore:`

## Área admin
- Acesso: adicionar `/#admin` ao final da URL
- **Não é login real** — é um lock client-side para edições locais
- Persistência via `localStorage` do navegador
- **DB_KEY:** `tncv_db` (diferente do TNL para evitar conflito de localStorage)
- **SENHA padrão:** `casaverde2026` (TROCAR antes de usar em produção)

## Diferenças do TNL (importante!)
Este projeto começou como cópia do `temnolimao-site`. Os pontos onde os dois
**não devem** ser sincronizados automaticamente:
- `DB_KEY` — `tncv_db` aqui, `tnl_db` no TNL
- `SENHA_KEY` — `tncv_senha` aqui, `tnl_senha` no TNL
- Conteúdo dos arrays (negócios, notícias, vagas, podcasts, depoimentos)
- Textos institucionais (Quem Somos, hero, footer)
- Cores/identidade visual (atualmente iguais, mas podem divergir)

Mudanças estruturais úteis (correções de bug, melhorias de layout) **podem** ser
replicadas manualmente entre os dois projetos durante a fase v1.

## Convenções de imagem
- **Sempre WebP** via `sharp-cli`:
  ```bash
  sharp -i "original.png" -o "nome-kebab-case.webp" -f webp -q 82
  ```
- PNG/JPG originais vão para o `.gitignore`
- Nomes em `kebab-case` sem espaços nem acentos

## Próxima fase (v2) — stack definitiva
Quando a v2 (Next.js + Supabase + Stripe) estiver pronta, este site deixa de
existir como projeto separado e vira um **workspace "Casa Verde"** dentro da
plataforma única. Todo o conteúdo cadastrado aqui será exportado e migrado
para o Supabase via seed script. Nada do trabalho de captura é perdido.

## Documentação
- [`docs/ARQUITETURA.md`](docs/ARQUITETURA.md) — como o site funciona por dentro
- [`docs/GUIA-EDICAO.md`](docs/GUIA-EDICAO.md) — como adicionar/editar conteúdo
- [`docs/CLONAR-PARA-CASA-VERDE.md`](docs/CLONAR-PARA-CASA-VERDE.md) — guia de clonagem (foi assim que esse projeto nasceu)
- [`docs/ROADMAP.md`](docs/ROADMAP.md) — plano de evolução
