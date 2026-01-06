# Project Case Study – Bachelor Thesis (Public Summary)

## Context
This is a public summary of my bachelor thesis project.  
The full implementation was developed in a private corporate GitLab repository and cannot be published.

QMindset is a web application that collects and analyzes Repertory Grid workshop data. The existing visualization used a PCA based heatmap, but it broke down in dense datasets and in multi workshop views.

## Problem
- The existing heatmap suffered from massive label overlaps in dense views, making the center unreadable. In the densest multi workshop scenario, 115 of 129 labels collided at the initial zoom level.
- Visual output was not scale robust across workshops, which made comparisons harder because coordinates and relative geometry were inconsistent.
- The visualization provided limited interpretability and limited interaction, so users had few cues to interpret distances, directions, and clusters correctly.
- It was hard because the UI needed to stay stable during zoom and pan while also solving label collisions without destroying the mapping between point and label.

## My Contribution (what I personally did)
- Designed and integrated an interactive PCA biplot view for QMindset, including interpretation cues like unit circle and explained variance.
- Reworked the PCA pipeline from a covariance approach on raw data to a correlation biplot on z standardized data, including eigenvalue and eigenvector handling and a consistent scaling model.
- Implemented a multi stage label decluttering pipeline: heuristic initialization, force based collision resolution, truncation, and hiding as last resort.
- Implemented zoom behavior that keeps the visualization stable: labels scale with the plot but are also pulled back towards their anchor point, and text detail increases with zoom.
- Built the backend to frontend interface: a compute_pca service returns structured JSON with PCA coordinates, explained variance ratio, and unit circle radius, reused by single and multi workshop views.
- Added user visible analytics overlays and interaction support like tooltips with similarity index, density contours, covariance ellipses, and deviation indicators.
- Defined and ran the evaluation methodology for label collisions and visibility to quantify improvements over the baseline heatmap.

## Solution Overview
- Visualization concept: correlation biplot in the PC1 PC2 plane, constructs as points and elements as normalized variable vectors, with unit circle, sectors, and an interactive legend for interpretability.
- Readability concept: decluttering pipeline with initial placement heuristics, force simulation, truncation, hide on extreme density, plus stable zoom behavior with progressive text detail.
- Interaction concept: details on demand via tooltips, hover highlighting, and layer toggles, with consistent state management across views.
- Evaluation and metrics: measured label collisions and visibility using O, L, r, v, and the combined readability index R = (1 − r) × v across scenarios and zoom levels.

## Tech Stack
- Frontend: D3.js based SVG visualization reused for single and multi workshop views.
- State management: Alpine.js store for toggles and consistent layer states.
- Backend: Ruby on Rails controllers calling a compute_pca service and returning JSON.
- Endpoints: multi workshop recalculation via an AJAX GET endpoint returning refreshed PCA data and analytics.
- Tooling and quality: GitLab workflow with issues and merge requests, plus tests and linting updates during integration.

## Key Features (user visible)
- Users can switch between heatmap and biplot layers and enable or disable overlays like density contours, covariance ellipses, legend, and more via toggles.
- Users can hover points and legend or sector entries to get tooltips and focused highlighting, including similarity cues.
- Users can zoom and pan without visual jumps: label mapping stays stable and label detail increases with zoom.

## Architecture (high level)

High level flow:
- Rails backend provides controllers that call compute_pca.
- compute_pca returns PCA data plus explained variance and unit circle radius as JSON.
- Browser renders with D3, while an Alpine store controls layer states and modes.
- Multi workshop selection triggers a recalculation endpoint, then the frontend redraws the visualization.

## Results / Evidence
- Quantified readability improved across all evaluated scenarios compared to the baseline heatmap.
- In the densest multi workshop view, the combined readability index R at the initial zoom improved from 0.109 to about 0.562 after the new approach.
- Baseline evidence: at initial zoom in the densest scenario, 115 of 129 labels collided.
- The evaluation uses a reproducible collision definition and metrics for comparability across zoom levels and scenarios.
- Optional: before and after screenshots are available in the thesis appendices for the baseline and measured scenarios.

