<script>
    import { onMount } from "svelte";
    import * as d3 from "d3";

    const dataMap = {
        cake: [
            { category: "Fiber", value: 0.2 },
            { category: "Protein", value: 2 },
            { category: "Sugar", value: 28 },
            { category: "Non-Sugar Carbs", value: 38 },
            { category: "Fat", value: 12 }
        ],
        darkChocolate: [
            { category: "Fiber", value: 2 },
            { category: "Protein", value: 1.4 },
            { category: "Sugar", value: 14 },
            { category: "Non-Sugar Carbs", value: 17 },
            { category: "Fat", value: 8.9 }
        ],
        chiaPudding: [
            { category: "Fiber", value: 6.3 },
            { category: "Protein", value: 12 },
            { category: "Sugar", value: 12 },
            { category: "Non-Sugar Carbs", value: 21 },
            { category: "Fat", value: 8 }
        ],
        kimbap: [
            { category: "Fiber", value: 1.7 },
            { category: "Protein", value: 34 },
            { category: "Sugar", value: 9.5 },
            { category: "Non-Sugar Carbs", value: 67 },
            { category: "Fat", value: 16 }
        ]
    };

    let svg;
    let activeDatasets = ["cake", "darkChocolate", "chiaPudding", "kimbap"];

    const dataColors = {
        cake: "rgb(255, 127, 80)",
        darkChocolate: "rgb(93, 95, 236)",
        chiaPudding: "rgb(255, 193, 7)",
        kimbap: "rgb(97, 183, 136)"
    };

    onMount(() => {
        renderChart();
    });

    function toggleDataset(dataset) {
        if (activeDatasets.includes(dataset)) {
            activeDatasets = activeDatasets.filter(d => d !== dataset);
        } else {
            activeDatasets.push(dataset);
        }
        renderChart(); // Re-render the chart with updated datasets
    }

    function renderChart() {
        const radarChartContainer = document.querySelector("#radar-chart");

        // Clear previous content
        d3.select(radarChartContainer).selectAll("*").remove();
        svg = null;

        const datasetsToShow = activeDatasets.map(key => dataMap[key]);
        const normalizedDatasets = datasetsToShow.map(data => {
            const total = d3.sum(data, d => d.value);
            return data.map(d => ({
                category: d.category,
                value: (d.value / total) * 100
            }));
        });

        const maxValue = d3.max(normalizedDatasets.flat(), d => d.value);

        renderCombinedChart(normalizedDatasets, maxValue);
    }

    function renderCombinedChart(normalizedDatasets, maxValue) {
        const width = 600;
        const height = 600;
        const margin = 50;
        const radius = Math.min(width, height) / 2 - margin;

        const allCategories = normalizedDatasets[0]?.map(d => d.category) || [];

        svg = d3
            .select("#radar-chart")
            .append("svg")
            .attr("width", width)
            .attr("height", height)
            .append("g")
            .attr("transform", `translate(${width / 2},${height / 2})`);

        const angleScale = d3
            .scaleOrdinal()
            .domain(allCategories)
            .range(allCategories.map((_, i) => (i / allCategories.length) * 2 * Math.PI));

        const radiusScale = d3.scaleLinear()
            .domain([0, maxValue * 1.1])
            .range([0, radius]);

        const levels = 5;
        svg.selectAll(".grid-level")
            .data(d3.range(1, levels + 1))
            .join("circle")
            .attr("class", "grid-level")
            .attr("r", d => (d / levels) * radiusScale(maxValue))
            .attr("fill", "none")
            .attr("stroke", "#ccc")
            .attr("stroke-dasharray", "2,2");

        svg.selectAll(".axis")
            .data(allCategories)
            .join("line")
            .attr("class", "axis")
            .attr("x1", 0)
            .attr("y1", 0)
            .attr("x2", d => Math.cos(angleScale(d) - Math.PI / 2) * radiusScale(maxValue * 1.1))
            .attr("y2", d => Math.sin(angleScale(d) - Math.PI / 2) * radiusScale(maxValue * 1.1))
            .attr("stroke", "#888");

        svg.selectAll(".axis-label")
            .data(allCategories)
            .join("text")
            .attr("class", "axis-label")
            .attr("x", d =>
                Math.cos(angleScale(d) - Math.PI / 2) * (radiusScale(maxValue * 1.1) + 20)
            )
            .attr("y", d =>
                Math.sin(angleScale(d) - Math.PI / 2) * (radiusScale(maxValue * 1.1) + 20)
            )
            .attr("text-anchor", "middle")
            .style("font-size", "12px")
            .text(d => d);

        normalizedDatasets.forEach((data, index) => {
            const datasetName = activeDatasets[index]; // Match dataset with its name
            const radarLine = d3
                .lineRadial()
                .radius(d => radiusScale(d.value))
                .angle(d => angleScale(d.category))
                .curve(d3.curveLinearClosed);

            // svg.append("path")
            //     .datum(data)
            //     .attr("d", radarLine)
            //     // .attr("fill", dataColors[index].color)
            //     .attr("fill", dataColors[datasetName]) // Use dataset name to get color
            //     .attr("fill-opacity", 0.4)
            //     // .attr("stroke", dataColors[index].color)
            //     .attr("stroke", dataColors[datasetName]) // Use dataset name to get color
            //     .attr("stroke-width", 2);

            // const path = svg.append("path")
            //     .datum(data)
            //     .attr("d", radarLine)
            //     .attr("fill", dataColors[datasetName])
            //     .attr("fill-opacity", 0.4)
            //     .attr("stroke", dataColors[datasetName])
            //     .attr("stroke-width", 2)
            //     .on("mouseover", function (event, d) {
            //         // Show percentage labels
            //         svg.selectAll(`.hover-label-${index}`)
            //             .data(data)
            //             .join("text")
            //             .attr("class", `hover-label-${index}`)
            //             .attr("x", d => Math.cos(angleScale(d.category) - Math.PI / 2) * radiusScale(d.value))
            //             .attr("y", d => Math.sin(angleScale(d.category) - Math.PI / 2) * radiusScale(d.value) - 10)
            //             .attr("text-anchor", "middle")
            //             .style("font-size", "10px")
            //             .style("fill", "#000")
            //             .text(d => `${d.value.toFixed(1)}%`);
            //     })
            //     .on("mouseout", function () {
            //         // Hide percentage labels
            //         svg.selectAll(`.hover-label-${index}`).remove();
            //     });

            svg.append("path")
                    .datum(data)
                    .attr("d", radarLine)
                    .attr("fill", dataColors[datasetName])
                    .attr("fill-opacity", 0.4)
                    .attr("stroke", dataColors[datasetName])
                    .attr("stroke-width", 2)
                    .on("mouseover", function () {
                        // Show percentage labels at the data point locations
                        svg.selectAll(`.hover-label-${index}`)
                            .data(data)
                            .join("text")
                            .attr("class", `hover-label-${index}`)
                            .attr("x", d => Math.cos(angleScale(d.category) - Math.PI / 2) * radiusScale(d.value))
                            .attr("y", d => Math.sin(angleScale(d.category) - Math.PI / 2) * radiusScale(d.value))
                            .attr("text-anchor", "middle")
                            .style("font-size", "10px")
                            .style("fill", "#000")
                            .text(d => `${d.value.toFixed(1)}%`);
                    })
                    .on("mouseout", function () {
                        // Hide percentage labels
                        svg.selectAll(`.hover-label-${index}`).remove();
                    });

            // Add invisible circles for better hover detection
            svg.selectAll(`.hover-circle-${index}`)
                .data(data)
                .join("circle")
                .attr("class", `hover-circle-${index}`)
                .attr("cx", d => Math.cos(angleScale(d.category) - Math.PI / 2) * radiusScale(d.value))
                .attr("cy", d => Math.sin(angleScale(d.category) - Math.PI / 2) * radiusScale(d.value))
                .attr("r", 5)
                .attr("fill", "transparent")
                .on("mouseover", function () {
                    // Show percentage labels on hovering the circle
                    svg.selectAll(`.hover-label-${index}`)
                        .data(data)
                        .join("text")
                        .attr("class", `hover-label-${index}`)
                        .attr("x", d => Math.cos(angleScale(d.category) - Math.PI / 2) * radiusScale(d.value))
                        .attr("y", d => Math.sin(angleScale(d.category) - Math.PI / 2) * radiusScale(d.value))
                        .attr("text-anchor", "middle")
                        .style("font-size", "10px")
                        .style("fill", "#000")
                        .text(d => `${d.value.toFixed(1)}%`);
                })
                .on("mouseout", function () {
                    // Hide percentage labels on mouse out
                    svg.selectAll(`.hover-label-${index}`).remove();
                });


            // // Add interactive circles
            // svg.selectAll(`.data-point-${index}`)
            //     .data(data)
            //     .join("circle")
            //     .attr("class", `data-point-${index}`)
            //     .attr("cx", d => Math.cos(angleScale(d.category) - Math.PI / 2) * radiusScale(d.value))
            //     .attr("cy", d => Math.sin(angleScale(d.category) - Math.PI / 2) * radiusScale(d.value))
            //     .attr("r", 4)
            //     .attr("fill", dataColors[datasetName])
            //     .on("mouseover", function (event, d) {
            //         // Add percentage label on hover
            //         svg.append("text")
            //             .attr("class", "hover-label")
            //             .attr("x", Math.cos(angleScale(d.category) - Math.PI / 2) * radiusScale(d.value))
            //             .attr("y", Math.sin(angleScale(d.category) - Math.PI / 2) * radiusScale(d.value) - 10)
            //             .attr("text-anchor", "middle")
            //             .style("font-size", "10px")
            //             .style("fill", "#000")
            //             .text(`${d.value.toFixed(1)}%`);
            //     })
            //     .on("mouseout", () => {
            //         // Remove percentage label on mouse out
            //         svg.selectAll(".hover-label").remove();
            //     });
            
            // Add percentage labels near each data point
            // svg.selectAll(`.value-label-${index}`)
            //     .data(data)
            //     .join("text")
            //     .attr("class", `value-label-${index}`)
            //     .attr("x", d => Math.cos(angleScale(d.category) - Math.PI / 2) * radiusScale(d.value))
            //     .attr("y", d => Math.sin(angleScale(d.category) - Math.PI / 2) * radiusScale(d.value) - 10)
            //     .attr("text-anchor", "middle")
            //     .style("font-size", "10px")
            //     .style("fill", "#000")
            //     .text(d => `${d.value.toFixed(1)}%`);

            // svg.selectAll(`.value-label-${index}`)
            //     .data(data)
            //     .join("text")
            //     .attr("class", `value-label-${index}`)
            //     .attr("x", d => {
            //         const angle = angleScale(d.category) - Math.PI / 2;
            //         const offset = 10 * Math.sign(Math.cos(angle)); // Offset direction
            //         return Math.cos(angle) * radiusScale(d.value) + offset;
            //     })
            //     .attr("y", d => {
            //         const angle = angleScale(d.category) - Math.PI / 2;
            //         const offset = 10 * Math.sign(Math.sin(angle)); // Offset direction
            //         return Math.sin(angle) * radiusScale(d.value) + offset;
            //     })
            //     .attr("text-anchor", "middle")
            //     .style("font-size", "10px")
            //     .style("fill", "#000")
            //     .text(d => `${d.value.toFixed(1)}%`);
        });

        addLegend(svg, dataColors, width, height);
    }

    function addLegend(svg, dataColors, width, height) {
        const legend = svg.append("g").attr("transform", `translate(${-width / 2},${-height / 2 + 20})`);

        dataColors.forEach(({ label, color }, index) => {
            const yOffset = index * 20;
            legend
                .append("circle")
                .attr("cx", 10)
                .attr("cy", yOffset)
                .attr("r", 5)
                .attr("fill", color);

            legend
                .append("text")
                .attr("x", 20)
                .attr("y", yOffset + 5)
                .style("font-size", "12px")
                .text(label);
        });
    }
</script>

<div id="container">
    <!-- <h2>Toggle Datasets</h2> -->
    <div>
        {#each Object.keys(dataMap) as dataset}
            <label style="color: {dataColors[dataset]}">
                <input
                    type="checkbox"
                    checked={activeDatasets.includes(dataset)}
                    on:change={() => toggleDataset(dataset)}
                />
                {dataset.charAt(0).toUpperCase() + dataset.slice(1)}
            </label>
        {/each}

    </div>
    <div id="radar-chart"></div>
</div>
<!-- <div id="container">
    <h2>Toggle Datasets</h2>
    <div>
        {#each Object.keys(dataMap) as dataset}
            <label>
                <input
                    type="checkbox"
                    checked={activeDatasets.includes(dataset)}
                    on:change={() => toggleDataset(dataset)}
                />
                {dataset.charAt(0).toUpperCase() + dataset.slice(1)}
            </label>
        {/each}
    </div>
    <div id="radar-chart"></div>
</div> -->

<style>
    #radar-chart {
        display: flex;
        justify-content: center;
        align-items: flex-start;
        flex-wrap: wrap; /* Allow the content to wrap */
        margin: 20px auto; /* Add spacing above and below */
        width: 100%; /* Ensure full width */
        height: auto; /* Allow dynamic height */
        overflow: hidden; /* Prevent scrollbars */
        position: relative; /* Ensure proper positioning for the chart */
        /* background: #f9f9f9; Subtle background for better contrast */
        border-radius: 8px; /* Smooth edges */
        /* box-shadow: 0px 4px 6px rgba(0, 0, 0, 0.1); Add a subtle shadow */
        /* padding: 20px; Add padding inside the chart area */
    }

    .chart-grid {
        display: grid;
        grid-template-columns: repeat(2, 300px); /* Explicitly set 2 columns */
        gap: 20px; /* Space between charts */
        justify-content: center; /* Center grid horizontally */
        align-items: start; /* Align content at the top */
        width: fit-content; /* Resize to fit content */
        margin: 0 auto; /* Center grid in the container */
    }

    .chart-container {
        display: flex;
        flex-direction: column;
        align-items: center;
        justify-content: flex-start; /* Align content at the top */
        width: 300px; /* Fixed width for grid items */
        height: 400px; /* Fixed height for grid items */
    }

    .chart-title {
        font-weight: bold;
        font-size: 16px;
        margin-bottom: 10px;
        text-align: center;
        height: auto; /* Adjust dynamically if needed */
        color: #333; /* Slightly darker text */
    }

    .value-label {
        font-size: 10px;
        fill: black;
    }

    /* New Styles for Toggle Controls */
    h2 {
        font-size: 18px;
        margin-bottom: 10px;
        color: #444;
        text-align: center;
    }

    label {
        display: inline-block;
        margin: 10px;
        cursor: pointer;
        font-size: 14px;
        color: #555;
    }

    input[type="checkbox"] {
        margin-right: 5px;
        cursor: pointer;
    }

    /* Legend Styles */
    .legend {
        display: flex;
        align-items: center;
        margin: 10px 0;
    }

    .legend circle {
        margin-right: 8px;
    }

    .legend text {
        font-size: 12px;
        color: #666;
    }
</style>