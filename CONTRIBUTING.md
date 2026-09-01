# Contributing to Health RADAR

Thank you for contributing to Health RADAR.

This guide is the detailed reference for preparing content, working with the website locally and submitting a contribution.

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

- report a bug or broken link
- suggest a new data source
- correct or clarify existing content
- add a data source page, visualisation or modelling example
- suggest improvements to the website

### Report an issue

Search the [existing issues](https://github.com/healthradartool/HealthRADAR/issues) before opening a new one.

For a bug, include:

- the affected page or file
- what you expected to happen
- what happened instead
- steps to reproduce the problem
- a screenshot or error message, when useful

If you are not familiar with GitHub, use the [Health RADAR issues form](https://healthradartool.net/issues.html).

### Suggest a data source

Use the [new dataset issue template](https://github.com/healthradartool/HealthRADAR/issues/new?template=new-dataset-template.md). Describe what the data contains, how it is accessed, its spatial and temporal resolution, and why it is useful for climate-sensitive infectious disease modelling.

You may suggest a dataset without providing code. A clear description is enough for another contributor to help build on.

## Before you start

For a new data source, guide or substantial website change, open an issue before doing extensive work. This gives maintainers an opportunity to confirm the scope, avoid duplicated effort and identify relevant examples.

Small corrections, such as typos and broken links, can go directly to a pull request.

Keep each contribution focused on one data source, guide, bug or improvement. Small changes are easier to review and less likely to conflict with other work.

## How the website is organised

Health RADAR is a [Quarto](https://quarto.org/) website. Content is written in `.qmd` files and most data processing and visualisation examples use R.

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

Start by looking at nearby files that are similar to the change you want to make. For a complete data source example, see the [World Malaria Report page and its supporting files](https://github.com/healthradartool/HealthRADAR/tree/dev/datasources/malaria/who-wmr).

## Setting up the project

### Requirements

Install:

- [Git](https://git-scm.com/)
- [R 4.5.0](https://cran.r-project.org/)
- [Quarto](https://quarto.org/docs/get-started/)
- RStudio or any IDE of your choice.

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
quarto render datasources/malaria/<datasource>/<datsource>.qmd
```

The project uses `freeze: auto`, so unchanged computational output can be reused from `_freeze/`.

If you introduce an R package, install it through `renv` and update the lockfile:

```r
renv::install("package-name")
renv::snapshot()
```

Only add a dependency when it provides a clear benefit that existing project packages do not.

## Development workflow

Health RADAR uses two primary branches:

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

Create a pull request targeting `dev` in `healthradartool/HealthRADAR`. Explain what changed, why it is useful and how you checked it. Link any related issue and include screenshots for visible website changes.

For a small text correction, you may edit the file in GitHub and open a pull request without setting up the project locally. Select `dev` as the base branch.

## Contribute a data source page

Each published data source has its own folder under `datasources/malaria/`. Draft pages may be developed under `datasources/drafts/`.

### Folder structure

Use a short, lowercase folder name. Hyphens are preferred between words.

```text
datasources/malaria/<datasource>/
├── <datasource>.qmd
├── data/
│   ├── README.md
│   └── <small-example-data>
├── images/
│   └── <dataset-thumbnail>.png
└── scripts/                  # Optional access or processing scripts
```

Where possible, avoid adding full source datasets when a small example or subset is sufficient.

### Page metadata

Each page begins with Quarto YAML metadata.

```yaml
---
title: "Data source name"
description: "Short description of the data source"
date: dd/mm/yyyy
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

### Available categories

Categories are grouped by theme, geography, temporal resolution and data type. Choose categories from the lists below rather than inventing new ones, so that filtering and listing pages stay consistent.

- **Theme**
  - Climate
  - Epidemiology
  - Entomology
  - Demography
  - Interventions
  - Drug Resistance
  - Health Systems & Policy
- **Geography**
  - Global
  - Multi-national
  - Country-level
  - Gridded
- **Temporal resolution**
  - Real-time
  - Daily
  - Monthly
  - Annual
  - Historical Archive
- **Data type**
  - Survey / Household
  - Surveillance / Routine
  - Observational / Station
  - Modelled Estimates
  - Reports & Scorecards

If a dataset needs a category that is not listed here, open an issue to discuss adding it before using it on a page.

### Required sections

Data source pages are described using three main tabs:

```markdown
::: {.page-tabs .panel-tabset}

## Overview

## Visualisations

## Modelling

:::
```

#### Overview

Provides a general high-level description of the data including a description of how to access the data.

#### Visualisations

Provide a small set of useful visualisations that show the type and important characteristics of the data. 

#### Modelling

Provide a focused working example showing how the data can support climate-sensitive infectious disease modelling. 

You can learn more in the [page checklist.](https://docs.google.com/spreadsheets/d/1b-RO9O2OIzR6sDSqwMNYvVbRSGYm8KP10gLvhNsgiM8/edit?gid=0#gid=0)

When editing an existing page, preserve its terminology and structure unless changing them is part of the contribution.

## Pull request checklist

Before opening a pull request, confirm that:

- [ ] the changed page renders without errors
- [ ] the complete website is rendered for changes to shared code, configuration or styling
- [ ] links, images, tabs, tables and code examples work
- [ ] no sensitive, restricted or unnecessarily large files are included
- [ ] new R packages are recorded in `renv.lock`
- [ ] changed computed output in `_freeze/` is included when needed
- [ ] generated `_site/` files are not committed
- [ ] the pull request targets `dev` and explains what changed and how it was checked

Maintainers may request changes. Push follow-up commits to the same branch, the pull request will update automatically.

## Getting help

If you are unsure how to approach a contribution, [open an issue](https://github.com/healthradartool/HealthRADAR/issues) and describe your idea. Questions and decisions recorded publicly can also help future contributors.

