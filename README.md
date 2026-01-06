# Interactive PCA Biplot – Case Study (Bachelor Thesis)

This repository is a public case study of my bachelor thesis project.
The original implementation was developed in the context of an existing Ruby on Rails web application
and cannot be published here (company/internal repository).

## What I built
- Interactive PCA correlation biplot for repertory-grid data (PC1/PC2)
- Improved readability via label decluttering:
  - heuristic label placement
  - force-based collision avoidance
  - truncation + zoom-based label behavior
- Interactive exploration features:
  - tooltips, hover highlighting, bring-to-front
  - auxiliary/leader lines
  - optional overlays (e.g., density contours / ellipses)
- Evaluation approach comparing readability before/after

## Tech stack used in the original project
Ruby on Rails, JavaScript, D3.js, Alpine.js, PCA, data visualization

## Proof / context
- Bachelor thesis: (link or note: available on request)
- Screenshots: see /assets
- Demo with dummy data: see /demo (reimplemented from scratch)
