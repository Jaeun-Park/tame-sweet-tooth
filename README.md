# Tame the Sweet Tooth — Interactive Nutrition Visualizer

Tame the Sweet Tooth is a Svelte-based data storytelling app that visualizes sugar content in common foods and highlights dietary trade-offs to support healthier decisions among college students.


**2nd Place Winner** — Carnegie Mellon IronViz 2024  
**Role**: Data Analytics Team Lead  
**Live Demo**: [https://jaeun-park.github.io/tame-sweet-tooth](https://jaeun-park.github.io/tame-sweet-tooth)

---

## Project Overview

This project addresses excessive sugar consumption among students, especially those facing food insecurity. It presents an engaging, visual alternative to static nutritional guidelines, using interactive charts to show:

- Sugar levels in snacks and meals
- Gaps in dietary quality
- Practical alternatives aligned with student habits

As the Data Analytics Lead, I built the interactive dashboard to convey nutritional insights, highlight risks of high-sugar diets, and encourage behavior change through intuitive design.

---

## Problem & Solution

**Problem**
College students facing food insecurity often rely on affordable, processed snacks high in added sugar. Healthy eating options are under-communicated, and existing resources lack engagement or personalization.

**Solution**
This project visualizes the nutritional consequences of everyday dietary choices and recommends alternatives with lower sugar impact. Key features include:
- Food Insecurity Context: Situates the problem with comparative data
- Sugar Breakdown by Food Type: Highlights hidden sources of added sugars
- Nutritionally Balanced Alternatives: Offers practical meal-based snack options like kimbap or chia pudding without emphasizing culture, focusing instead on nutrient balance

---


## Project Goals

- Raise awareness of sugar intake in student diets
- Equip CMU Dining Services and student stakeholders with actionable, visual insights
- Provide a replicable model for nutritional storytelling tools

---

## Visual Features

- **D3.js Visualizations**: Interactive charts, hover feedback, transitions
- **Dynamic User Experience**: Scrollable insights and visual narratives
- **Contextualized Health Metrics**: Clear comparison of sugar content across foods

---

## Tech Stack

- **Frontend**: [Svelte](https://svelte.dev/)
- **Build Tool**: [Vite](https://vitejs.dev/)
- **Visualizations**: D3.js (within Svelte components)
- **Deployment**: GitHub Pages via GitHub Actions

---

## Data Sources

- [CMU 31st Annual Food Drive](https://secure.qgiv.com/event/cmu31/)
- [A Decade of College Student Hunger](https://pmc.ncbi.nlm.nih.gov/articles/PMC8913502/)
- [Food Insecurity among U.S. College Students (Scoping Review)](https://www.sciencedirect.com/science/article/pii/S2161831322002599)
- [USDA ERS - Food Security Stats](https://www.ers.usda.gov/topics/food-nutrition-assistance/food-security-in-the-u-s/key-statistics-graphics/#foodsecure)
- [Dietary Guidelines for Americans, 2020-2025](https://www.dietaryguidelines.gov/resources/2020-2025-dietary-guidelines-online-materials)
- [Nutritionix API](https://www.nutritionix.com/food/)
- [Food.com Recipes – Kaggle](https://www.kaggle.com/datasets/irkaal/foodcom-recipes-and-reviews)

---

## Repository Structure
```
tame-sweet-tooth/
├── src/ # Svelte components (main UI logic and visualizations)
├── static/ # Static assets like images or data files
├── .github/workflows/ # GitHub Actions for deployment
├── package.json # Project metadata and dependencies
├── README.md # Project documentation
└── vite.config.js # Vite build configuration
```

---

## How to Run Locally

```bash
# 1. Clone the repository
git clone https://github.com/Jaeun-Park/tame-sweet-tooth.git
cd tame-sweet-tooth

# 2. Install dependencies
npm install

# 3. Run the dev server
npm run dev

# The app will be available at http://localhost:5173
```