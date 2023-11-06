#Reference Scenario

<!DOCTYPE html>
<html lang="en">
<head>
	<script src="https://d3js.org/d3.v6.min.js"></script>
</head>
<md-main>
<script>
    var dataArray = [20, 40, 45];
		var canvas1 = d3.select("md-main")
						.append("svg")
						.attr("width", 500)
						.attr("height", 500)
            .attr("border","black")	
  	var canvas2 = d3.select("md-main")
            .append("svg")
						.attr("width", 500)
						.attr("height", 500)
            .attr("x", 550)
    var canvas3 = d3.select("md-main")
            .append("svg")
						.attr("width", 500)
						.attr("height", 500)
            .attr("x", 550)
            .attr("y", 500);
		var bars = canvas1.selectAll("rect")
					.data(dataArray)
					.enter()
						.append("rect")
						.attr("width", function(d){return d*10;})
						.attr("height", 50)
						.attr("y", function(d, i){ return i*100 });
    var bars = canvas2.selectAll("rect")
					  .data(dataArray)
					  .enter()
						.append("rect")
						.attr("width", function(d){return d*5;})
						.attr("height", 50)
						.attr("y", function(d, i){ return i*100 })
            .attr("fill","green");
    var bars = canvas3.selectAll("rect")
					  .data(dataArray)
					  .enter()
						.append("rect")
						.attr("width", function(d){return d*15;})
						.attr("height", 50)
						.attr("y", function(d, i){ return i*100 })
            .attr("fill","orange");
            
</script>

</md-main>
</html>

