<script>
    import * as d3 from "d3";
    import { onMount } from "svelte";

    let data;
    let rootNode;
    let currentNode;
    let currentCategory = "Recipes";
    let width = 928;
    let height = 924;
    let svg;
    let group;
    let x, y;

    onMount(async () => {
        try {
            // console.log("Fetching hierarchical data...");
            const response = await fetch('./data/hierarchical_recipes.json');
            if (!response.ok) throw new Error("Failed to fetch JSON data");
            data = await response.json();
            // console.log("Data loaded:", data);
            setupTreemap();
        } catch (error) {
            console.error("Error loading hierarchical data:", error);
        }
    });

    function setupTreemap() {
        // console.log("Initializing treemap...");

        // Prepare hierarchical data
        rootNode = d3
            .hierarchy(data)
            .sum((d) => (d.children ? d.children.length : 1)) // Use count of children or 1 for leaf nodes
            .sort((a, b) => b.value - a.value);

        currentNode = rootNode;

        // console.log("Hierarchical data created:", rootNode);

        // Create scales for zooming
        x = d3.scaleLinear().domain([0, width]).range([0, width]);
        y = d3.scaleLinear().domain([0, height]).range([0, height]);

        // Apply treemap layout
        const treemap = d3.treemap().size([width, height]).paddingInner(1);
        treemap(rootNode);

        // console.log("Treemap layout computed:", rootNode);

        renderTreemap(rootNode);
    }

    function renderTreemap(node) {
        currentCategory = node.data.name;
        // console.log(`Rendering treemap for: ${currentCategory}`);

        if (svg) svg.remove();

        const container = d3.select("#treemap");
        const width = container.node().getBoundingClientRect().width;
        const height = Math.max(container.node().getBoundingClientRect().height - 60, 500);

        x.range([0, width]);
        y.range([0, height]);

        svg = container
            .append("svg")
            .attr("viewBox", `0 0 ${width} ${height + 30}`)
            .attr("width", width)
            .attr("height", height + 30)
            .style("font", "10px sans-serif");

        // Parent category
        if (node.parent) {
            svg.append("text")
                .attr("x", 10)
                .attr("y", 20)
                .attr("fill", "blue")
                .attr("font-size", "13px")
                .style("cursor", "pointer")
                .style("text-decoration", "underline")
                .text(node.parent.data.name)
                .on("click", () => zoomOut(node));
        }

        // Current category
        svg.append("text")
            .attr("x", node.parent ? 80 : 10)
            .attr("y", 20)
            .attr("fill", "black")
            .attr("font-size", "13px")
            .attr("font-weight", "bold")
            .text(node.parent ? `/ ${node.data.name}` : node.data.name);

        group = svg.append("g").attr("transform", "translate(0,30)");

        renderNodes(group, node);
    }


    const colorScale = d3.scaleOrdinal()
        .domain(["Category", "Leaf Node"])
        .range(["#6baed6", "#fd8d3c"]);

    function renderNodes(group, root) {
        // console.log("Rendering nodes...");

        const colorScale = {
            Desserts: "#ff8364", // color for Desserts
            Beverages: "#e8aa8c", // color for Beverages
            "Other Desserts": "#edf7fa", // Seashell for Other Desserts
        };

        const nodes = group
            .selectAll("g")
            .data(root.children || [])
            .join("g")
            .attr("transform", (d) => `translate(${x(d.x0)},${y(d.y0)})`)
            .on("click", (_, d) => {
                if (d.children && d.depth <= 2) {
                    // console.log(`Zooming in to: ${d.data.name}`);
                    zoomIn(d);
                } else {
                    // console.log(`Click ignored: ${d.data.name} (depth ${d.depth})`);
                }
            });

        nodes.style("cursor", (d) => (d.children && d.depth <= 2 ? "pointer" : "default"));

        // Add rectangles for nodes
        nodes.append("rect")
            .attr("fill", (d) => {
                if (d.depth === 1) {
                    // Category colors
                    return colorScale[d.data.name] || "#ccc"; // Default gray
                } else if (d.depth === 2) {
                    // Subcategory colors
                    if (d.parent.data.name === "Desserts") {
                        return "#FFC0CB"; // Pink for subcategories under Desserts
                    } else if (d.parent.data.name === "Beverages") {
                        return "#D2B48C"; // Tan for subcategories under Beverages
                    }
                    const categoryColor = colorScale[d.parent.data.name];
                    return d3.color(categoryColor)?.brighter(1) || "#ddd"; // Lighter version
                } else {
                    // Default for deeper levels
                    return "#f5f5f5";
                }
            })
            .attr("stroke", "#000")
            .attr("stroke-width", 1)
            .attr("width", (d) => x(d.x1) - x(d.x0)) // Use exact scale for width
            .attr("height", (d) => y(d.y1) - y(d.y0)); // Use exact scale for height

        // Add text for non-leaf nodes
        nodes.append("text")
            .filter((d) => d.children) // Only add text for nodes with children
            .attr("x", 3)
            .attr("y", 13)
            .attr("fill", "#000")
            .text((d) => `${d.data.name} (${d.value})`)
            .style("font-size", (d) => {
                // Dynamically adjust font size based on node size
                const height = y(d.y1) - y(d.y0);
                return height > 15 ? "10px" : height > 10 ? "8px" : "6px";
            });

        // console.log("Nodes created:", nodes);
    }

    function zoomIn(node) {
        if (!node.children) return; // Prevent zooming on leaf nodes

        currentNode = node;
        // console.log(`Zooming in to: ${node.data.name}`);

        x.domain([node.x0, node.x1]);
        y.domain([node.y0, node.y1]);

        // console.log("Updated X domain:", x.domain());
        // console.log("Updated Y domain:", y.domain());

        const t = svg.transition().duration(750);

        group
            .selectAll("g")
            .transition(t)
            .attr("transform", (d) => `translate(${x(d.x0)},${y(d.y0)})`)
            .select("rect")
            .attr("width", (d) => Math.max(1, x(d.x1) - x(d.x0)))
            .attr("height", (d) => Math.max(1, y(d.y1) - y(d.y0)));

        t.end().then(() => renderTreemap(node));
    }

    function zoomOut(node) {
        if (!node.parent) {
            // console.log("Already at the root level, cannot zoom out further.");
            return; // Prevent zooming out beyond the root node
        }

        currentNode = node.parent;
        // console.log(`Zooming out to: ${currentNode.data.name}`);

        // Update the domain of the scales
        x.domain([currentNode.x0, currentNode.x1]);
        y.domain([currentNode.y0, currentNode.y1]);

        // console.log("Updated X domain:", x.domain());
        // console.log("Updated Y domain:", y.domain());

        const t = svg.transition().duration(750);

        // Transition group elements to match the parent node's domain
        group
            .selectAll("g")
            .transition(t)
            .attr("transform", (d) => `translate(${x(d.x0)},${y(d.y0)})`)
            .select("rect")
            .attr("width", (d) => Math.max(1, x(d.x1) - x(d.x0)))
            .attr("height", (d) => Math.max(1, y(d.y1) - y(d.y0)));

        // Render the parent node after the transition completes
        t.end().then(() => renderTreemap(currentNode));
    }
</script>

<div id="treemap"></div>

<style>
    #treemap {
        max-width: 100%;
        height: auto;
        overflow: hidden;
    }

    svg {
        display: block;
        margin: auto;
    }

    rect {
        cursor: pointer;
        transition: transform 0.3s;
    }

    rect:hover {
        transform: scale(1.02);
    }

    text {
        font-size: 12px;
        font-weight: bold;
    }
</style>