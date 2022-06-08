# D3-challenge
# Data_Journalism_and_D3

## Tasks
Create a web page for a major metro paper to visualise the current trends shaping people's lives analysis, including creating charts, graphs, and interactive elements to help readers understand your findings.<br/>

Sniff out the health risks facing particular demographics by sifting through information from the U.S. Census Bureau and the Behavioural Risk Factor Surveillance System.<br/>

The data set is based on 2014 ACS 1-year estimates<br/>

**_Web page image_**<br/>
![full_page](images/screen_shot_full_page.png).<br/>

## Step by Step Approch

### Step 1 Create a scatter plot with a tooltip fuction

![tooltip](output/tooltip.gif)<br/>

* Pull in the data from `data.csv` by using the `d3.csv` function. <br/>
  ``` python
  d3.csv("assets/data/data.csv").then(function(data, err) {
  
  if (err) throw err;
  
  data.forEach(d => {
    d.poverty = +d.poverty;
    d.age = +d.age;
    d.income = +d.income;
    d.healthcare = +d.healthcare;
    d.obesity = +d.obesity;
    d.smokes = +d.smokes;
  });
  }
  ```
* Create a scatter plot that represents each state with circle elements, and include state abbreviations in the circles.<br/>
  ``` python
  var circlesText = circlesGroup.append("text")
    .text(d => d.abbr)
    .attr("dx", d => xLinearScale(d[XAxis]))
    .attr("dy", d => yLinearScale(d[YAxis])+5)
    .classed('stateText', true);
  ```
* Add a tooltip on each circle elements.<br/>
  ``` python
  // Initialize tool tip
  var toolTip = d3.tip()
    .attr("class", "tooltip")
    .offset([40, -60])
    .html(function (d) {
    return (`${d.state}<br>Poverty: ${d.poverty}%<br>Obesity: ${d.obesity}% `);
    });
  // call tooltip
  circlesGroup.call(toolTip);
  // Create event listeners
  circlesGroup.on("mouseover", function(data) {
    toolTip.show(data,this);
  })
  // onmouseout event
  .on("mouseout", function(data) {
  toolTip.hide(data);
  });
  ```
### Step 2 Create a interact scatter plot based on the aboce dataset

![animated-scatter](output/animated-scatter.gif)

**Main solution ideas:**<br/>

* Update the layout of the plot with variables.<br/>
  ``` python
  // x-label
  function renderXAxes(newXScale, xAxis) {
    var bottomAxis = d3.axisBottom(newXScale);
    xAxis.transition()
      .duration(1000)
      .call(bottomAxis);
    return xAxis;
  }
  // y-label
  function renderYAxes(newYScale, yAxis) {
    var leftAxis = d3.axisLeft(newYScale);
    yAxis.transition()
      .duration(1000)
      .call(leftAxis);
    return yAxis;
  }
  ```
* Update tooltip with if statement and variable.<br/>
  ``` python
  function updateToolTip(XAxis, YAxis, circlesGroup) {
  // update output text
  var xlabel;
  var ylabel;
  if (XAxis === "poverty") {
    xlabel = "Poverty:";
  }
  else if (XAxis === "age") {
    xlabel = "Age:";
  }
  else if (XAxis === "income"){
      xlabel = "Household income:"
  }
  if (YAxis === 'healthcare'){
      ylabel = "Health:"
  }
  else if (YAxis === 'obesity'){
      ylabel = "Obesity:"
  }
  else if (YAxis === 'smokes'){
      ylabel = "Smokes:"
  }
  ```
* Update with "active" & "inactive" and variables<br/>
  ``` python
  if (XAxis === "age") {
      AgeLabel
        .classed("active", true)
        .classed("inactive", false);
      PovertyLabel
        .classed("active", false)
        .classed("inactive", true);
      IncomeLabel
        .classed("active", false)
        .classed("inactive", true);
  }
  else if(XAxis === 'income'){
    IncomeLabel
      .classed("active", true)
      .classed("inactive", false);
    PovertyLabel
      .classed("active", false)
      .classed("inactive", true);
    AgeLabel
      .classed("active", false)
      .classed("inactive", true);
  }
  else {
    IncomeLabel
      .classed("active", false)
      .classed("inactive", true);
    AgeLabel
      .classed("active", false)
      .classed("inactive", true);
    PovertyLabel
      .classed("active", true)
      .classed("inactive", false);
  }
  ```
  
  ## Files
  [output](/output)<br/>
  - animated-scatter.gif<br/>
  - screen_shot_full_page.png<br/>
  - tooltip.gif<br/>

  [index.html](/index.html)<br/>

  [assets](/assets)<br/>
  - css<br/>
    - d3Style.css<br/>
    - style.css <br/>
  - data<br/>
    - data.csv<br/> 
  - js<br/>
    - app.js <br/>
