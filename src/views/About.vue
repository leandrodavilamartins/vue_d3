<template>
  <div class="about">
    <h1 id="title"></h1>
    <svg id="graph"></svg>
  </div>
</template>

<script>
import * as d3 from "d3";

export default {

  data(){
    return{
        gdp: [
        {country: "USA", value: 20.5 },
        {country: "China", value: 13.4 },
        {country: "Japan", value: 4.9 },
        {country: "Germany", value: 4.0 },
        {country: "France", value: 2.8 },
        {country: "Brazil", value: 2.1},
        {country: "Mexico", value: 1.2},
        {country: "India", value: 1.0}
      ]
    }
  },
  mounted(){
    this.generateGraph()
  },
  methods:{
    generateGraph(){ // function that generates a graph 
      let svg = d3.select('#graph')

      let data = this.gdp // data input 
      const width = 900;
      const height = 600;

      const margin = {top:20, bottom: 20, left: 30, right:30}

      svg
      .attr('width', width )
      .attr('height', height)

      let x_scale = d3.scaleBand().range([margin.left,width /2]).padding(0.1)
      let y_scale = d3.scaleLinear().range([height - margin.bottom, margin.top])

      x_scale.domain(data.map( d => d.country))
      y_scale.domain([0, d3.max(data, d => d.value)])

      let x_axis = d3.axisBottom(x_scale);
      let y_axis = d3.axisLeft(y_scale);

      svg
      .selectAll('rect')
      .data(data)
      .join('rect')
      .attr('fill', 'green')
      .attr('x', (d) => x_scale(d.country))
      .attr('y', (d) => y_scale(d.value))
      .attr('width', x_scale.bandwidth())
      .attr('height', (d) =>  height - margin.bottom - y_scale(d.value))

      svg
      .append('g')
      .call(x_axis)
      .attr('transform', `translate(0,${height - margin.bottom})`)

      svg
      .append('g')
      .call(y_axis)
      .attr('transform', `translate(${margin.left},0)`)
    }
  }
}

</script>
