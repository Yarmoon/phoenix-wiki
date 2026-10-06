# PhoenixWiki — TTRPG wiki (Quartz 4)

Wiki for the world and rules system **Phoenix** (setting: Годир / Онганмар / Бурне / Чёрный Юг). Built with [Quartz 4](https://quartz.jzhao.xyz) (v4.5.0), deployed on Vercel from GitHub (`Yarmoon/phoenix-wiki`, branch `v4`). Content is **Russian**; keep all wiki text, filenames and note titles in Russian.

## Two kinds of tasks

1. **Add/change content** → edit Markdown in `content/`.
2. **Change the web app** → edit Quartz config/components/styles outside `content/`.

## Deploy

The user's alias `updQuartz` is not defined anywhere findable; it is almost certainly `npx quartz sync` (code: `quartz/cli/handlers.js`, `handleSync`). That command does `git add .` → commit `Quartz sync: <date>` → `git pull origin v4` → `git push -uf origin v4` (force). Vercel rebuilds on push to `v4`.

**Do not commit/push unless asked.** Path used and confirmed working:

1. `git add` only the intended files (leave the user's WIP notes and `CLAUDE.md` out unless asked), commit with a short descriptive message plus the Co-Authored-By line.
2. `git push origin v4` (plain push, no force).
3. `npx quartz sync --no-commit` to sync with the fork (pull + push; commits nothing). Use plain `npx quartz sync` only if the user wants everything in the working tree committed with the date message.

## Content (`content/`) — Obsidian vault

Notes are plain Markdown, written in Obsidian. Conventions:

- **Filename = page title** (no `title:` frontmatter, except `index.md`). Use the exact Russian name, capitalized as in neighboring notes.
- **Links**: `[[Название]]`, or `[[Название|склонённая форма]]` for inflected Russian text (e.g. `[[Иссир|Иссира]]`). Links resolve by "shortest" path, so use just the note name, not the folder.
- **Frontmatter** is rare: only `aliases:` (list of inflected forms, e.g. `aliases: - орков`) so that links in text resolve. Add it to new pages that will often be mentioned in inflected forms.
- **Tone/length**: notes are terse encyclopedic entries. Many are one or two sentences. Match this, don't pad or invent lore. When information is missing, ask.
- Line breaks are hard (HardLineBreaks plugin): a single newline renders as a line break.
- Obsidian syntax is supported (callouts, `![[embed]]`, wikilinks). Tags are rarely used.
- Never touch `content/.obsidian` (ignored by build) or `private/` (ignored).

### Structure

| Folder | Contents |
|---|---|
| `Правила/` | Rules (~127 notes): `Бой`, `Время`, `Даунтайм`, `Заклинания`, `Механики`, `Снаряжение/{Алхимия,Броня,Магические предметы,Оружие}`, `Характеристики`, `Черты`, plus top-level rule pages (Навыки, Монета, Ноша, Орёл, Сторонники) |
| `Лор/` | World (~46): `Артефакты, Гильдии, Земли, Летопись, Народы, Организации, Персонажи` (NPCs), `Персонажи Игроков` (PCs), `Поселения, События, Языки` |
| `Бестиарий/` | Monsters: stat line (`[[Хит поинты]]`, `[[Броня]]`, `Базовая атака`) then description |
| `Дневники/` | Session logs per campaign: `Банда Без Имени` and `Серая Гильдия` (dated `YYYY-MM-DD.md`, header lines `Год: [[145 о.о.Б.\|145]]-й [[Летоисчисление\|от основания Бурне]]` and `Состав группы: [[..]]`), `Чёрный Юг` (numbered titled episodes `NN. Название.md`) |
| `Индексы/` | Hand-maintained lists of links: `Список заклинаний`, `Список снаряжения`, `Список черт`, `Годир`, `Правители и титулы` |
| `Руководства/` | Player guides (Создание персонажа, Прокачка персонажа, `Пути Аартана/`…); `index.md` links to the main ones |

### Adding new info — checklist

1. Put the note in the matching folder (look at siblings for style first).
2. **Update the relevant index** if the category has one (new spell → `Индексы/Список заклинаний`, new feat → `Список черт` under the right `##` section, new gear → `Список снаряжения`, ruler/title → `Правители и титулы`).
3. Link to existing notes for terms (`Grep` the vault for the name/inflections to find the exact note title or alias). Link both ways when sensible (e.g. add the new NPC to its settlement/organization page).
4. Don't create notes for unlinked-to-nothing stubs unless asked; broken `[[links]]` render as dead links, so check target names exist.
5. Rules text: keep the system's established terms (Бросок Фортуны, Орёл, Очки действий, Преимущество/Помеха…) exactly as they are in `Правила/`.

## Web app (outside `content/`)

- `quartz.config.ts` — site config. pageTitle "PhoenixWiki", locale `ru-RU`, custom dark-orange palette (same colors in light and dark mode), plugins (HardLineBreaks, ObsidianFlavoredMarkdown, CustomOgImages, …). `baseUrl` is still the Quartz default (`quartz.jzhao.xyz`); sitemap/RSS/OG use it.
- `quartz.layout.ts` — page layout; list pages use a custom Explorer sort (folders first, localeCompare numeric).
- `quartz/` — Quartz source (components, plugins, styles in `quartz/styles/`, `quartz/components/styles/`). Prefer config/layout/custom SCSS changes over editing core files, to keep upstream updates mergeable.
- `vercel.json` — `cleanUrls: true`. `.github/` workflows are upstream Quartz leftovers (guarded by `github.repository == 'jackyzha0/quartz'`, so they don't run here).
- Not tracked: `node_modules`, `public/` (build output), `.quartz-cache`.

Commands (Node ≥ 20, npm ≥ 9.3.1):

```bash
npx quartz build --serve     # local preview at http://localhost:8080
npx quartz build             # build into public/
npm run check                # tsc + prettier check
```

For web-app changes, verify with a local build/preview (and look at it in the browser pane) before telling the user to run `updQuartz`.

## Working notes

- Environment is Windows; paths contain Cyrillic. `git status` shows them octal-escaped (`git -c core.quotepath=off status` for readable output).
