<template>
<div id="app">
    <div id="chart">
    </div>
</div>
</template>

<script>
import * as d3 from "d3";

export default {
    name: 'App',
    data(){
      return {
        population: [
          // {country: "China", value: 1398 },
          // {country: "EEUU", value: 328.2 },
          {country: "España", value: 46.94 },
          {country: "Francia", value: 67.06 },
          {country: "Alemania", value: 83.02 },
          {country: "Italia", value: 60.36 },
          {country: "UK", value: 66.65 },
        ]
      }
    },
    components: {},
    mounted() {
      const w = 500;
      const h = 500;

      const svg = d3
        .select("#chart")
        .append("svg")
        .attr("width", w)
        .attr("height", h);

      const sortedPopulation = this.population.sort((a, b) => (a.value > b.value ? 1 : -1));
      const color = d3.scaleOrdinal(d3.schemeTableau10);
      const maxPopulation = d3.max(sortedPopulation, o => o.value);

      const angleScale = d3
        .scaleLinear()
        .domain([0, maxPopulation])
        .range([0, 1.5 * Math.PI]);

      const arc = d3
        .arc()
        .innerRadius((d, i) => (i + 1) * 25)
        .outerRadius((d, i) => (i + 2) * 25)
        .startAngle(angleScale(0))
        .endAngle(d => angleScale(d.value));

      const g = svg.append("g");

      g.selectAll("path")
        .data(sortedPopulation)
        .enter()
        .append("path")
        .attr("d", arc)
        .attr("fill", (d, i) => color(i))
        .attr("stroke", "#FFF")
        .attr("stroke-width", "1px");

      g.selectAll("text")
        .data(this.population)
        .enter()
        .append("text")
        .text(d => `${d.country} -  ${d.value} M`)
        .attr("x", -150)
        .attr("dy", -8)
        .attr("y", (d, i) => -(i + 1) * 25);

      g.attr("transform", "translate(200,300)");
    }
}
</script>

<style>
#app {
    font-family: Avenir, Helvetica, Arial, sans-serif;
    -webkit-font-smoothing: antialiased;
    -moz-osx-font-smoothing: grayscale;
    text-align: center;
    color: #2c3e50;
    margin-top: 60px;
}
</style>
