
> [!IMPORTANT]
> Read this first! Your vault needs to have the right templates and plugins for coding to work.

This guide walks through creating a fresh Obsidian vault that replicates the thematic analysis workspace used in this workshop — including folder structure, the Auto Note Mover plugin, two Bases, and the code and theme note templates.

---

## 1. Create the Vault

1. Open Obsidian.
2. Click **Open another vault** (bottom-left vault icon) → **Create new vault**.
3. Give it a name (e.g. `Thematic analysis`) and choose a location.
4. Click **Create**.

---

## 2. Create the Folder Structure

Inside the vault, create the following folders. You can do this via **File → New folder** or by right-clicking in the file explorer pane.

```
Bases/
Templates/
Analysis/
Analysis/Codes/
Analysis/Themes/
Analysis/Transcripts/
```

- **Bases/** — stores your `.base` database views
- **Templates/** — stores note templates
- **Analysis/Codes/** — where individual code notes will live
- **Analysis/Themes/** — where theme notes will live
- **Analysis/Transcripts/** — where your source material (transcripts, documents, etc.) lives

---

## 3. Create the Templates

Go to **Settings** → **Core plugins** → enable **Templates** if it is not already on. Then set the template folder to `Templates`.

> [!NOTE]
> If you use the Templater community plugin instead, the process is the same but you set the folder under **Settings → Templater**.

### 3.1 Code Template

Create a new note at `Templates/Thematic analysis code.md` and paste in the following:

```markdown
---
aliases:
tags:
  - data/analysis/semanticcode
source:
extract:
context:
collection:
themes:
---
```


> [!INFO] Switching to and from 'source mode'
> It will be easier to past in the properties above if you switch to 'source mode' temporarily. Click the ellipsis at the top right of your new note and select 'source mode', then paste the YAML code in. You can then select 'source mode' again and the YAML will display as Obsidian properties.


> [!WARNING] YAML must start and end with `---`
> If your properties aren't appearing properly, make sure you have included the three dashes (`---`) at the top and the bottom of the code block. Obsidian needs those to recognise it as YAML. If you're still having trouble, make sure the first set of dashes is at the first line of the note, under the title.


**Guide to template properties:**

|Field|Purpose|
|---|---|
|`source`|The document, interview, or data item the code came from|
|`extract`|The verbatim or paraphrased text being coded|
|`context`|Brief note on the surrounding context|
|`collection`|Optional grouping label (e.g. interview round, document set)|
|`themes`|Link(s) to theme notes — e.g. `[[Theme A]]`|

When you save a note using this template, Auto Note Mover will detect the `data/analysis/semanticcode` tag and move the note into the `Codes/` folder automatically.

### 3.2 Theme Template

Create a new note at `Templates/Thematic analysis theme.md` and paste in the following:

```markdown
---
aliases:
tags:
  - data/analysis/theme
description:
sub-theme:
---

## Relevant Codes

![[Semantic codes.base#Relevant codes]]
```

The embedded Base (`![[Semantic codes.base#Relevant codes]]`) will display all code notes that link back to this theme note — giving you a live, filtered view of evidence for each theme.

When saved, Auto Note Mover will move this note into `Themes/` automatically.

---

## 3. Install and Configure Auto Note Mover

Auto Note Mover automatically files new notes into the correct folder based on their tags — essential for keeping codes and themes organised without manual drag-and-drop.

### 3.1 Install the Plugin

1. Go to **Settings** → **Community plugins**.
2. If prompted, click **Turn on community plugins** to disable restricted mode.

> [!WARNING]
> Obsidian enables restricted mode by default. You must turn it off to install community plugins.

3. Click **Browse** and search for **Auto Note Mover**.
4. Click **Install**, then **Enable**.

### 3.2 Configure Auto Note Mover Rules

1. Go to **Settings** → **Auto Note Mover**.
2. Under **Rules**, add the following two rules by clicking the **+** button for each:

| Rule | Tag                          | Destination folder |
| ---- | ---------------------------- | ------------------ |
| 1    | `data/analysis/semanticcode` | `Codes`            |
| 2    | `data/analysis/theme`        | `Themes`           |

3. Make sure **Trigger** is set to **Automatic** so notes move as soon as you save them with the relevant tag.
4. Next to **Add excluded folder**, click the + button and enter `Templates`. This will ensure that Auto Note Mover will ignore files in that folder (we don't want our template files accidentally being moved into our analysis area!)

---

## 5. Create the Bases

Bases are Obsidian's built-in database views (available from Obsidian 1.8+). They query your vault's note properties to build dynamic tables.

> [!WARNING] This is the slightly complex bit!
> You'll have to follow the instructions carefully to make sure the bases and filters all work properly.

### 5.1 Semantic Codes Base

1. Right-click the `Bases/` folder → **New base** (or use **File → New base**).
2. Name it `Semantic codes`.
3. **Create the main view:**
	1. Click the `Filter` button
	2. Click the triangle to expand `All views`
	3. Next to `Where`, click `File` then `Has tag` then select `data/analysis/semanticcode` (the tag you created previously). ==You should see the template you created appear==. 
	4. Click `Add filter` (make sure you do it in `All views`, not `This view`)
	5. Next to `Where`, click `File` then `Is not in folder` then select `Templates`. ==This excludes the template file from your list==.
	6. Click on the `View` dropdown, then click the arrow on the right to rename it. 
	7. Rename the view to `All codes`.
4. **Create the `Relevant codes` view:**
	1. Click on the `View` dropdown and click `Add view`. 
	2. Using the steps above, rename the view `Relevant codes`. 
	3. Click `Filter`. 
	4. Under `This view` (==not `All views`!==), click the `Advanced filter` button (it looks like `</>').
	5. Paste in the following: `file.hasLink(this.file)`.
5. **Create the `Theme development` view:**
	1. Click on the `View` dropdown and click `Add view`. 
	2. Using the steps above, rename the view `Theme development`. 
	3. Click `Properties`. 
	4. Start typing 'Theme' in the search bar. When you see the 'Theme' property appear, check the box next to it. 

**What each view does:**

| View                  | Purpose                                                                 |
| --------------------- | ----------------------------------------------------------------------- |
| **All codes**         | Every code note in the vault                                            |
| **Relevant codes**    | Codes that link to _this_ note — used when embedded inside a theme note |
| **Theme development** | Flat list for working through code → theme relationships                |

> [!NOTE] Note on the `Relevant codes` view
> This view uses `file.hasLink(this.file)`, which means it only makes sense when the Base is _embedded inside another note_ (a theme note). When you open the Base directly, `this.file` has no referent, so the view will appear empty — that is expected behaviour.

### 5.2 Semantic Themes Base

1. Create a new base in `Bases/` named `Semantic themes`.
2. **Create the main view:**
	1. Click the `Filter` button
	2. Click the triangle to expand `All views`
	3. Next to `Where`, click `File` then `Has tag` then select `data/analysis/theme` (). ==You should see the template you created appear==. 
	4. Click `Add filter` (make sure you do it in `All views`, not `This view`)
	5. Next to `Where`, click `File` then `Is not in folder` then select `Templates`. ==This excludes the template file from your list==.

This gives you a simple table of all theme notes — useful for getting an overview of your emerging themes.

---
## 6. Create keyboard shortcuts

Go to Obsidian settings and select 'Hotkeys'. Use the search filter to find the 'Insert footnote' command. Give it a shortcut you will remember. I chose `Cmd+Option+Shift+F`.

Do the same for 'Templates: Insert template'. I chose `Cmd+Option+T`.

## 7. Verify the Setup

Run through this checklist before you start coding:

- [ ] `Bases/`, `Templates/`, `Codes/`, `Themes/`, and `Transcripts/` folders exist
- [ ] Auto Note Mover is installed and enabled (restricted mode is off)
- [ ] Auto Note Mover has two rules: `data/analysis/semanticcode` → `Codes/` and `data/analysis/theme` → `Themes/`
- [ ] `Templates/Thematic analysis code.md` exists with the correct frontmatter
- [ ] `Templates/Thematic analysis theme.md` exists with the correct frontmatter and embedded Base
- [ ] `Bases/Semantic codes.base` exists with all four views
- [ ] `Bases/Semantic themes.base` exists

### Quick test

1. Create a new note and apply the **Thematic analysis code** template.
2. Fill in `source`, `extract`, and `context`.
3. Save — the note should move to `Codes/` automatically.
4. Create a theme note using the **Thematic analysis theme** template.
5. In the code note, add a link to your theme note in the `themes:` field (e.g. `themes: "[[My First Theme]]"`).
6. Open the theme note — the embedded **Relevant codes** Base should display your code note.

---

## 7. Workflow Summary

```
New note → apply code template → fill frontmatter → save
  → Auto Note Mover files it in Codes/
  → Link it to a theme note via the `themes:` property
  → Theme note's embedded Base updates automatically
  → Semantic codes.base shows all codes grouped by theme
```

---

_Created for the ResBaz 2026 workshop: Thematic Analysis Using Obsidian — Thursday 25 June, 11am–12:30pm, Room KG-5151._
