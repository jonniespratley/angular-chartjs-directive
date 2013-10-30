# ng-chartjs-directive
The AngularJS directive for ChartJS library.


![image](https://dl.dropboxusercontent.com/u/26906414/cdn/img/ng-chartjs-banner.png)




# * How to create an AngularJS Component
First find a cool library that you will most likey want to use in a web application. For this tutorial we are going to pick a very good HTML5 canvas charting library. It goes by the name of ChartJS, it has many charts and they are very easy to use. But using a AngularDirective will be easier, so lets create a component.



## Get Started
To quickly get started creating a custom component for angular, install the [AngularJS Component Generator](https://github.com/mgcrea/generator-angular-component). 

**Execute the following:**

	$ sudo npm install -g generator-angular-component
	
Now you'll be able to scaffold a angular component project.

### Step 1 - Create the project
Proceed to create your project folder and then `cd` into that directory.

	$ mkdir ng-chartjs-directive && cd ng-chartjs-directive
	
Now use Yeoman to create your project files

**Execute the following:**

	$ yo angular-component
	
Then proceed to answer a few questions about your project.

	[?] Would you mind telling me your username on Github? jonniespratley
	[?] What's the base name of your project? angular-rickshaw-directive
	[?] Under which lincense your project shall be released? MIT
	[?] Do you need the unstable branch of AngularJS? Yes
	[?] Does your module requires CSS styles? Yes

Now you are ready to start development on your new AngularJS component.

### Step 2 - Register with Bower
For the component to be available via `bower` register the component, execute the following:

	$ bower register [name] [endpoint]

Now your component is available to the world via the `bower` package manager.





---


# * How to Use
This is how to use this directive in your application

#### 1. Download or Use Bower 

Download the [production version][min] or the [development version][max].

[min]: https://raw.github.com/jonniespratley/ng-chartjs-directive/master/dist/ng-chartjs-directive.min.js
[max]: https://raw.github.com/jonniespratley/ng-chartjs-directive/master/dist/ng-chartjs-directive.js

Or use bower package manager:

	bower install ng-chartjs-directive --save



#### 2. Add Dependency

In the `index.html` page:

	<script src="angular.js"></script>
	<script src="dist/ng-chartjs-directive.min.js"></script>
	
	
In the `app.js` file add `jonniespratley.ngChartjsDirective` to the dependencies.

	var app = angular.module('app', [
		'jonniespratley.ngChartjsDirective'
	]);

#### 4. Add Markup
Now add the following markup to your view.

	<chart-js id="chart1"
			width="{{chart.width}}"
			height="{{chart.height}}"
			title="{{chart.title}}"
			type="{{chart.type}}"
			data="{{chart.data}}"
			options="{{chart.options}}"
			ng-model="chart.data">
			
			<!-- Fallback -->
			You do not support the HTML5 Canvas.
			
	</chart-js>


#### 5. Add Configuration
Now add the configuration inside of your controller.

	$scope.chart = {
	  "title": "Chart",
	  "type": "Bar",
	  "height": 300,
	  "width": 600,
	  "options": {
	    "chart": {
	      "events": {}
	    }
	  },
	  "data": {
	    "labels": [
	      "January",
	      "February",
	      "March",
	      "April",
	      "May",
	      "June",
	      "July"
	    ],
	    "datasets": [
	      {
	        "fillColor": "rgba(220,220,220,0.5)",
	        "strokeColor": "rgba(220,220,220,1)",
	        "pointColor": "rgba(220,220,220,1)",
	        "pointStrokeColor": "#fff",
	        "data": [
	          65,
	          59,
	          90,
	          81,
	          56,
	          55,
	          40
	        ]
	      },
	      {
	        "fillColor": "rgba(151,187,205,0.5)",
	        "strokeColor": "rgba(151,187,205,1)",
	        "pointColor": "rgba(151,187,205,1)",
	        "pointStrokeColor": "#fff",
	        "data": [
	          28,
	          48,
	          40,
	          19,
	          96,
	          27,
	          100
	        ]
	      }
	    ]
	  }
	}




## Documentation
_(Coming soon)_


## Example
To view a example, open the `index.html` file located in the `www` directory in a web browser.

