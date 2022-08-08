---
title: "Daniel Sánchez's CV"
author: Daniel Sánchez
date: "2022-08-08"
params:
  pdf_mode:
    value: false
output:
  pagedown::html_resume:
    css: ['dd_cv.css', 'resume']
    self_contained: true
---







Aside
================================================================================

<script id = "data_for_network" type = "application/json">{"nodes":[{"section":"Industry Positions","title":"Data Analyst","loc":"Valencia, Spain (remote)","institution":"Milmoh, S.L.","start":"2022","end":"Present","description_bullets":"- Data analyst position at a SaaS, fully remote company based in Spain.\n- Database management, wrangling and analysis as well as query editing in PostgreSQL, Power Query Editor and data connections to Power BI and Tableau Dashboards","start_year":"2022","end_year":"2032","no_start":false,"has_start":true,"no_end":false,"has_end":true,"id":1},{"section":"Education","title":"Bachelor of Arts in Finance","loc":"Quito, Ecuador","institution":"Universidad San Francisco de Quito","start":"2019","end":"2022","description_bullets":"- Second major. GPA 3.81/4.00 (Magna Cum Laude).","start_year":"2019","end_year":"2022","no_start":false,"has_start":true,"no_end":false,"has_end":true,"id":2},{"section":"About Me Press","title":"TV Interview on \"The Daily Rip\" about my comet discovery","loc":"N/A","institution":"Washington DC, USA","start":"2022","end":"2022","description_bullets":"- Invited for a TV interview on the popular TV show \"The Daily Rip\" hosted by Jack and Brie\n- I appeared on the same segment as the news on Ariana Grande's and DJ Chello's breakup","start_year":"2022","end_year":"2022","no_start":false,"has_start":true,"no_end":false,"has_end":true,"id":3},{"section":"Academic Articles","title":"Discovery of a novel comet, Comet Dibiasky, heading towards Earth","loc":"Nature","institution":"N/A","start":"2022","end":"2022","description_bullets":"- Dibiasky and Mindy","start_year":"2022","end_year":"2022","no_start":false,"has_start":true,"no_end":false,"has_end":true,"id":4},{"section":"Education","title":"Bachelor of Arts in Economics","loc":"Quito, Ecuador","institution":"Universidad San Francisco de Quito","start":"2017","end":"2021","description_bullets":"- GPA 3.82/4.00 (Magna Cum Laude)","start_year":"2017","end_year":"2021","no_start":false,"has_start":true,"no_end":false,"has_end":true,"id":5},{"section":"Education","title":"Exchange Program ","loc":"East Lansing, Michigan, USA","institution":"Michigan State University","start":"2019","end":"2020","description_bullets":"- GPA 3.92/4.00","start_year":"2019","end_year":"2020","no_start":false,"has_start":true,"no_end":false,"has_end":true,"id":6},{"section":"Teaching Positions","title":"Javascript for Shiny Users","loc":"RStudio::conf 2020","institution":"N/A","start":"N/A","end":"2020","description_bullets":"- Served as TA for two day workshop on how to leverage Javascript in Shiny applications\n- Lectured on [using R2D3 package to build interactive visualizations.](http://nickstrayer.me/js4shiny_r2d3/slides)","start_year":"2032","end_year":"2020","no_start":true,"has_start":false,"no_end":false,"has_end":true,"id":7},{"section":"Teaching Positions","title":"Statistical Computing in R","loc":"Vanderbilt Biostatistics Department","institution":"Nashville, TN","start":"2017","end":"2017","description_bullets":"- TA and lectured\n- Covered introduction to R language for statistics applications\n- Graduate level class","start_year":"2017","end_year":"2017","no_start":false,"has_start":true,"no_end":false,"has_end":true,"id":8},{"section":"Industry Positions","title":"Research Intern","loc":"Location","institution":"Institution","start":"2012","end":"2015","description_bullets":"- 3-month internship in the research division of one of the top supervisory and regulatory institution in Ecuador\n- Contributed to the production and review of research paper written by authors in the division, which involved econometric modelling and literature reviews on key issues about the Ecuadorian economy.\n- Wrote a research paper which was submitted and published at the institutions academic journal, [X-Pedientes Económicos](https://ojs.supercias.gob.ec/index.php/X-pedientes_Economicos).","start_year":"2012","end_year":"2015","no_start":false,"has_start":true,"no_end":false,"has_end":true,"id":9}],"edges":[{"year":2022,"source":1,"target":2},{"year":2022,"source":1,"target":3},{"year":2022,"source":1,"target":4},{"year":2022,"source":1,"target":7},{"year":2022,"source":2,"target":3},{"year":2022,"source":2,"target":4},{"year":2022,"source":2,"target":7},{"year":2022,"source":3,"target":4},{"year":2022,"source":3,"target":7},{"year":2022,"source":4,"target":7},{"year":2019,"source":2,"target":5},{"year":2019,"source":2,"target":6},{"year":2019,"source":5,"target":6},{"year":2020,"source":2,"target":5},{"year":2020,"source":2,"target":6},{"year":2020,"source":2,"target":7},{"year":2020,"source":5,"target":6},{"year":2020,"source":5,"target":7},{"year":2020,"source":6,"target":7},{"year":2021,"source":2,"target":5},{"year":2021,"source":2,"target":7},{"year":2021,"source":5,"target":7},{"year":2017,"source":5,"target":8}]}</script><script src="https://cdnjs.cloudflare.com/ajax/libs/d3/5.16.0/d3.min.js"></script><svg style = "width: 100%; height:320px; margin-top: -125px;" id = "cv_network_viz"></svg><script>const data_json = document.querySelector("script[type='application/json']").textContent;
const {edges, nodes} = JSON.parse(data_json);

class MyHandler extends Paged.Handler {
	constructor(chunker, polisher, caller) {
		super(chunker, polisher, caller);
	}

	afterRendered (){
		plot_network();
	}
}
Paged.registerHandlers(MyHandler);

function plot_network(){
  const {width, height} = document.querySelector("svg#cv_network_viz").getBoundingClientRect();

  const svg = d3.select("svg#cv_network_viz")
    .attr("width", width)
    .attr("height", height);

  const unique_sections = [...new Set(nodes.map(d => d.section))];
  const color_scale = d3.scaleOrdinal()
    .domain(unique_sections)
    .range(d3.schemeSet2);

  const edge_color = d3.scaleLinear()
    .domain(d3.extent(edges, d => d.year));

  const simulation = d3.forceSimulation(nodes)
    .force("link", d3.forceLink(edges).id(d => d.id))
    .force("charge", d3.forceManyBody())
    .force("center", d3.forceCenter(width / 2, height / 2))
    .on("tick", ticked);

  const g = svg.append("g");

  const link = g
    .selectAll("line")
    .data(edges)
    .enter().append("line")
      .attr("stroke", d => d3.interpolateGreys(edge_color(d.year)))
      .attr("stroke-width", 0.5);

  const node = g
    .attr("stroke", "#fff")
    .attr("stroke-width", 1.5)
    .selectAll("circle")
    .data(nodes)
    .enter().append("circle")
      .attr("r", 5)
      .attr("fill", d => color_scale(d.section))
      .call(drag(simulation));

  node.append("title")
      .text(d => `${d.section}\n${d.title}`);

  svg.call(d3.zoom()
      .extent([[0, 0], [width, height]])
      .scaleExtent([1, 8])
      .on("zoom", zoomed));

  function ticked() {
    link
        .attr("x1", d => d.source.x)
        .attr("y1", d => d.source.y)
        .attr("x2", d => d.target.x)
        .attr("y2", d => d.target.y);

    node
        .attr("cx", d => d.x)
        .attr("cy", d => d.y);
  }

  function zoomed() {
    g.attr("transform", d3.event.transform);
  }

  function drag(simulation){

    function dragstarted(d) {
      if (!d3.event.active) simulation.alphaTarget(0.3).restart();
      d.fx = d.x;
      d.fy = d.y;
    }

    function dragged(d) {
      d.fx = d3.event.x;
      d.fy = d3.event.y;
    }

    function dragended(d) {
      if (!d3.event.active) simulation.alphaTarget(0);
      d.fx = null;
      d.fy = null;
    }

    return d3.drag()
        .on("start", dragstarted)
        .on("drag", dragged)
        .on("end", dragended);
  }

}
</script>


[<i class='fas fa-download'></i> Download a PDF of this CV](https://github.com/dsanchezp18/automated-cv-dsp/daniel_sanchez_cv_english.pdf)

Contact {#contact}
--------------------------------------------------------------------------------

- <i class='fa fa-envelope'></i> kdibiasky@gmail.com
- <i class='fa fa-twitter'></i> DontLookUpFilm
- <i class='fa fa-github'></i> github.com/shaziaruybal/automate-cv-rmd



Language Skills {#skills}
--------------------------------------------------------------------------------

<div
  class = 'skill-bar'
  style = "background:linear-gradient(to right,
                                      #969696 100%,
                                      #d9d9d9 100% 100%)"
>R</div>
<div
  class = 'skill-bar'
  style = "background:linear-gradient(to right,
                                      #969696 60%,
                                      #d9d9d9 60% 100%)"
>Excel</div>
<div
  class = 'skill-bar'
  style = "background:linear-gradient(to right,
                                      #969696 60%,
                                      #d9d9d9 60% 100%)"
>Power BI</div>
<div
  class = 'skill-bar'
  style = "background:linear-gradient(to right,
                                      #969696 60%,
                                      #d9d9d9 60% 100%)"
>SQL</div>
<div
  class = 'skill-bar'
  style = "background:linear-gradient(to right,
                                      #969696 60%,
                                      #d9d9d9 60% 100%)"
>Tableau</div>
<div
  class = 'skill-bar'
  style = "background:linear-gradient(to right,
                                      #969696 60%,
                                      #d9d9d9 60% 100%)"
>LaTeX</div>
<div
  class = 'skill-bar'
  style = "background:linear-gradient(to right,
                                      #969696 30%,
                                      #d9d9d9 30% 100%)"
>Stata</div>
<div
  class = 'skill-bar'
  style = "background:linear-gradient(to right,
                                      #969696 30%,
                                      #d9d9d9 30% 100%)"
>Minitab</div>
<div
  class = 'skill-bar'
  style = "background:linear-gradient(to right,
                                      #969696 10%,
                                      #d9d9d9 10% 100%)"
>Git</div>
<div
  class = 'skill-bar'
  style = "background:linear-gradient(to right,
                                      #969696 10%,
                                      #d9d9d9 10% 100%)"
>Python</div>
<div
  class = 'skill-bar'
  style = "background:linear-gradient(to right,
                                      #969696 60%,
                                      #d9d9d9 60% 100%)"
>VBA</div>



Disclaimer {#disclaimer}
--------------------------------------------------------------------------------

Made with the R package [**pagedown**](https://github.com/rstudio/pagedown). 

The source code is available [on github.com/nstrayer/cv](https://github.com/dsanchezp18/automated-cv-dsp).

Last updated on 2022-08-08.



Main
================================================================================

Daniel Sánchez {#title}
--------------------------------------------------------------------------------

Bachelor of Arts in Economics and Bachelor of Arts in Finance

Education {data-icon=graduation-cap data-concise=true}
--------------------------------------------------------------------------------

### Bachelor of Arts in Finance

Quito, Ecuador

Universidad San Francisco de Quito

2022 - 2019

- Second major. GPA 3.81/4.00 (Magna Cum Laude).



### Bachelor of Arts in Economics

Quito, Ecuador

Universidad San Francisco de Quito

2021 - 2017

- GPA 3.82/4.00 (Magna Cum Laude)



### Exchange Program (MSU Economics)

East Lansing, Michigan, USA

Michigan State University

2020 - 2019

- GPA 3.92/4.00

Industry Experience {data-icon=suitcase}
--------------------------------------------------------------------------------

::: aside
NA
:::

### Data Analyst

Valencia, Spain (remote)

Milmoh, S.L.

Present - 2022

- Data analyst position at a SaaS, fully remote company based in Spain.
- Database management, wrangling and analysis as well as query editing in PostgreSQL, Power Query Editor and data connections to Power BI and Tableau Dashboards



### Research Intern

Location

Institution

2015 - 2012

- 3-month internship in the research division of one of the top supervisory and regulatory institution in Ecuador
- Contributed to the production and review of research paper written by authors in the division, which involved econometric modelling and literature reviews on key issues about the Ecuadorian economy.
- Wrote a research paper which was submitted and published at the institutions academic journal, [X-Pedientes Económicos](https://ojs.supercias.gob.ec/index.php/X-pedientes_Economicos).

<!-- These breaks just force a new page so section doesnt get cut off -->
<br>
<br>
<br>



