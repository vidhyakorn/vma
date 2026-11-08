---
title: "Modelling & Simulation"
description: "Agent-based models, system dynamics, and simulation tools that operationalise the NDLS framework and support CARP estimation."
weight: 2
feedExclude: true
---

## Modelling & Simulation

This thread develops the **computational tools and formal models** that turn NDLS and CARP from conceptual frameworks into empirically testable and policy-useful instruments.

### System dynamics core

The NDLS framework describes how biological, cognitive, social and environmental components co-evolve over a person's lifespan.  A system-dynamics model captures that co-evolution as a set of coupled differential equations, making feedback loops explicit and allowing scenario analysis — *What happens to flourishing if early support is delayed by two years?  What if housing stability is improved but employment access is not?*

### Agent-based extension

Where system dynamics averages across a population, agent-based models preserve individual heterogeneity.  Agents in the NDLS-ABM are persons with NDCs whose attributes (sensory profile, executive function, social network, access to reasonable accommodations) are sampled from survey distributions.  Emergent population patterns — clustering of unmet need, tipping points in community inclusion — appear without being programmed in directly.

### Calibration and uncertainty

Both model types require empirical calibration against Thai cohort data (MORU), Thai disability surveys, and published NDC epidemiology.  Uncertainty propagation (Monte Carlo, partial-rank correlation) produces credible intervals around model outputs that are honest about what is known and what is assumed.

### Tools in development

| Tool | Description | Status |
|---|---|---|
| `ndls-sd` (R) | System-dynamics solver for the NDLS core model | Prototyping |
| `ndls-abm` (Python / Mesa) | Agent-based extension with heterogeneous agents | Design phase |
| CALY weight estimator (R) | Person-trade-off weighting + uncertainty propagation | Early development |
| Scenario dashboard | Shiny app for policy scenario exploration | Planned |

### Role in the portfolio

Modelling & Simulation is the **methodological spine** of the research portfolio: it takes theoretical constructs from [NDLS](/ndls/) and measurement outputs from [CARP](/research/projects/carp/), combines them into forward-looking scenario models, and feeds back insights that refine both frameworks.
