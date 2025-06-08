# Tame the Sweet Tooth — Interactive Nutrition Visualizer

This Svelte-based data storytelling app visualizes the sugar content of common foods and compares Korean and American dietary patterns to promote balanced eating.  

**2nd Place Winner** — Carnegie Mellon IronViz 2024  
**Role**: Data Analytics Team Lead  
**Live Demo**: [https://jaeun-park.github.io/tame-sweet-tooth](https://jaeun-park.github.io/tame-sweet-tooth)


---

## Project Overview

This project raises awareness about excessive sugar consumption among college students and promotes healthier dietary alternatives through cultural insights drawn from Korean eating habits.

As the Data Analytics Lead, I developed a dynamic D3-based dashboard to:
- Visualize disparities in food insecurity and dietary patterns
- Compare sugar content across American and Korean food items
- Propose culturally relevant snacks that align with student behavior and health goals

---

## Project Goals

- Raise awareness around excessive sugar intake in U.S. campus diets  
- Inform CMU Dining Services and public audiences with actionable insights and visual narratives

---

## Visual Features

- **Custom D3.js Visualizations** – Interactive bar charts, hover elements, transitions  
- **Data-Driven Storytelling** – Scrollable explanations and embedded insights  
- **Cultural Health Metrics** – Sugar content breakdown by food type and origin

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