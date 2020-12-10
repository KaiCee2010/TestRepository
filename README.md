# TestRepository



optionChanged()

function optionChanged() {

Promise.all([d3.json("county_counts.json"), d3.json("cause_counts.json")]).then(function(fireData){
		
		d3.select("#myBarChart").remove()
		d3.select("#myBarChart2").remove()
		d3.select("#myScatterChart").remove()
		
		console.log("firedata", fireData[0])
		
		console.log("firedata", fireData[1])
		
		var abbr = fireData[0].map(function(d){
			return d.state
		})
		
		var unique = abbr.filter(onlyUnique)
				
		console.log("state names", unique)
		
		d3.select("#selDataset")
			.selectAll("option")
			.data(unique)
			.enter()
			.append("option")
			.html(function(d) {
				return `${d}`;
			});
		
		var sel = document.getElementById('selDataset');
		var sel_val = sel.options[sel.selectedIndex].value
		console.log(sel_val)
		
		
		var statefiltered = fireData[0].filter(function(data) {
            return String(data.state) === sel_val;
        });

		console.log("state filtered", statefiltered)
		
		var name = []
		var count = []
		var fipsName = []
		var amount = []
		var fire_size = []
		var burning_days = []
		var dataset = []
		
		var x

		var fips = ""

		statefiltered.forEach(function(data) {
		   
			name.push(data.county)
			count.push(data.total_fires)
			var x = data.burning_days
			var y = data.fire_size
			
			var vals = {x: x, y: y}

			dataset.push(vals)
			
		});

		console.log("name", name)
		console.log("count", count)
		console.log("xy", vals)
	
	d3.select("#canvasBarChart")
		.append("canvas")
		.attr("id", "myBarChart")
		
	
	var ctxBar = document.getElementById('myBarChart').getContext('2d');
	
	
	var chart = new Chart(ctxBar, {
		// The type of chart we want to create
		type: 'bar',

		// The data for our dataset
		data: {
			// labels: ['January', 'February', 'March', 'April', 'May', 'June', 'July'],
			labels: name,
			datasets: [{
				label: 'Number of Fires By County',
				backgroundColor: 'rgb(255, 99, 132)',
				borderColor: 'rgb(255, 99, 132)',
				// data: [0, 10, 5, 2, 20, 30, 45]
				data: count
			}]
		},

		// Configuration options go here
		options: {}
	});
	
	
	var statefilteredCause = fireData[1].filter(function(data) {
            return String(data.state) === sel_val;
        });

		console.log("state filtered", statefiltered)
	
	var cause = []
	var causeCount = []
	var stateAbbr = []
	
	statefilteredCause.forEach(function(data) {
		   
			cause.push(data.cause)
			causeCount.push(data.total_causes)
			stateAbbr.push(data.state)
		});

		console.log("cause", cause)
		console.log("cause count", causeCount)
		console.log("state", stateAbbr)
		
	d3.select("#canvasBarChart2")
		.append("canvas")
		.attr("id", "myBarChart2")	
		
	var ctxBar2 = document.getElementById('myBarChart2').getContext('2d');
	
	
	var chart = new Chart(ctxBar2, {
		// The type of chart we want to create
		type: 'bar',

		// The data for our dataset
		data: {
			// labels: ['January', 'February', 'March', 'April', 'May', 'June', 'July'],
			labels: cause,
			datasets: [{
				label: 'Number of Fires By County',
				backgroundColor: 'rgb(255, 99, 132)',
				borderColor: 'rgb(255, 99, 132)',
				// data: [0, 10, 5, 2, 20, 30, 45]
				data: causeCount
			}]
		},

		// Configuration options go here
		options: {}
	});
	
	
	
	
	
	d3.select("#canvasScatterChart")
		.append("canvas")
		.attr("id", "myScatterChart")
	
	var ctxScatter = document.getElementById('myScatterChart').getContext('2d');
	
	
	var chart = new Chart(ctxScatter, {
		// The type of chart we want to create
		type: 'scatter',

		// The data for our dataset
		data: {
			// labels: ['January', 'February', 'March', 'April', 'May', 'June', 'July'],
			labels: "Scatter DataSet",
			datasets: [{
				label: 'Number of Fires By County',
				backgroundColor: 'rgb(255, 99, 132)',
				borderColor: 'rgb(255, 99, 132)',
				// data: [0, 10, 5, 2, 20, 30, 45]
				data: fireCombo
			}]
		},

		// Configuration options go here
		options: {}
	});
		
});

}

function onlyUnique(value,index, self) {
	return self.indexOf(value) === index;	
}
