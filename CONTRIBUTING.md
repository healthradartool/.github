# Contributing to Health RADAR

Thank you for contributing to Health RADAR.

This guide is the detailed reference for preparing content, working with the website locally, and submitting a contribution.

## Contents

- [Code of Conduct](#code-of-conduct)
- [Ways to contribute](#ways-to-contribute)
- [Before you start](#before-you-start)
- [How the website is organised](#how-the-website-is-organised)
- [Set up the project](#set-up-the-project)
- [Development workflow](#development-workflow)
- [Contribute a data source page](#contribute-a-data-source-page)
- [Visualisation guidance](#visualisation-guidance)
- [R code guidance](#r-code-guidance)
- [Writing and accessibility](#writing-and-accessibility)
- [Pull request checklist](#pull-request-checklist)
- [Getting help](#getting-help)

## Code of Conduct

Health RADAR and everyone participating in it are governed by our [Code of Conduct](https://github.com/healthradartool/.github/blob/main/CODE_OF_CONDUCT.md). By participating, you agree to uphold it.

## Ways to contribute

There are multiple ways to contribute. You can:

- report a bug or broken link;
- suggest a new data source;
- correct or clarify existing content;
- add a data source page, visualisation, or modelling example; or
- suggest improvements to the website's code, Quarto configuration, or styling.

### Report an issue

Search the [existing issues](https://github.com/healthradartool/HealthRADAR/issues) before opening a new one.

For a bug, include:

- the affected page or file;
- what you expected to happen;
- what happened instead;
- steps to reproduce the problem; and
- a screenshot or error message, when useful.

If you are not familiar with GitHub, use the [Health RADAR feedback form](https://healthradartool.net/issues.html).

### Suggest a data source

Use the [new dataset issue template](https://github.com/healthradartool/HealthRADAR/issues/new?template=new-dataset-template.md). Describe what the data contains, how it is accessed, its spatial and temporal resolution, and why it is useful for climate-sensitive infectious disease modelling.

You may suggest a dataset without providing code. A clear description is enough for another contributor to help build on.

## Before you start

For a new data source, guide, or substantial website change, open an issue before doing extensive work. This gives maintainers an opportunity to confirm the scope, avoid duplicated effort, and identify relevant examples.

Small corrections, such as typos and broken links, can go directly to a pull request.

Keep each contribution focused on one data source, guide, bug, or improvement. Small changes are easier to review and less likely to conflict with other work.

## How the website is organised

Health RADAR is a [Quarto](https://quarto.org/) website. Content is written in `.qmd` files, and most data processing and visualisation examples use R.

The main files and folders are:

```text
HealthRADAR/
├── _quarto.yml              # Website configuration
├── index.qmd                # Home page
├── data.qmd                 # Data source listing
├── contribute.qmd           # Short contribution overview
├── datasources/
│   ├── malaria/             # Published data source pages
│   └── drafts/              # Work in progress
├── guides/                  # Climate, entomology, and modelling guides
├── theme_health_radar.R     # Plot theme and colour scales
├── gt_theme_health_radar.R  # Table theme
├── style-base.scss          # Shared website styling
├── renv.lock                # R package versions
├── _freeze/                 # Saved computational output
└── _site/                   # Generated website; do not commit
```

Start by looking at nearby files that are similar to the change you want to make. For a complete data source example, see the [CHIRPS page and its supporting files](https://github.com/healthradartool/HealthRADAR/tree/dev/datasources/malaria/chirps).

## Set up the project

### Requirements

Install:

- [Git](https://git-scm.com/);
- [R 4.5.0](https://cran.r-project.org/);
- [Quarto](https://quarto.org/docs/get-started/); and
- RStudio, or another editor with R and Quarto support.

Pages using spatial R packages may also require system libraries such as `GDAL`, `GEOS`, `PROJ`, and `udunits`.

### Fork and clone

Fork [healthradartool/HealthRADAR](https://github.com/healthradartool/HealthRADAR), then clone your fork:

```bash
git clone https://github.com/<your-username>/HealthRADAR.git
cd HealthRADAR
git remote add upstream https://github.com/healthradartool/HealthRADAR.git
```

Open `HealthRADAR.Rproj` in RStudio or open the `HealthRADAR` directory in your IDE of choice. The project uses [renv](https://rstudio.github.io/renv/) to keep package versions consistent. Restore the packages recorded in `renv.lock`:

```r
renv::restore()
```

The first restore may take some time.

### Preview the website

From the project root, run:

```bash
quarto preview
```

The local preview is available at `http://127.0.0.1:4200`.

Render the complete website without starting a preview server with:

```bash
quarto render
```

You can also render one page while working on it:

```bash
quarto render datasources/malaria/<dataset>/<dataset>.qmd
```

The project uses `freeze: auto`, so unchanged computational output can be reused from `_freeze/`.

## Development workflow

Health RADAR uses two long-lived branches:

- `dev` is the integration branch. Create a feature branch for your work and create a pull request into `dev`.
- `main` contains the published version of the website. Maintainers promote tested changes from `dev` to `main`.

### 1. Update your local `dev` branch

```bash
git checkout dev
git fetch upstream
git merge upstream/dev
```

### 2. Create a branch

Use a short, descriptive name:

```bash
git checkout -b feature/<short-description>
```

Examples include `feature/chirps-page`, `feature/climate-guide`, and `fix/broken-data-link`.

### 3. Make and check your changes

Preview as you work. Commit small, logical groups of changes with clear messages:

```bash
git add <changed-files>
git commit -m "Add CHIRPS data source page"
```

Avoid `git add .` when possible. Staging named files makes it easier to keep generated or unrelated files out of the commit.

### 4. Push and open a pull request

```bash
git push -u origin feature/<short-description>
```

Open a pull request against `healthradartool/HealthRADAR:dev`. Explain what changed, why it is useful, and how you checked it. Link any related issue and include screenshots for visible website changes.

For a small text correction, you may edit the file in GitHub and open a pull request without setting up the project locally. Select `dev` as the base branch.

## Contribute a data source page

Each published data source has its own folder under `datasources/malaria/`. Draft pages may be developed under `datasources/drafts/`.

### Folder structure

Use a short, lowercase folder name. Hyphens are preferred between words.

```text
datasources/malaria/<dataset>/
├── <dataset>.qmd
├── data/
│   ├── README.md
│   └── <small-example-data>
├── images/
│   └── <dataset-thumbnail>.png
└── scripts/                  # Optional access or processing scripts
```

Do not add full source datasets when a small example or subset is sufficient.

### Page metadata

Each page begins with Quarto YAML metadata. Shared layout settings come from `datasources/malaria/_metadata.yml`.

```yaml
---
title: "Dataset name"
description: "Short description of the dataset"
date: 07/31/2026
draft: true
image: "images/dataset-thumbnail.png"

categories:
  - climate
  - gridded
  - rainfall
  - global
  - daily
---
```

Use `draft: true` until the page is ready to appear in the data source listing. Choose a small set of categories that accurately describes the dataset.

### Required sections

Data source pages use three main tabs:

```markdown
::: {.page-tabs .panel-tabset}

## Overview

## Visualisations

## Modelling

:::
```

#### Overview

Help a reader understand and assess the dataset. Include:

- what the dataset contains and why it was created;
- the organisation responsible for it;
- spatial and temporal coverage and resolution;
- variables and units;
- update frequency;
- how to access the data;
- important caveats, strengths, and limitations;
- a suggested citation; and
- licence or terms of use.

Prefer official data pages, documentation, and DOI links.

#### Visualisations

Provide a small set of useful visualisations that show the type and important characteristics of the data. Each visualisation should have a clear title, labelled axes and legend, an informative caption, and a short interpretation in the surrounding text.

#### Modelling

Provide a focused, runnable example showing how the data can support climate-sensitive infectious disease modelling. Explain the purpose of the example, the preparation steps, and what the result means. The aim is to teach a reusable approach rather than present a complete research analysis.

### Data provenance

Every included data file must be reproducible. In `data/README.md`, document:

- the file name and where it is used;
- the original source and download link;
- when the data was accessed;
- filters, subsets, or other processing applied; and
- any restrictions on access or reuse.

Place repeatable download or processing code in `scripts/` when appropriate. Never commit confidential, personally identifiable, or restricted data.

### ABCDE principles

Every data source should be:

- **Accessible:** users can obtain the data from its source.
- **Befitting:** the data is appropriate for climate-sensitive infectious disease modelling.
- **Cited:** citation and attribution information is provided.
- **Documented:** collection methods and the context needed to interpret the data are explained.
- **Exemplified:** practical code shows how the data can be used in a model or modelling workflow.

## Visualisation guidance

Visualisations should help a new user quickly understand the dataset. Aim for a few well-chosen plots rather than many similar ones.

When appropriate, focus examples on malaria in the Elimination 8 countries, particularly the frontline countries: Angola, Mozambique, Zambia, and Zimbabwe. Use other locations when they better demonstrate the dataset.

Use the project helpers for a consistent appearance:

```r
source(here::here("theme_health_radar.R"))

ggplot(data, aes(x, y, colour = group)) +
  geom_line() +
  scale_colour_manual_health_radar() +
  theme_health_radar()
```

Available helpers include:

- `theme_health_radar()`;
- `scale_colour_manual_health_radar()`;
- `scale_fill_manual_health_radar()`;
- `scale_colour_continuous_health_radar()`; and
- `scale_fill_continuous_health_radar()`.

Use `gt_theme_health_radar()` for tables created with [`gt`](https://gt.rstudio.com/).

Write captions so that a copied figure retains its meaning. State what is shown, identify the source, and note any uncertainty or limitation that affects interpretation. Add alt text that communicates the figure's purpose and main information.

## R code guidance

Code examples should be readable by someone who is new to the dataset.

- Use the base R pipe `|>` where practical.
- Use meaningful `snake_case` object names.
- Put one operation on each line and indent continued expressions.
- Add comments that explain decisions or unfamiliar transformations.
- Filter and select data early to keep examples small.
- Prefer focused verbs such as `transmute()` and `pull()` when they clearly express the task.
- Use `pivot_longer()` or `pivot_wider()` when reshaping improves clarity.
- Avoid unnecessary `group_by()` calls when a `.by` argument is clearer.
- Suppress messages and warnings that do not help the reader.
- Remove unused packages, objects, and exploratory code before submitting.

Use existing Health RADAR pages as the first reference for style. For questions not covered here, follow the [tidyverse style guide](https://style.tidyverse.org/).

If you introduce an R package, install it through `renv` and update the lockfile:

```r
renv::install("package-name")
renv::snapshot()
```

Only add a dependency when it provides a clear benefit that existing project packages do not.

## Writing and accessibility

- Write in plain English for modellers and analysts who may be new to the dataset.
- Use British or international spelling, such as *modelling*, *visualisation*, and *colour*.
- Define abbreviations the first time they appear.
- Keep paragraphs short and use descriptive headings.
- Use meaningful link text rather than “click here”.
- Add captions and alt text to images and plots.
- Do not use colour alone to communicate meaning.
- Give tables clear column names and include units where relevant.
- Cite factual claims and use stable sources where possible.

When editing an existing page, preserve its terminology and structure unless changing them is part of the contribution.

## Pull request checklist

Before opening a pull request, confirm that:

- [ ] the change is focused and any related issue is linked;
- [ ] the changed page renders without errors;
- [ ] the complete website is rendered for changes to shared code, configuration, or styling;
- [ ] links, images, tabs, tables, and code examples work;
- [ ] figures have useful captions and alt text;
- [ ] data sources, processing, citations, and terms of use are documented;
- [ ] no sensitive, restricted, or unnecessarily large files are included;
- [ ] new R packages are recorded in `renv.lock`;
- [ ] changed computed output in `_freeze/` is included when needed;
- [ ] generated `_site/` files are not committed; and
- [ ] the pull request targets `dev` and explains what changed and how it was checked.

Maintainers may request changes. Push follow-up commits to the same branch; the pull request will update automatically.

## Getting help

If you are unsure how to approach a contribution, [open an issue](https://github.com/healthradartool/HealthRADAR/issues) and describe your idea. Questions and decisions recorded publicly can also help future contributors.

