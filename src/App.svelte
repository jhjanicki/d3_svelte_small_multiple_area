<script>
  import { onMount } from "svelte";
  import Area from "./lib/Area.svelte";
  import Xaxis from "./lib/Xaxis.svelte";
  import Yaxis from "./lib/Yaxis.svelte";
  import data from "./data/data.json";
  import * as d3 from "d3";
  import textures from "textures";

  //group data
  const regions = d3.group(data, (d) => d.area);

  const scaleFactor = 4;

  let width = 1920 / scaleFactor;
  let height = 1080 / scaleFactor;

  const margin = {
    top: (40 + 84) / scaleFactor,
    left: (140 + 84) / scaleFactor,
    bottom: (120 + 84) / scaleFactor,
    right: 84 / scaleFactor,
  };

  const xScale = d3
    .scaleLinear()
    .domain(d3.extent(data, (d) => d.year))
    .range([0, width - margin.left - margin.right]);

  const yScale = d3
    .scaleLinear()
    .domain([0, d3.max(data, (d) => d.population_thousand)]) //use pop thousand otherwise hard to fit y axis labels
    .range([height - margin.bottom, 0]);

  let svgElement;
  const texture = textures.lines().size(4).strokeWidth(1).stroke("#e34a33");

  onMount(() => {
    d3.select(svgElement).call(texture);
  });
</script>

{#each [...regions] as [key, value]}
  <svg {width} {height} bind:this={svgElement}>
    <g transform="translate({margin.left},{margin.top})">
      <text x="10" y="0" fill="black" font-weight="700">{key}</text>
      <text x={width-160} y="0" fill="black" font-size="12">{key==="AFRICA"?"(unit: thousands)":""}</text>
      <Area
        data={value.filter((d) => d.year <= 2022)}
        {xScale}
        {yScale}
        color={"#e34a33"}
      />
      <Area
        data={value.filter((d) => d.year >= 2022)}
        {xScale}
        {yScale}
        color={texture.url()}
      />
      <Xaxis {xScale} height={height - margin.bottom} />
      <Yaxis {yScale} />
    </g>
  </svg>
{/each}
