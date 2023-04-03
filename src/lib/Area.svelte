<script>
    import * as d3 from "d3";
    export let data;
    export let xScale;
    export let yScale;
    export let color;

    // Create an area generator
    const areaGenerator = d3.area()
        .x(d=>xScale(d.year))
        .y0(yScale(0))
        .y1(d=>yScale(d.population_thousand))
</script>

<path d={areaGenerator(data)} fill={color} />
