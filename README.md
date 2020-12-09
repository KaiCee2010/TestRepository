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

 [
   {
     "cause": "Debris Burning", 
     "state": "AZ", 
     "total_causes": 1
   }, 
   {
     "cause": "Campfire", 
     "state": "WA", 
     "total_causes": 4
   }, 
   {
     "cause": "Smoking", 
     "state": "NM", 
     "total_causes": 3
   }, 
   {
     "cause": "Smoking", 
     "state": "SD", 
     "total_causes": 1
   }, 
   {
     "cause": "Equipment Use", 
     "state": "NV", 
     "total_causes": 7
   }, 
   {
     "cause": "Railroad", 
     "state": "CA", 
     "total_causes": 2
   }, 
   {
     "cause": "Debris Burning", 
     "state": "SD", 
     "total_causes": 2
   }, 
   {
     "cause": "Lightning", 
     "state": "WY", 
     "total_causes": 66
   }, 
   {
     "cause": "Debris Burning", 
     "state": "GA", 
     "total_causes": 4
   }, 
   {
     "cause": "Equipment Use", 
     "state": "SD", 
     "total_causes": 6
   }, 
   {
     "cause": "Equipment Use", 
     "state": "OK", 
     "total_causes": 2
   }, 
   {
     "cause": "Arson", 
     "state": "FL", 
     "total_causes": 12
   }, 
   {
     "cause": "Campfire", 
     "state": "MT", 
     "total_causes": 4
   }, 
   {
     "cause": "Missing/Undefined", 
     "state": "KS", 
     "total_causes": 22
   }, 
   {
     "cause": "Arson", 
     "state": "TN", 
     "total_causes": 1
   }, 
   {
     "cause": "Powerline", 
     "state": "WY", 
     "total_causes": 2
   }, 
   {
     "cause": "Equipment Use", 
     "state": "NM", 
     "total_causes": 13
   }, 
   {
     "cause": "Miscellaneous", 
     "state": "ND", 
     "total_causes": 2
   }, 
   {
     "cause": "Fireworks", 
     "state": "OK", 
     "total_causes": 1
   }, 
   {
     "cause": "Arson", 
     "state": "MN", 
     "total_causes": 6
   }, 
   {
     "cause": "Missing/Undefined", 
     "state": "ID", 
     "total_causes": 2
   }, 
   {
     "cause": "Debris Burning", 
     "state": "AK", 
     "total_causes": 6
   }, 
   {
     "cause": "Miscellaneous", 
     "state": "AK", 
     "total_causes": 5
   }, 
   {
     "cause": "Campfire", 
     "state": "AK", 
     "total_causes": 2
   }, 
   {
     "cause": "Campfire", 
     "state": "NC", 
     "total_causes": 1
   }, 
   {
     "cause": "Equipment Use", 
     "state": "CA", 
     "total_causes": 29
   }, 
   {
     "cause": "Arson", 
     "state": "SD", 
     "total_causes": 1
   }, 
   {
     "cause": "Miscellaneous", 
     "state": "TX", 
     "total_causes": 86
   }, 
   {
     "cause": "Debris Burning", 
     "state": "TX", 
     "total_causes": 7
   }, 
   {
     "cause": "Equipment Use", 
     "state": "WY", 
     "total_causes": 3
   }, 
   {
     "cause": "Arson", 
     "state": "CO", 
     "total_causes": 1
   }, 
   {
     "cause": "Miscellaneous", 
     "state": "GA", 
     "total_causes": 1
   }, 
   {
     "cause": "Arson", 
     "state": "ND", 
     "total_causes": 1
   }, 
   {
     "cause": "Campfire", 
     "state": "ND", 
     "total_causes": 1
   }, 
   {
     "cause": "Lightning", 
     "state": "MN", 
     "total_causes": 2
   }, 
   {
     "cause": "Equipment Use", 
     "state": "ID", 
     "total_causes": 14
   }, 
   {
     "cause": "Debris Burning", 
     "state": "ID", 
     "total_causes": 7
   }, 
   {
     "cause": "Equipment Use", 
     "state": "NE", 
     "total_causes": 1
   }, 
   {
     "cause": "Miscellaneous", 
     "state": "NE", 
     "total_causes": 1
   }, 
   {
     "cause": "Children", 
     "state": "CA", 
     "total_causes": 3
   }, 
   {
     "cause": "Structure", 
     "state": "NM", 
     "total_causes": 1
   }, 
   {
     "cause": "Campfire", 
     "state": "UT", 
     "total_causes": 3
   }, 
   {
     "cause": "Missing/Undefined", 
     "state": "AK", 
     "total_causes": 9
   }, 
   {
     "cause": "Equipment Use", 
     "state": "WI", 
     "total_causes": 1
   }, 
   {
     "cause": "Smoking", 
     "state": "OK", 
     "total_causes": 2
   }, 
   {
     "cause": "Fireworks", 
     "state": "SD", 
     "total_causes": 2
   }, 
   {
     "cause": "Debris Burning", 
     "state": "CA", 
     "total_causes": 8
   }, 
   {
     "cause": "Powerline", 
     "state": "TX", 
     "total_causes": 23
   }, 
   {
     "cause": "Missing/Undefined", 
     "state": "NV", 
     "total_causes": 3
   }, 
   {
     "cause": "Missing/Undefined", 
     "state": "OK", 
     "total_causes": 27
   }, 
   {
     "cause": "Debris Burning", 
     "state": "OR", 
     "total_causes": 2
   }, 
   {
     "cause": "Arson", 
     "state": "OR", 
     "total_causes": 3
   }, 
   {
     "cause": "Campfire", 
     "state": "AZ", 
     "total_causes": 17
   }, 
   {
     "cause": "Campfire", 
     "state": "CA", 
     "total_causes": 13
   }, 
   {
     "cause": "Campfire", 
     "state": "NM", 
     "total_causes": 6
   }, 
   {
     "cause": "Lightning", 
     "state": "CA", 
     "total_causes": 125
   }, 
   {
     "cause": "Structure", 
     "state": "CA", 
     "total_causes": 1
   }, 
   {
     "cause": "Powerline", 
     "state": "CO", 
     "total_causes": 1
   }, 
   {
     "cause": "Lightning", 
     "state": "OR", 
     "total_causes": 176
   }, 
   {
     "cause": "Powerline", 
     "state": "NE", 
     "total_causes": 2
   }, 
   {
     "cause": "Structure", 
     "state": "SD", 
     "total_causes": 1
   }, 
   {
     "cause": "Debris Burning", 
     "state": "SC", 
     "total_causes": 1
   }, 
   {
     "cause": "Equipment Use", 
     "state": "OR", 
     "total_causes": 9
   }, 
   {
     "cause": "Railroad", 
     "state": "VA", 
     "total_causes": 1
   }, 
   {
     "cause": "Powerline", 
     "state": "NM", 
     "total_causes": 5
   }, 
   {
     "cause": "Equipment Use", 
     "state": "AK", 
     "total_causes": 3
   }, 
   {
     "cause": "Arson", 
     "state": "OK", 
     "total_causes": 14
   }, 
   {
     "cause": "Equipment Use", 
     "state": "MT", 
     "total_causes": 10
   }, 
   {
     "cause": "Arson", 
     "state": "WA", 
     "total_causes": 6
   }, 
   {
     "cause": "Lightning", 
     "state": "WA", 
     "total_causes": 63
   }, 
   {
     "cause": "Debris Burning", 
     "state": "NV", 
     "total_causes": 3
   }, 
   {
     "cause": "Powerline", 
     "state": "UT", 
     "total_causes": 1
   }, 
   {
     "cause": "Equipment Use", 
     "state": "FL", 
     "total_causes": 11
   }, 
   {
     "cause": "Equipment Use", 
     "state": "AZ", 
     "total_causes": 4
   }, 
   {
     "cause": "Equipment Use", 
     "state": "TX", 
     "total_causes": 18
   }, 
   {
     "cause": "Lightning", 
     "state": "AK", 
     "total_causes": 504
   }, 
   {
     "cause": "Children", 
     "state": "OR", 
     "total_causes": 2
   }, 
   {
     "cause": "Missing/Undefined", 
     "state": "OR", 
     "total_causes": 1
   }, 
   {
     "cause": "Miscellaneous", 
     "state": "AZ", 
     "total_causes": 21
   }, 
   {
     "cause": "Children", 
     "state": "FL", 
     "total_causes": 1
   }, 
   {
     "cause": "Arson", 
     "state": "LA", 
     "total_causes": 1
   }, 
   {
     "cause": "Miscellaneous", 
     "state": "SD", 
     "total_causes": 8
   }, 
   {
     "cause": "Lightning", 
     "state": "GA", 
     "total_causes": 4
   }, 
   {
     "cause": "Miscellaneous", 
     "state": "NM", 
     "total_causes": 26
   }, 
   {
     "cause": "Missing/Undefined", 
     "state": "WA", 
     "total_causes": 19
   }, 
   {
     "cause": "Powerline", 
     "state": "OK", 
     "total_causes": 2
   }, 
   {
     "cause": "Children", 
     "state": "SD", 
     "total_causes": 1
   }, 
   {
     "cause": "Lightning", 
     "state": "FL", 
     "total_causes": 46
   }, 
   {
     "cause": "Campfire", 
     "state": "CO", 
     "total_causes": 1
   }, 
   {
     "cause": "Debris Burning", 
     "state": "CO", 
     "total_causes": 2
   }, 
   {
     "cause": "Debris Burning", 
     "state": "MI", 
     "total_causes": 2
   }, 
   {
     "cause": "Miscellaneous", 
     "state": "OR", 
     "total_causes": 12
   }, 
   {
     "cause": "Structure", 
     "state": "WA", 
     "total_causes": 1
   }, 
   {
     "cause": "Debris Burning", 
     "state": "KS", 
     "total_causes": 1
   }, 
   {
     "cause": "Missing/Undefined", 
     "state": "MN", 
     "total_causes": 1
   }, 
   {
     "cause": "Lightning", 
     "state": "VA", 
     "total_causes": 1
   }, 
   {
     "cause": "Missing/Undefined", 
     "state": "CA", 
     "total_causes": 21
   }, 
   {
     "cause": "Miscellaneous", 
     "state": "KS", 
     "total_causes": 1
   }, 
   {
     "cause": "Smoking", 
     "state": "CO", 
     "total_causes": 1
   }, 
   {
     "cause": "Miscellaneous", 
     "state": "CA", 
     "total_causes": 60
   }, 
   {
     "cause": "Arson", 
     "state": "NC", 
     "total_causes": 1
   }, 
   {
     "cause": "Powerline", 
     "state": "WA", 
     "total_causes": 2
   }, 
   {
     "cause": "Lightning", 
     "state": "CO", 
     "total_causes": 35
   }, 
   {
     "cause": "Arson", 
     "state": "CA", 
     "total_causes": 21
   }, 
   {
     "cause": "Missing/Undefined", 
     "state": "WY", 
     "total_causes": 4
   }, 
   {
     "cause": "Campfire", 
     "state": "WY", 
     "total_causes": 2
   }, 
   {
     "cause": "Missing/Undefined", 
     "state": "NM", 
     "total_causes": 15
   }, 
   {
     "cause": "Debris Burning", 
     "state": "MN", 
     "total_causes": 5
   }, 
   {
     "cause": "Equipment Use", 
     "state": "KS", 
     "total_causes": 1
   }, 
   {
     "cause": "Powerline", 
     "state": "MT", 
     "total_causes": 2
   }, 
   {
     "cause": "Campfire", 
     "state": "VA", 
     "total_causes": 1
   }, 
   {
     "cause": "Arson", 
     "state": "UT", 
     "total_causes": 2
   }, 
   {
     "cause": "Campfire", 
     "state": "TX", 
     "total_causes": 2
   }, 
   {
     "cause": "Miscellaneous", 
     "state": "CO", 
     "total_causes": 8
   }, 
   {
     "cause": "Campfire", 
     "state": "OR", 
     "total_causes": 1
   }, 
   {
     "cause": "Missing/Undefined", 
     "state": "CO", 
     "total_causes": 11
   }, 
   {
     "cause": "Campfire", 
     "state": "ID", 
     "total_causes": 5
   }, 
   {
     "cause": "Lightning", 
     "state": "NC", 
     "total_causes": 5
   }, 
   {
     "cause": "Miscellaneous", 
     "state": "FL", 
     "total_causes": 7
   }, 
   {
     "cause": "Miscellaneous", 
     "state": "MN", 
     "total_causes": 5
   }, 
   {
     "cause": "Lightning", 
     "state": "MT", 
     "total_causes": 149
   }, 
   {
     "cause": "Lightning", 
     "state": "SD", 
     "total_causes": 27
   }, 
   {
     "cause": "Powerline", 
     "state": "CA", 
     "total_causes": 1
   }, 
   {
     "cause": "Arson", 
     "state": "VA", 
     "total_causes": 2
   }, 
   {
     "cause": "Lightning", 
     "state": "AZ", 
     "total_causes": 87
   }, 
   {
     "cause": "Lightning", 
     "state": "NM", 
     "total_causes": 86
   }, 
   {
     "cause": "Lightning", 
     "state": "ID", 
     "total_causes": 246
   }, 
   {
     "cause": "Smoking", 
     "state": "WA", 
     "total_causes": 1
   }, 
   {
     "cause": "Missing/Undefined", 
     "state": "FL", 
     "total_causes": 3
   }, 
   {
     "cause": "Debris Burning", 
     "state": "WY", 
     "total_causes": 2
   }, 
   {
     "cause": "Railroad", 
     "state": "AZ", 
     "total_causes": 1
   }, 
   {
     "cause": "Railroad", 
     "state": "FL", 
     "total_causes": 3
   }, 
   {
     "cause": "Miscellaneous", 
     "state": "UT", 
     "total_causes": 17
   }, 
   {
     "cause": "Lightning", 
     "state": "ND", 
     "total_causes": 2
   }, 
   {
     "cause": "Railroad", 
     "state": "ND", 
     "total_causes": 1
   }, 
   {
     "cause": "Arson", 
     "state": "NM", 
     "total_causes": 7
   }, 
   {
     "cause": "Debris Burning", 
     "state": "WA", 
     "total_causes": 2
   }, 
   {
     "cause": "Arson", 
     "state": "TX", 
     "total_causes": 2
   }, 
   {
     "cause": "Miscellaneous", 
     "state": "LA", 
     "total_causes": 2
   }, 
   {
     "cause": "Lightning", 
     "state": "UT", 
     "total_causes": 62
   }, 
   {
     "cause": "Powerline", 
     "state": "ID", 
     "total_causes": 3
   }, 
   {
     "cause": "Arson", 
     "state": "AK", 
     "total_causes": 4
   }, 
   {
     "cause": "Debris Burning", 
     "state": "FL", 
     "total_causes": 6
   }, 
   {
     "cause": "Missing/Undefined", 
     "state": "MT", 
     "total_causes": 1
   }, 
   {
     "cause": "Miscellaneous", 
     "state": "WY", 
     "total_causes": 6
   }, 
   {
     "cause": "Lightning", 
     "state": "LA", 
     "total_causes": 11
   }, 
   {
     "cause": "Miscellaneous", 
     "state": "ID", 
     "total_causes": 17
   }
  
 ]





