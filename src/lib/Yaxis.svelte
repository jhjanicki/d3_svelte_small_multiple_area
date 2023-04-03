<script>
    export let yScale;

    const TICK_LENGTH = 8;
</script>

{#each yScale.ticks(5) as tick}
    <text
        x="0"
        dx="-14"
        y={yScale(tick)}
        text-anchor="end"
        dominant-baseline="middle"
        fill="black"
    >
        {tick}
    </text>
    <line
        x1={-TICK_LENGTH}
        x2={0}
        y1={yScale(tick)}
        y2={yScale(tick)}
        stroke="black"
    />
{/each}

<style>
    text {
        text-transform: uppercase;
        font-size:12px;
    }
</style>