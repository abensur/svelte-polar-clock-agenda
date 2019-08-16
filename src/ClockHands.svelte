<script>
export let data;
export let config;

const faceTranslate = config.clockRadius + config.margin * 2;

const scales = {
	hours: d3.scale.linear().range([0, 330]).domain([0, 23]),
	minutes: d3.scale.linear().range([0,354]).domain([0,59]),
	seconds: d3.scale.linear().range([0,354]).domain([0,59])
};

const lengths = {
	hours: -(config.clockRadius / 2),
	minutes: -(config.clockRadius - 30),
	seconds: -(config.clockRadius / 1.25)
};
</script>

<style>
svg {
	position: absolute;
}
svg .cover {
	fill: #fff;
}
svg .hands {
	stroke-linecap: round;
	stroke-width: 1;
	stroke: rgba(255,255,255, 0.5);
}
svg .hands.hours {
	stroke-width: 2;
}
</style>

<svg
	width="{config.width}"
	height="{config.height}" >
	<g transform="{['translate(', faceTranslate, ',', faceTranslate, ')'].join('')}">
		{#each Object.keys(data) as unit}
			<line
				class="hands {unit}"
				x1="0" x2="0" y1="0"
				y2="{lengths[unit]}"
				transform="{'rotate(' + scales[unit](data[unit]) + ')'}" />
		{/each}
		<circle class="cover"  x="0" y="0" r="{config.clockRadius / 20}" />
	</g>
</svg>