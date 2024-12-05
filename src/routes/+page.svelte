<script>
    import CelebratingGiving from '$lib/CelebratingGiving.svelte';
    import FoodInsecurityChart from '$lib/FoodInsecurityChart.svelte';
    import NutritionIntakeChart from '$lib/NutritionIntakeChart.svelte';
    import SugarRecipesTreemap from '$lib/SugarRecipesTreemap.svelte';
    import NutritionComparison from '$lib/NutritionComparison.svelte';
</script>

<style>
    @import url("$lib/global.css");
</style>

<hr class="divider">
<h1 class="project-title">Let's Tame the Sugar Tooth</h1>
<h2 class="project-subtitle">Healthier Snack Choices for a Balanced Campus Diet</h2>
<hr class="divider">


<div class="first-content">
    <h2>Tis A Season of Giving</h2>
    <p>
        Christmas is approaching, and the air is filled with the warmth of giving and sharing.
        Recently, Carnegie Mellon University celebrated a significant achievement—successfully meeting its goal for the annual food drive.
        This milestone reflects the incredible generosity and compassion of the CMU community, as students, faculty, and staff united to support those in need across the Greater Pittsburgh area.
    </p>

    <CelebratingGiving />
</div>

<div class="second-content">
    <h2>Food Insecurity: A Growing Concern</h2>
    <p>
        But as a new international student from a country where the cost of living is lower,
        I can’t help but notice another side of the story.
        <br>While the spirit of giving is alive and well, the reality for many CMU students is starkly different.
        I’ve seen classmates skipping meals or making do with insufficient food, and I can’t shake the question:
        <strong>Why are so many struggling to feed themselves?</strong>
    </p>

    <p>The statistics paint a sobering picture.</p>
    <p>While food insecurity in U.S. households has improved over recent years, college students have not shared in this progress.
        In fact, the opposite is true—research reveals that food insecurity among students has worsened significantly, even as conditions for the general population have improved.
    </p>
    <FoodInsecurityChart />
    <p class="dataset-info">
        <strong>About the Data Sources:</strong>
        This chart draws from several key data sources to highlight the widening gap in food security between U.S. college students and the general population:
        <br><strong>A Decade of College Student Hunger: What We Know and Where We Need to Go</strong> 
                (<a href="https://pmc.ncbi.nlm.nih.gov/articles/PMC8913502/" target="_blank">https://pmc.ncbi.nlm.nih.gov/articles/PMC8913502/</a>). 
                This article, published in <em>Front Public Health</em>, provides foundational insights into college food insecurity trends, including the first survey from 2009 reporting 21% food insecurity among college students.
        <br><strong>Food Insecurity among College Students in the United States: A Scoping Review</strong> 
                (<a href="https://www.sciencedirect.com/science/article/pii/S2161831322002599" target="_blank">https://www.sciencedirect.com/science/article/pii/S2161831322002599</a>). 
                This scoping review, published in <em>ScienceDirect</em>, reveals a stark rise in food insecurity among college students, from 21% in 2009 to 48% in 2019, contrasting with a decline in national food insecurity rates.
        <br><strong>USDA ERS - Key Statistics & Graphics</strong> 
                (<a href="https://www.ers.usda.gov/topics/food-nutrition-assistance/food-security-in-the-u-s/key-statistics-graphics/#foodsecure" target="_blank">https://www.ers.usda.gov/topics/food-nutrition-assistance/food-security-in-the-u-s/key-statistics-graphics/#foodsecure</a>). 
                This resource from the United States Department of Agriculture provides comprehensive statistics on food security in the general U.S. population, offering a baseline for comparison with student-specific data.
    </p>
</div>

<div class="third-content">
    <h2>Food Insecurity and Its Impact on Nutrition</h2>
    <p>
        Food insecurity doesn't just mean skipping meals—it often leads to poor nutrition, as students resort to inexpensive, less healthy options. 
        <br>How does this impact their overall health? Let’s take a closer look at the data.
    </p>
    <NutritionIntakeChart />
    <p class="dataset-info">
        <strong>About the Data Sources:</strong>
        This chart draws from two key data sources to highlight nutritional deficiencies among college students and compare their intake to recommended guidelines:
        <br><strong>Dietary Guidelines for Americans, 2020-2025</strong> 
            (<a href="https://www.dietaryguidelines.gov/resources/2020-2025-dietary-guidelines-online-materials" target="_blank">https://www.dietaryguidelines.gov/resources/2020-2025-dietary-guidelines-online-materials</a>, 
            <a href="https://www.dietaryguidelines.gov/sites/default/files/2020-12/Dietary_Guidelines_for_Americans_2020-2025.pdf" target="_blank">PDF</a>). 
            Published by the U.S. Department of Agriculture and U.S. Department of Health and Human Services, these guidelines provide recommended daily intake levels for nutrients.
             <!-- The data is used as a benchmark to identify areas of deficiency (e.g., fiber, whole grains) and excess (e.g., added sugars) in college students' diets. -->
        <br><strong>Food Insecurity and Dietary Intake among College Students with Unlimited Meal Plans at a Large, Midwestern University</strong> 
            (<a href="https://pmc.ncbi.nlm.nih.gov/articles/PMC8530829/" target="_blank">https://pmc.ncbi.nlm.nih.gov/articles/PMC8530829/</a>). 
            Published in the <em>Journal of the Academy of Nutrition and Dietetics</em> in 2021, this study identifies specific nutritional deficiencies among college students, providing data that is directly compared to the Dietary Guidelines for Americans.
             <!-- It emphasizes the gaps in students' intake, which are critical to addressing their dietary needs. -->
    </p>
    <p>One of the most alarming trends is the overconsumption of sugar.</p>
    <p>Faced with limited choices, students often turn to sugary snacks and beverages instead of balanced meals
        This compounds their nutritional challenges and has serious health consequences.
        <strong>How prevalent is sugar in their diets?</strong>
    </p>
</div>

<div class="fourth-content">
    <h2>Excess Sugar Consumption: A Health Risk Amplified by Food Insecurity</h2>
    <p>A comprehensive analysis of popular recipes reveals that among the top 10% of high-sugar foods:</p>
    <ul>
        <li>Desserts account for <strong>50.8%</strong></li>
        <li>Beverages account for <strong>6.8%</strong></li>
    </ul>
    <p>These are the very items students choose when faced with limited meal options.</p>
    <p>Click to move around the recipes!</p>
    <SugarRecipesTreemap />

    <p class="dataset-info">
        <strong>About the Dataset:</strong> This recipes dataset from Kaggle 
        (<a href="https://www.kaggle.com/datasets/irkaal/foodcom-recipes-and-reviews" target="_blank">https://www.kaggle.com/datasets/irkaal/foodcom-recipes-and-reviews</a>) 
        contains 522,517 recipes from 312 different categories. 
        This dataset provides information about each recipe like cooking times, servings, ingredients, nutrition, instructions, and more.
    </p>
</div>

<div class="fifth-content">
    <h2>Health Implications of High Sugar Consumption</h2>
    <p>High sugar intake has been linked to several health risks, including:</p>
    <ul>
        <li>
            <strong>Weight Gain and Obesity:</strong> Excess sugar contributes to weight gain, increasing the risk of cardiovascular disease and diabetes.
        </li>
        <li>
            <strong>Increased Risk of Type 2 Diabetes:</strong> High sugar intake can lead to insulin resistance, a precursor to Type 2 diabetes.
        </li>
        <li>
            <strong>Mental Health and Academic Performance:</strong> Diets high in sugar are linked to mood swings, reduced focus, and poor academic outcomes.
        </li>
    </ul>
</div>

<div class="solution-content">
    <h2>Proposing Healthier Alternatives</h2>
    <h3>Current Choice: Cake</h3>
    <p>
        Students often gravitate toward sweet, calorie-dense snacks like cake because they are convenient, satisfying, and widely available. However, while cake offers a quick energy boost, it lacks essential nutrients like fiber and protein.
    </p>
    <h3>A Healthier Alternative: Dark Chocolate</h3>
    <p>
        For students looking for a sweet treat, dark chocolate provides a slightly healthier option.
    </p>
    <ul>
        <li>Lower in sugar compared to cake.</li>
        <li>Contains antioxidants that may benefit heart health.</li>
        <li>A more mindful indulgence without eliminating sweetness entirely.</li>
    </ul>
    <h3>An Even Healthier Snack: Chia Pudding</h3>
    <p>
        For a more significant nutritional upgrade, chia pudding stands out as an excellent choice.
    </p>
    <ul>
        <li><strong>High in Fiber</strong>: Supports digestion and satiety</li>
        <li><strong>Rich in Protein</strong>: Essential for muscle repair and sustained energy</li>
        <li><strong>Healthy Fats</strong>: Made from chia seeds soaked in milk or plant-based alternatives, healthy fats helps maintain overall energy balance.</li>
        <li><strong>Versatile</strong>: Can be flavored with natural sweeteners or fruits</li>
    </ul>

    <h3>The Ideal Convenient and Affordable Meal: Kimbap</h3>
    <p>While healthy snacks like chia pudding address cravings, they don’t replace the need for balanced meals. Kimbap, a Korean dish similar to sushi rolls, offers a complete and portable meal.</p>
    <p>Kimbap delivers a balance of macronutrients and micronutrients, including:</p>
    <ul>
        <li><strong>Carbohydrates</strong> for sustained energy</li>
        <li><strong>Protein and fats</strong> for satiety and muscle repair</li>
        <li><strong>Vitamins and minerals</strong> from fresh vegetables</li>
    </ul>

    <p>It’s not just nutritious—it’s practical and affordable:</p>
    <ul>
        <li><strong>Affordable</strong>: Priced around $3.99, cheaper than a typical dessert like Trader Joe’s $4.99 Vanilla Bean Mini Cake.</li>
        <li><strong>Affordable and Portable</strong>: As a frozen meal, it's easy to carry and eat on the go.</li>
        <li><strong>Trendy</strong>: Growing popularity on social media makes it appealing to students.</li>
    </ul>
     <p>
        Let’s take a closer look at how these choices stack up nutritionally! Click on checkboxs to compare the choices.
    </p>
    <NutritionComparison />
    <p class="dataset-info">
        <strong>About the Data Sources:</strong>
        This chart draws from multiple key data sources to provide a comparative analysis of the nutritional profiles of four popular food choices: cake, dark chocolate, chia pudding, and tuna kimbap:
        <strong>Nutritionix</strong> 
            (<a href="https://www.nutritionix.com/food/" target="_blank">https://www.nutritionix.com/food/</a>). 
            provides comprehensive nutritional information for various foods, including 
            <a href="https://www.nutritionix.com/food/tuna-kimbap" target="_blank">tuna kimbap</a>.
    </p>
 
</div>
<div class="final-content">
    <h2>Addressing Student Food Insecurity with Nutritious Options</h2>
    <p>Carnegie Mellon University has the opportunity to lead the way in tackling student food insecurity by championing healthier, accessible options. By partnering with local vendors and expanding the availability of nutrient-rich snacks like chia pudding and balanced meals such as kimbap, CMU Dining Services can:</p>
    <ul>
        <li><strong>Support students’ physical and mental well-being</strong></li>
        <li><strong>Empower students to make healthier dietary decisions</strong></li>
        <li><strong>Foster a more resilient and health-conscious campus community</strong></li>
    </ul>
    <p>Together, we can make sure every CMU student has access to affordable, nutritious food options that fuel their academic and personal success.</p>
    <p>To healthier students!</p>
    <div style = "text-align: center;">
        <img src="/images/student image.jpg" alt="Student is buying Kimbap and chia pudding" width="800" style="margin-top: 20px;">   
    </div>
</div>


