<script>
    import * as d3 from 'd3';
    import { onMount } from 'svelte';

    const data = [
        { nutrient: "Fruits (cup)", recommended: 2.0, students: 1.07 },
        { nutrient: "Vegetables (cup)", recommended: 2.5, students: 1.54 },
        { nutrient: "Whole grains (ounce)", recommended: 3.0, students: 0.78 },
        { nutrient: "Dairy (cup)", recommended: 3.0, students: 1.94 },
        { nutrient: "Added sugars (teaspoon)", recommended: 12.5, students: 16.77 },
        { nutrient: "Calcium (gram)", recommended: 1.0, students: 1.1 },
        { nutrient: "Fiber (gram)", recommended: 31.0, students: 17.72 }
    ];

    let svgWidth = 800;
    let svgHeight = 500;
    let svg;

    let hasAnimated = false;  // To ensure animation happens only once

    onMount(() => {
        drawChart();
        // Intersection Observer to trigger animation when the chart is in view
        const observer = new IntersectionObserver(entries => {
            entries.forEach(entry => {
                if (entry.isIntersecting && !hasAnimated) {
                    hasAnimated = true;
                    fadeInDifferenceBars();  // Trigger fade-in animation
                }
            });
        });

        observer.observe(document.getElementById("nutrition-chart"));
    });

    let x0, x1, y;
    
    function drawChart() {
        svg = d3.select('#nutrition-chart')
            .attr('width', svgWidth)
            .attr('height', svgHeight);

        // Define pattern for difference bars
        svg.append("defs")
            .append("pattern")
            .attr("id", "diagonalHatch")
            .attr("patternUnits", "userSpaceOnUse")
            .attr("width", 4)
            .attr("height", 4)
            .append("path")
            .attr("d", "M0,0 L4,4 M4,0 L0,4") // Diagonal lines
            .attr("stroke", "red")
            .attr("stroke-width", 1);

        const margin = { top: 20, right: 80, bottom: 90, left: 60 };  // Adjust right margin
        const width = svgWidth - margin.left - margin.right;
        const height = svgHeight - margin.top - margin.bottom;

        x0 = d3.scaleBand()
                     .domain(data.map(d => d.nutrient))
                     .rangeRound([margin.left, width + margin.left])  // Match width without extra padding
                    //  .rangeRound([margin.left, width + margin.left + 30])  // Add extra space on the right
                     .paddingInner(0.5);  // Adjust padding to create extra space for the difference bars

        x1 = d3.scaleBand()
                     .domain(["recommended", "students"])
                     .rangeRound([0, x0.bandwidth()])
                     .padding(0.05);

        y = d3.scaleLinear()
                    // .domain([0, d3.max(data, d => Math.max(d.recommended, d.students))])
                    .domain([-15, d3.max(data, d => Math.max(d.recommended, d.students))])
                    .nice()
                    .rangeRound([height + margin.top, margin.top]);

        const color = d3.scaleOrdinal()
                        .domain(["recommended", "students"])
                        .range(["#5f6caf", "#ff8364"]);

        // Draw bars for recommended and students initially
        svg.append("g")
           .selectAll("g")
           .data(data)
           .join("g")
             .attr("transform", d => `translate(${x0(d.nutrient)},0)`)
           .selectAll("rect")
           .data(d => [
               { key: "recommended", value: d.recommended },
               { key: "students", value: d.students }
           ])
           .join("rect")
             .attr("x", d => x1(d.key))
            //  .attr("y", d => y(d.value))
            .attr("y", d => Math.min(y(d.value), y(0))) // Handle negative values
             .attr("width", x1.bandwidth())
            //  .attr("height", d => y(0) - y(d.value))
            .attr("height", d => Math.abs(y(0) - y(d.value))) // Ensure height works for negative values
             .attr("fill", d => color(d.key));

        // Add difference bars with pattern fill and initial opacity 0
        svg.append("g")
           .selectAll(".difference-bar")
           .data(data)
           .enter()
           .append("rect")
             .attr("class", "difference-bar")
             .attr("x", d => {
                 if (d.recommended > d.students) {
                     return x0(d.nutrient) + x1("students"); // Place above students bar
                 } else {
                     return x0(d.nutrient) + x1("recommended"); // Place above recommended bar
                 }
             })
             .attr("width", x1.bandwidth())
            //  .attr("fill", "url(#diagonalHatch)")  // Use pattern as fill
            .attr("fill", d => {
                const difference = d.students - d.recommended;
                return difference > 0 ? "#004a2f" : "#ff4545"; // Green for excess, red for deficiency
            })
             .attr("opacity", 0)  // Start hidden
             .attr("y", d => {
                 if (d.recommended > d.students) {
                     return y(d.recommended); // Place above students bar
                 } else {
                     return y(d.students); // Place above recommended bar
                 }
             })
             .attr("height", d => Math.abs(y(d.recommended) - y(d.students)));

        // X Axis
        svg.append("g")
           .attr("transform", `translate(0,${height + margin.top})`)
           .call(d3.axisBottom(x0).tickSizeOuter(0))  // Prevent ticks extending beyond axis
           .append("line")  // Manually extend the axis line to full width
           .attr("x1", margin.left)  // Start at left margin
           .attr("x2", svgWidth - margin.right)  // End at full SVG width minus right margin

        svg.selectAll(".tick text")  // Style for readability
           .attr("transform", "rotate(-45)")
           .style("text-anchor", "end");

        // Y Axis
        svg.append("g")
           .attr("transform", `translate(${margin.left},0)`)
           .call(d3.axisLeft(y));

        // Y-Axis Label
        svg.append("text")
           .attr("transform", "rotate(-90)")
           .attr("y", margin.left - 50)
           .attr("x", 0 - (height / 2))
           .attr("dy", "1em")
           .style("text-anchor", "middle")
           .style("font-size", "14px")
           .text("Daily Intake");

        // Legend
        svg.append("rect")
           .attr("x", svgWidth - 700)
           .attr("y", margin.top)
           .attr("width", 10)
           .attr("height", 10)
           .attr("fill", "#5f6caf");

        svg.append("text")
           .attr("x", svgWidth - 680)
           .attr("y", margin.top + 9)
           .style("font-size", "12px")
           .text("Recommended Intake");

        svg.append("rect")
           .attr("x", svgWidth - 700)
           .attr("y", margin.top + 20)
           .attr("width", 10)
           .attr("height", 10)
           .attr("fill", "#ff8364");

        svg.append("text")
           .attr("x", svgWidth - 680)
           .attr("y", margin.top + 29)
           .style("font-size", "12px")
           .text("Student Intake");

        // Deficiency Legend
        svg.append("rect")
        .attr("x", svgWidth - 700)
        .attr("y", margin.top + 40)
        .attr("width", 10)
        .attr("height", 10)
        .attr("fill", "#ff4545");

        svg.append("text")
        .attr("x", svgWidth - 680)
        .attr("y", margin.top + 49)
        .style("font-size", "12px")
        .text("Deficiency (Student intake < Recommended)");

        // Excess Legend
        svg.append("rect")
        .attr("x", svgWidth - 700)
        .attr("y", margin.top + 60)
        .attr("width", 10)
        .attr("height", 10)
        .attr("fill", "#004a2f");

        svg.append("text")
        .attr("x", svgWidth - 680)
        .attr("y", margin.top + 69)
        .style("font-size", "12px")
        .text("Excess (Student intake > Recommended)");

    }

    function fadeInDifferenceBars() {
        // Fade in difference bars
        svg.selectAll(".difference-bar")
            .transition()
            .duration(2000)
            .attr("opacity", 1);

        // Explicitly call the next function after the fade-in completes
        setTimeout(moveBarsRight, 2000); // Delay matches the fade-in duration
    }

    function moveBarsRight() {
        // Move difference bars to the right of the student bar
        svg.selectAll(".difference-bar")
            .transition()
            .duration(1000)
            .attr("x", d => x0(d.nutrient) + x1("students") + x1.bandwidth()+2); // Move to the right of the student bar

        // Explicitly call the next function after moving to the right
        setTimeout(dropBarsToXAxis, 1000); // Delay matches the move right duration
    }

    function dropBarsToXAxis() {
        svg.selectAll(".difference-bar")
        .transition()
        .duration(1000)
        .attr("y", d => {
            // Position bars based on difference (above or below x-axis)
            const difference = d.students - d.recommended;
            return difference > 0 ? y(difference) : y(0);
        })
        .attr("height", d => {
            // Adjust height based on the absolute value of the difference
            return Math.abs(y(d.students - d.recommended) - y(0));
        });        
    }
</script>

<style>
    .chart-container {
        display: flex;
        flex-direction: column;
        align-items: center;
        margin-top: 20px;
    }
</style>

<div class="chart-container">
    <svg id="nutrition-chart"></svg>
</div>