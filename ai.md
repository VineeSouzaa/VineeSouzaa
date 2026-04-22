# ai.md — Como este README foi gerado

Documento de processo. Serve para reproduzir e **incrementar** a análise no futuro, quando mais repositórios estiverem acessíveis (mobile, projetos antigos com Laravel/Angular/etc.).

---

## 1. Objetivo

Gerar um `README.md` de perfil GitHub **fiel às habilidades reais** do Vinícius, baseado em **evidência verificável** (código + commits), sem inventar stack por marketing.

Regra de ouro: **se não aparece em commit, não entra no README**. Exceção única: histórico de carreira ("desde 2018") que não dá pra verificar localmente mas não tem por que duvidar.

---

## 2. Fontes analisadas (iteração 1 — 2026-04-22)

Repositórios inspecionados em `/home/viniciussouza/Projects/`:

| Repo | Papel | Commits do Vinícius | Stack dominante |
|---|---|---:|---|
| `ruk/simba-core` | Backend principal (Ruk) | ~332 | NestJS 11, Apollo GraphQL, Prisma 6, PostgreSQL, Redis, Stripe, AWS S3/SNS, OpenTelemetry |
| `ruk/simba-sign-server` | Backend de assinatura | 24 | NestJS, BullMQ/SQS/Kafka, Puppeteer, pdf-lib, Langchain Groq |
| `ruk/simba-login-web` | Frontend (auth/hub) | ~93 | Next.js 16, React 19, Apollo Client, Radix UI, Tailwind 4, next-auth |
| `ruk/simba-sign-web-v2` | Frontend (document signing) | ~112 | Next.js 16, TipTap, CKEditor 5, DnD Kit, Apollo, Chromatic, Storybook |
| `AI/ai-solution-analysis` | Knowledge base pessoal | — | ADRs, viability analysis, viewer Node.js |
| `tests/hocuspocus`, `tests/yjs-demos` | Sandbox pessoal | — | Pesquisa em colaboração real-time (Yjs/Hocuspocus) |

**Autor oficial:** `vine.vsb3@gmail.com` (também usado como `VineeSouzaa` e `vinicius souza`). Todos os três nomes foram somados nas contagens.

---

## 3. Método (replicável)

Para cada repositório:

### 3.1. Mapear stack real
```bash
cat package.json   # dependências reais, não listadas em README
```

### 3.2. Medir participação do Vinícius
```bash
git shortlog -sne --all                       # ranking geral de autores
git log --oneline | wc -l                     # total de commits
git log --author="vine.vsb3\|vinicius\|VineeSouzaa" --oneline | wc -l
```

### 3.3. Entender o *tipo* de trabalho dele
```bash
# Categorias de commit (feat/fix/refactor por escopo)
git log --author="vine.vsb3\|vinicius\|VineeSouzaa" --pretty=format:"%s" \
  | awk -F: '{print $1}' | sort | uniq -c | sort -rn | head -15

# Pastas mais tocadas (onde ele realmente mora no código)
git log --author="vine.vsb3\|vinicius\|VineeSouzaa" --name-only --pretty=format: \
  | grep -v "^$" | awk -F/ '{print $1"/"$2"/"$3}' | sort | uniq -c | sort -rn | head -20

# Linha do tempo
git log --author="vine.vsb3" --reverse --pretty=format:"%ad %s" --date=short | head -3
git log --author="vine.vsb3" --pretty=format:"%ad %s" --date=short | head -5
```

### 3.4. Cruzar com dependências do `package.json`
Só confirmar uma tech (ex.: "Kafka") se **aparecer em `package.json` E houver commit do Vinícius tocando código relacionado**. Dependência existir sozinha não basta — projetos compartilhados têm libs que outros colegas usam.

---

## 4. Decisões tomadas na iteração 1

### Entrou (evidência forte)
- **NestJS, TypeScript, Node.js, GraphQL (Apollo), Prisma, PostgreSQL, Redis** → todos os 4 simba-* usam; Vinícius tem commits pesados em `src/infrastructure/http`, `useCases`, `domain/entities`.
- **Next.js, React, Apollo Client, Tailwind, Radix UI, React Hook Form, Zod** → `simba-login-web` + `simba-sign-web-v2`.
- **TipTap** → 45+ commits pessoais em `simba-sign-web-v2` (especialidade clara).
- **Clean Architecture / DDD** → pastas `domain/application/infrastructure` são onde ele vive, não decoração.
- **Pagamentos (Pix, Itaú, Maxipago, Stripe, PagBank, bolecode, antifraude)** → domínio de negócio visível nos escopos dos commits (`feat(payment)`, `fix(itau webhook)`, etc.).
- **Docker, Jest, GitHub Actions, pnpm, Linux, AWS S3** → visível em configs e commits.

### Saiu (sem evidência nos repos atuais)
| Tech do README antigo | Motivo |
|---|---|
| Laravel, Laravel Blade | Nenhum repo PHP analisado |
| Angular | Nenhum repo Angular analisado |
| React Native, Flutter, Ionic | Nenhum repo mobile analisado |
| .NET | Estava listado em "Frontend" no antigo — estranho e sem evidência |
| MariaDB, Oracle | Só PostgreSQL aparece |
| Java | Pedido explícito do usuário: era teste 100% IA |

**Importante:** "saiu" ≠ "não sabe". Saiu porque não tínhamos repo onde comprovar. Quando surgir repo mobile/Laravel/Angular, ver §5.

---

## 5. Próxima iteração — como re-incluir stacks

Quando o Vinícius liberar mais repos, rodar o método da §3 em cada novo diretório e **re-adicionar ao README** se:

1. Houver `package.json` / `composer.json` / `pubspec.yaml` / `.csproj` com a stack, **E**
2. Vinícius tiver commits reais tocando código-fonte (não só config/README), **E**
3. A quantidade de commits justificar a menção (heurística: >10 commits de `feat`/`refactor` no core do projeto).

### Stacks em "lista de espera" (prováveis, só faltou repo)
- **React Native / Flutter / Ionic** → mobile
- **Laravel (+ Blade)** → backend PHP histórico
- **Angular** → frontend histórico
- **MariaDB / Oracle** → bancos de projetos anteriores
- **Kafka** → está nas deps de `simba-sign-server` mas precisa confirmar se o Vinícius tocou código de producer/consumer

### Stacks já confirmadas mas que podem ganhar destaque
- **Hocuspocus / Yjs** → se virar feature em produção (hoje é só sandbox em `tests/`)
- **Langchain / Groq** → aparece em `simba-sign-server/package.json`, checar se Vinícius mexeu
- **OpenTelemetry** → presente em todos os backends, vale citar se ele configurou pipelines

---

## 6. Estrutura do README (por que ficou assim)

O README antigo era uma **lista de badges de tecnologia**. Recrutador técnico bate o olho e não descobre nada sobre você — só que você já ouviu falar de muita coisa.

A versão nova tem três blocos que respondem às perguntas de um tech recruiter:

1. **"What I actually ship"** → o que você *entrega*, não o que você *sabe*. Respostas: pagamentos, editor TipTap, GraphQL, Clean Architecture. Isso diferencia.
2. **"How I work"** → disciplina de engenharia (conventional commits, semantic-release, testes, lint pesado). Poucos candidatos mostram isso e é altamente valorizado.
3. **"Tech I use day-to-day"** → badges, mas só das coisas verificadas. Sem inflação.

---

## 7. Reiteração contínua

Este `ai.md` deve ser **atualizado a cada nova análise**. Formato sugerido:

```
## Iteração N — AAAA-MM-DD
- Repos novos analisados: ...
- Stacks adicionadas ao README: ...
- Stacks removidas: ...
- Por quê: ...
```

A regra de ouro (§1) nunca muda: **evidência em código, não promessa**.
