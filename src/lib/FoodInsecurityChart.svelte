<script>
    import * as d3 from 'd3';
    import { onMount } from 'svelte';

    const data = [
        { year: 2009, usFoodInsecurePercent: 14.7, collegeStudentFoodInsecurePercent: 21 },
        { year: 2010, usFoodInsecurePercent: 14.5 },
        { year: 2011, usFoodInsecurePercent: 14.9 },
        { year: 2012, usFoodInsecurePercent: 14.5 },
        { year: 2013, usFoodInsecurePercent: 14.3 },
        { year: 2014, usFoodInsecurePercent: 14.0 },
        { year: 2015, usFoodInsecurePercent: 12.7 },
        { year: 2016, usFoodInsecurePercent: 12.3 },
        { year: 2017, usFoodInsecurePercent: 11.8 },
        { year: 2018, usFoodInsecurePercent: 11.1 },
        { year: 2019, usFoodInsecurePercent: 10.5, collegeStudentFoodInsecurePercent: 48 }
    ];

    const width = 800, height = 450;
    const margin = { top: 20, right: 20, bottom: 50, left: 60 };
    let svg;

    onMount(() => {
        drawUSLine();
    });

    function drawUSLine() {
        d3.select('#food-insecurity-chart').selectAll('svg').remove();

        svg = d3.select('#food-insecurity-chart')
            .append('svg')
            .attr('width', width)
            .attr('height', height);

        const x = d3.scaleLinear()
            .domain([d3.min(data, d => d.year) - 1, d3.max(data, d => d.year) + 1])
            .range([margin.left, width - margin.right]);

        const y = d3.scaleLinear()
            .domain([0, d3.max(data, d => Math.max(d.usFoodInsecurePercent || 0, d.collegeStudentFoodInsecurePercent || 0))])
            .range([height - margin.bottom, margin.top]);

        const lineUS = d3.line()
            .x(d => x(d.year))
            .y(d => y(d.usFoodInsecurePercent));

        svg.append('path')
            .datum(data)
            .attr('fill', 'none')
            .attr('stroke', '#5f6caf')
            .attr('stroke-width', 2)
            .attr('d', lineUS)
            .attr('stroke-dasharray', function() {
                if (this instanceof SVGPathElement) {
                    return this.getTotalLength();
                } else {
                    console.error('Error: Non-path element in stroke-dasharray:', this);
                    return 0;
                }
            })
            .attr('stroke-dashoffset', function() {
                return this.getTotalLength();
            })
            .transition()
            .duration(2000)
            .attr('stroke-dashoffset', 0)
            .on('end', displayUSDataPoints);

        svg.append('g')
            .attr('transform', `translate(0,${height - margin.bottom})`)
            .call(d3.axisBottom(x).tickFormat(d3.format("d")));

        svg.append('g')
            .attr('transform', `translate(${margin.left},0)`)
            .call(d3.axisLeft(y));

        svg.append("text")
            .attr("transform", "rotate(-90)")
            .attr("y", margin.left - 50)
            .attr("x", 0 - (height / 2))
            .attr("dy", "1em")
            .style("text-anchor", "middle")
            .style("font-size", "14px")
            .text("Food Insecure Percentage");

        function displayUSDataPoints() {
            svg.selectAll(".us-point")
                .data(data)
                .enter()
                .append("text")
                .attr("class", "us-point")
                .attr("x", d => x(d.year))
                .attr("y", d => y(d.usFoodInsecurePercent) - 10)
                .attr("fill", "#5f6caf")
                .style("font-size", "12px")
                .text(d => d.usFoodInsecurePercent);

            svg.append("text")
                .attr("x", x(2015))
                .attr("y", y(10.5) - 50)
                .attr("fill", "#5f6caf")
                .style("font-size", "14px")
                .text("Food Insecure U.S. Households (%)");
        }
    }

    function drawCollegeLine() {
        const x = d3.scaleLinear()
            .domain([d3.min(data, d => d.year) - 1, d3.max(data, d => d.year) + 1])
            .range([margin.left, width - margin.right]);

        const y = d3.scaleLinear()
            .domain([0, d3.max(data, d => Math.max(d.usFoodInsecurePercent || 0, d.collegeStudentFoodInsecurePercent || 0))])
            .range([height - margin.bottom, margin.top]);

        const collegeData = data.filter(d => d.collegeStudentFoodInsecurePercent != null);

        const lineCollege = d3.line()
            .x(d => x(d.year))
            .y(d => y(d.collegeStudentFoodInsecurePercent));

        svg.append('path')
            .datum(collegeData)
            .attr('fill', 'none')
            .attr('stroke', '#ff8364')
            .attr('stroke-width', 2)
            .attr('d', lineCollege)
            .attr('stroke-dasharray', function () {
                return this.getTotalLength();
            })
            .attr('stroke-dashoffset', function () {
                return this.getTotalLength();
            })
            .transition()
            .duration(2000)
            .attr('stroke-dashoffset', 0)
            .on('end', displayCollegeDataPoints);

        function displayCollegeDataPoints() {
            svg.selectAll(".college-point")
                .data(collegeData)
                .enter()
                .append("circle")
                .attr("class", "college-point")
                .attr("cx", d => x(d.year))
                .attr("cy", d => y(d.collegeStudentFoodInsecurePercent))
                .attr("r", 4)
                .attr("fill", "#ff8364");

            svg.selectAll(".college-label")
                .data(collegeData)
                .enter()
                .append("text")
                .attr("class", "college-label")
                .attr("x", d => x(d.year) + 5)
                .attr("y", d => y(d.collegeStudentFoodInsecurePercent) - 10)
                .attr("fill", "#ff8364")
                .style("font-size", "12px")
                .text(d => d.collegeStudentFoodInsecurePercent);

            svg.append("text")
                .attr("x", x(2015))
                .attr("y", y(35))
                .attr("fill", "#ff8364")
                .style("font-size", "14px")
                .text("Food Insecure U.S. College Students (%)");
        }
    }
</script>
<div>
    <button class="editbutton" on:click={drawCollegeLine}>What About College Students?</button>
</div>
<div id="food-insecurity-chart"></div>

<style>
    
    #food-insecurity-chart {
        margin-top: 20px;
        width: 100%;
        height: 500px;
    }

    button {
        margin-top: 20px;
        padding: 10px 20px;
        font-size: 16px;
        cursor: pointer;
    }
    .editbutton {
        background: linear-gradient(45deg, #ffb677, #ff8364); /* Softer gradient */
        color: white; /* Text color */
        font-size: 16px; /* Font size */
        font-weight: bold; /* Make text bold */
        padding: 12px 24px; /* Inner spacing */
        border: none; /* Remove default border */
        border-radius: 25px; /* Rounded corners */
        cursor: pointer; /* Pointer cursor on hover */
        box-shadow: 0 4px 6px rgba(0, 0, 0, 0.2); /* Subtle shadow */
        transition: all 0.3s ease; /* Smooth hover animation */
    }

    /* Hover effect */
    .editbutton:hover {
        background: linear-gradient(135deg, #ff8364, #ffb677); /* Rotate gradient */
        box-shadow: 0 6px 10px rgba(0, 0, 0, 0.3); /* Increase shadow */
        transform: scale(1.05); /* Slightly enlarge */
    }

    /* Focus effect (when using Tab key) */
    .editbutton:focus {
        outline: none; /* Remove default focus outline */
        box-shadow: 0 0 6px 3px #ffbf87; /* Highlight focus */
}
</style>
