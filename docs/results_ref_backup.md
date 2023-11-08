<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Stacked Area Chart</title>
    <style>
        .area {
            opacity: 0.6;
        }
        .axis path,
        .axis line {
            fill: none;
            stroke: #000;
            shape-rendering: crispEdges;
        }
    </style>
</head>
<body>
    <svg width="500" height="500"></svg>
    <script src="https://d3js.org/d3.v6.min.js"></script>
    <script>
// Set the dimensions of the canvas / graph
const margin = { top: 20, right: 20, bottom: 30, left: 50 },
      width = 500 - margin.left - margin.right,
      height = 300 - margin.top - margin.bottom;

// Parse the date / time
const parseDate = d3.timeParse("%Y");

// Set the ranges
const x = d3.scaleTime().range([0, width]);
const y = d3.scaleLinear().range([height, 0]);

// Define the area
const area = d3.area()
    .x(d => x(d.data.Branch))
    .y0(d => y(d[0]))
    .y1(d => y(d[1]));

// Define the stack
const stack = d3.stack();

// Create a tooltip div that is hidden by default:
const tooltip = d3.select("body").append("div")
    .attr("class", "tooltip")
    .style("opacity", 0)
    .style("position", "absolute")
    .style("text-align", "center")
    .style("width", "120px")
    .style("height", "28px")
    .style("padding", "2px")
    .style("font", "12px sans-serif")
    .style("background", "lightsteelblue")
    .style("border", "0px")
    .style("border-radius", "8px")
    .style("pointer-events", "none");

// Adds the svg canvas
const svg = d3.select("svg")
    .attr("width", width + margin.left + margin.right)
    .attr("height", height + margin.top + margin.bottom)
	.append("g")
    .attr("transform", `translate(${margin.left},${margin.top})`);

// Load the data
d3.csv("Ref-Emissions.csv").then(function(data) {

    // Coerce the data to numbers.
    data.forEach(d => {
        d.Branch = parseDate(d.Branch);
        d.Demand = +d.Demand;
        d.Transformation = +d.Transformation;
        d.NonEnergy = +d.NonEnergy;
    });

    // Stack the data
    const series = stack.keys(["Demand", "Transformation", "NonEnergy"])(data);
	
		


    // Scale the range of the data
    x.domain(d3.extent(data, d => d.Branch));
    y.domain([0, d3.max(series, d => d3.max(d, d => d[1]))]);

    // Add the stacked areas
    svg.selectAll(".area")
    .data(series)
    .enter().append("path")
    .attr("class", "area")
    .attr("d", area)
    .style("fill", (d, i) => d3.schemeCategory10[i])
    .on("mouseover", function(event, d) {
        tooltip.transition()
            .duration(200)
            .style("opacity", .9);
        tooltip.html(d.key) // Set the text of the tooltip to the key of the data series
            .style("left", (event.pageX) + "px")
            .style("top", (event.pageY - 28) + "px");
    })
    .on("mouseout", function(d) {
        tooltip.transition()
            .duration(500)
            .style("opacity", 0);
    });
		

    // Add the X Axis
    svg.append("g")
        .attr("transform", `translate(0,${height})`)
        .call(d3.axisBottom(x).ticks(d3.timeYear));
		
		

    // Add the Y Axis
    svg.append("g")
        .call(d3.axisLeft(y))
	svg.append("text")
    .attr("text-anchor", "middle")  // this makes it easy to center the text as the transform is applied
    .attr("transform", `translate(${-margin.left + 20},${height / 2})rotate(-90)`)  // text is drawn off the screen top left, move down and out and rotate
    .text("MtCO2eq");
		
});


    </script>
</body>
</html>
