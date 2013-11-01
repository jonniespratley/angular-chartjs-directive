# * How to create a AngularJS Component
For this tutorial we are going to pick a very good HTML5 canvas charting library. It goes by the name of ChartJS, it has many charts and they are very easy to use. 

![image](https://dl.dropboxusercontent.com/u/26906414/cdn/img/ng-chartjs-banner.png)

## Step 1 - Get Started

#### 1. Installing Yeoman
Installation is pretty straight forward, the official build script does all the hard work for you. 

To install `yo`, `grunt`, and `bower`, execute the following command:

	$ npm install -g yo grunt-cli bower


If you run into installation issues please visit the [Getting Started Wiki](https://github.com/yeoman/yeoman/wiki/Getting-Started).

	


#### 2. Installing Generators
Before development we need to install a generator, since we are going to utilize the AngularJS framework install the [AngularJS Component Generator](https://github.com/mgcrea/generator-angular-component). 


**Execute the following:**

	$ sudo npm install -g generator-angular-component
	
Now you'll be able to scaffold a angular component project.

For documentation and how to create custom generators please visit the [wiki](https://github.com/yeoman/yeoman/wiki/Generators).







## Step 2 - Create the project
Proceed to create your project folder and then `cd` into that directory.

	$ mkdir ng-chartjs-directive && cd ng-chartjs-directive
	
Now use Yeoman to create your project files

**Execute the following:**

	$ yo angular-component
	
Then proceed to answer a few questions about your project.

	[?] Would you mind telling me your username on Github? jonniespratley
	[?] What's the base name of your project? ng-chartjs-directive
	[?] Under which lincense your project shall be released? MIT
	[?] Do you need the unstable branch of AngularJS? Yes
	[?] Does your module requires CSS styles? Yes

For distribution, register the new project with [Bower](http://bower.io/) (a web library package manager).

**Execute the following:**

	$ bower register [component-name] [component-github]

Now the component is available to the world via the `bower` package manager. 

**To install the component:**

	$ bower install [component-name]

Now any other project can use this component.





## Step 3 - Development


To create a directive with [AngularJS](http://angularjs.org/) it is best to create a module for the directive, then attach the directive definition to your module instance. 

This allows users of the component to easily include the required `scripts` and declare the component in the existing applications dependencies array.


#### 3.1 - Module Definition
To define the module, use the `angular.module()` method to create a module instance, in this case the variable `_app` is the components module.

	# Module instance
	_app = angular.module('jonniespratley.ngChartjsDirective', [])

The `angular.module` is a global method for creating, registering and retrieving Angular modules. 

1. When passed two or more arguments, a new module is created. 

		angular.module('jonniespratley.ngChartjsDirective', [])

2. If passed only one argument, an existing module (the name passed as the first argument to module) is retrieved.

		angular.module('jonniespratley.ngChartjsDirective')

All modules that should be available to an application must be registered using this method.


##### 3.2 - Directive Definition
The directive definition is a function that returns an object with all properties set.
	
	_app.directive "chartJs", ->
	  scope:
	    id: "@"
	    title: "@"
	    type: "@"
	    width: "="
	    height: "="
	    options: "@"
	    data: "@"
	    ngModel: "="
	  restrict: "E"
	  replace: true
	  require: "?ngModel"
	  template: """
	    <div class="chartjs">
	      <legend ng-show="{{title}}">{{title}}</legend>
	      <div class="chartjs-wrap">
	        <canvas id="chart_{{id}}" width="{{width}}" height="{{height}}">
	          Content
	        </canvas>
	      </div>
	    </div>
	  """
	  
##### 3.3 - Link Function
This is the function that wires up the model to the template and builds the chart that is now in the display. If there is no model, then just return and stop execution, instead of causing an error and haulting the entire application.

	  link: (scope, element, attrs, ngModel) ->
    	return unless ngModel 
    
	
###### a. Static variables
We need to setup some variables to hold information about the chart such as the id, options, type and data, Also to deal with tying to resize the chart while a resize is already in progress.
   
	
    #STATIC variables
    CHART = 
      id: "#chart_" + attrs.id
      options: attrs.options
      type: 'bar'
      data: []
      
    # Generic date
    rtime = new Date(1, 1, 2000, 12, '00', '00')
    
    # Set timeout or not
    timeout = false
    
    # Delta to update against
    delta = 100
    


###### b. Resizing the chart
When the brower is resized we are going to update the charts height and width because the chart needs to be redrawn.
	    
	    #Build chart based on done resizing
	    resizeend = ->
	      if new Date() - rtime < delta
	        setTimeout resizeend, delta
	      else
	        timeout = false
	        buildChart()


###### c. Create the chart
The `createChart` method takes a number of arguments to create a chart instance then returns that instance.
	    
	    createChart = (id, type, data, options) ->
	      ctx = angular.element(id).get(0).getContext("2d")
	      defaults = angular.extend({}, options)
	      wrapper = angular.element(id).parent()
	      
	      #Set the size
	      scope.$apply ->
	        scope.width = ctx.width = wrapper.width()
	        scope.height = ctx.height = wrapper.height()
	
	      #Switch based on type
	      switch type
	        when "line"
	          return new Chart(ctx).Line(data, defaults)
	        when "bar"
	          return new Chart(ctx).Bar(data, defaults)
	        when "doughnut"
	          return new Chart(ctx).Doughnut(data, defaults)
	        when "pie"
	          return new Chart(ctx).Pie(data, defaults)
	        when "polar"
	          return new Chart(ctx).PolarArea(data, defaults)
	        when "radar"
	          return new Chart(ctx).Radar(data, defaults)
	        else
	          return new Chart(ctx).Bar(data, defaults)
	    
	    
	    
	    
###### d. Build the chart
The `buildChart` method waits 500 ms before invoking the `createChart` method which actually does the chart initializing.
	    
    buildChart = ->
      setTimeout (->
        createChart CHART.id, CHART.type, CHART.data, CHART.options
      ), 500
	    
###### e. Render the chart   
The `ngModel.$render` method is to specify how the UI needs to be updated when the model changes, in this case, we invoke the `buildChart` method to re-initialize the chart.

    ngModel.$render = ->
      console.log 'Model render'
      buildChart()

	    
###### f. Watch the window
Attach a event listener to the window for a resize event, then if the timeout is false, set the timeout variable to true and call the setTimeout function passing the resizeend method with the delta.

    angular.element(window).resize ->
      rtime = new Date()
      if timeout is false
        timeout = true
        setTimeout resizeend, delta
	    
###### g. Watch the type
The `attrs.$observe` method allows for watching when values change, in this case the type we are listening to and building the chart with the dyanmic type set.
	    
    attrs.$observe "type", (value) ->
      CHART.type = String(value).toLowerCase()
      buildChart()
	    
###### h. Watch the data
Watch the data attribute and update the chart when changes
	    
    attrs.$observe "data", (value) ->
      buildChart()
	

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
		width="chart.width" 
		height="chart.height" 
		title="{{chart.title}}" 
		type="{{chart.type}}"
		data="{{chart.data}}"
		ng-model="chart">
		
		<!-- Fallback -->
		You do not support the HTML5 Canvas.
		
	</chart-js>

#### 5. Add Configuration
Now add the configuration inside of your controller.

	$scope.chart =
	  title: "Chart"
	  type: "bar"
	  height: 300
	  width: 600
	  options:
	    chart:
	      events: {}
	  data:
	    labels: ["January", "February", "March", "April", "May", "June", "July"]
	    datasets: [
	      fillColor: "rgba(220,220,220,0.5)"
	      strokeColor: "rgba(220,220,220,1)"
	      pointColor: "rgba(220,220,220,1)"
	      pointStrokeColor: "#fff"
	      data: [65, 59, 90, 81, 56, 55, 40]
	    ,
	      fillColor: "rgba(151,187,205,0.5)"
	      strokeColor: "rgba(151,187,205,1)"
	      pointColor: "rgba(151,187,205,1)"
	      pointStrokeColor: "#fff"
	      data: [28, 48, 40, 19, 96, 27, 100]
	    ]





## Example
The code is available on [github](https://github.com/jonniespratley/ng-chartjs-directive) for any suggestions and a example is avaiable on [plunkr](http://embed.plnkr.co/QE2TLfiS9zabrTRKM0Vr/).

![image](https://dl.dropboxusercontent.com/u/26906414/cdn/img/ng-chartjs-directive-screenshot.png)