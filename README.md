# TestRepository


<!doctype html>
<html lang="en">
  <head>
    <!-- Required meta tags -->
    <meta charset="utf-8">
    <meta name="viewport" content="width=device-width, initial-scale=1">

    <!-- Bootstrap CSS -->
    <link href="https://cdn.jsdelivr.net/npm/bootstrap@5.0.0-beta1/dist/css/bootstrap.min.css" rel="stylesheet" integrity="sha384-giJF6kkoqNQ00vy+HMDP7azOuL0xtbfIcaT9wjKHr8RbDVddVHyTfAAsrekwKmP1" crossorigin="anonymous">

    <title>Hello, world!</title>
  </head>
  <body class="bg-secondary">
    <h1>Fires By State</h1>
	<div class="row">
      <div class="col-md-2">
        <div class="well">
          <h5>Select a State:</h5>
          <select id="selDataset" onchange="optionChanged()" ></select>
        </div>
      </div>
	</div>
	
	<div class="row">
		<div class= "m-5">
			<div class="card col-md-6 offset-md-3">
				<canvas id="myChart"></canvas>
			</div>
		</div>
	</div>
	
	<script src="https://cdn.jsdelivr.net/npm/chart.js@2.8.0"></script>
	<script src="https://d3js.org/d3.v5.min.js"></script>	
	<script src="fires.js"></script>
    
  </body>
</html>




optionChanged()

function optionChanged() {

	d3.json("county_counts.json").then(function(fireData){
		
		d3.select("#myChart").remove()
		
		console.log("firedata", fireData)
		
		var abbr = fireData.map(function(d){
			return d.STATE
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
		
		
		var statefiltered = fireData.filter(function(data) {
            return String(data.STATE) === sel_val;
        });

		console.log("state filtered", statefiltered)
		
		var name = []
		var count = []
		var fipsName = []
		var amount = []

		var fips = ""

		statefiltered.forEach(function(data) {
		   
			name.push(data.FIPS_NAME)
			count.push(data.FIRE_COUNT)
		});

		console.log("name", name)
		console.log("count", count)
	
	d3.select("#myChart").remove()
	
	var ctx = document.getElementById('myChart').getContext('2d');
	
	
	var chart = new Chart(ctx, {
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
});

}

function onlyUnique(value,index, self) {
	return self.indexOf(value) === index;	
}







