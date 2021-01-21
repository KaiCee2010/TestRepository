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


# New stuff

data cleanup

# Import standard library modules
import arcpy, os, sys
from arcpy import env
import datetime


# Allow for file overwrite
arcpy.env.overwriteOutput = True

# Set the workspace directory 
workspace = r"C:\Users\kcyoung\Downloads\Drought" 



# Get the list of the featureclasses to process
# fc_tables = arcpy.ListFeatureClasses()
# folder_name = ['Set1', 'Set2']
# print(fc_tables)

feature_classes = []

walk = arcpy.da.Walk(workspace, datatype="FeatureClass", type="Polygon")

filePath = os.path.join("C:", "users", "kcyoung", "downloads","Drought")
for dirpath, dirnames, filenames in walk:
    fc_tables = arcpy.ListFeatureClasses()
    print(dirpath)
    print(dirnames)
    print(filenames)
    for filename in filenames:
        # Define field name and expression
        field1 = "FILENAME"
        field2 = "START_DATE"
        field3 = "END_DATE"


        # Create a new field with a new name
        arcpy.AddField_management(filename,field1,"TEXT")
        arcpy.AddField_management(filename,field2,"TEXT")
        arcpy.AddField_management(filename,field3,"TEXT")

#         # Calculate field here
#         arcpy.CalculateField_management(filename, field1, '"'+fName+'"', "PYTHON")
#         arcpy.CalculateField_management(filename, field2, '"'+startDate+'"', "PYTHON")
#         arcpy.CalculateField_management(filename, field3, '"'+endDate+'"', "PYTHON")

    

# for dirpath, dirnames, filenames in walk:
#     for filename in filenames:
#         print("processing " + str(filename))
        
#         fName=str(filename)
#         fName=fName.replace(".shp", "")
#         fName=fName.replace("USDM_", "")
#         print(fName)

#         eDate = datetime.datetime.strptime(fName, "%Y%m%d").date()
#         print("end date", eDate)
#         sDate = eDate - datetime.timedelta(days=7)
#         print("start date", sDate)
        
#         endDate = str(eDate)
#         startDate = str(sDate)

#         # Define field name and expression
#         field1 = "FILENAME"
#         field2 = "START_DATE"
#         field3 = "END_DATE"


#         # Create a new field with a new name
#         arcpy.AddField_management(filename,field1,"TEXT")
#         arcpy.AddField_management(filename,field2,"TEXT")
#         arcpy.AddField_management(filename,field3,"TEXT")

#         # Calculate field here
#         arcpy.CalculateField_management(filename, field1, '"'+fName+'"', "PYTHON")
#         arcpy.CalculateField_management(filename, field2, '"'+startDate+'"', "PYTHON")
#         arcpy.CalculateField_management(filename, field3, '"'+endDate+'"', "PYTHON")
        
#         feature_classes.append(os.path.join(dirpath, filename))
        
# location = r"C:\Users\kcyoung\Downloads\Data.gdb\Sets"
# arcpy.Merge_management(feature_classes, location)

# for dirpath, dirnames, filenames in arcpy.da.Walk(workspace,datatype="FeatureClass", type="Polygon"):
#     # Loop through each file and perform the processing
#     for filename in filenames:
#         print("processing " + str(fc))
#         fName=str(filename)
#         fName=fName.replace(".shp", "")
#         fName=fName.replace("USDM_", "")
#         print(fName)

#         eDate = datetime.datetime.strptime(fName, "%Y%m%d").date()
#         print("end date", eDate)
#         sDate = eDate - datetime.timedelta(days=7)
#         print("start date", sDate)
        
#         desc = arcpy.Describe(os.path.join(dirpath, filename)  
#         feature_classes.append(os.path.join(dirpath, filename))
        
    

#         endDate = str(eDate)
#         startDate = str(sDate)

#         # Define field name and expression
#         field1 = "FILENAME"
#         field2 = "START_DATE"
#         field3 = "END_DATE"


#         # Create a new field with a new name
#         arcpy.AddField_management(fc,field1,"TEXT")
#         arcpy.AddField_management(fc,field2,"TEXT")
#         arcpy.AddField_management(fc,field3,"TEXT")

#         # Calculate field here
#         arcpy.CalculateField_management(fc, field1, '"'+fName+'"', "PYTHON")
#         arcpy.CalculateField_management(fc, field2, '"'+startDate+'"', "PYTHON")
#         arcpy.CalculateField_management(fc, field3, '"'+endDate+'"', "PYTHON")

