<script>
export let config;
export let events;

let faceTranslate = config.clockRadius + config.margin * 2;

const scales = {
	hours: d3.scale.linear().range([0, 330]).domain([0, 23]),
	minutes: d3.scale.linear().range([0,354]).domain([0,59]),
	seconds: d3.scale.linear().range([0,354]).domain([0,59]),
	color: d3.scale.linear().range(["hsl(-180,50%,50%)", "hsl(180,50%,50%)"])
		.domain([0, events.length])
		.interpolate(interpolateHsl)
}

function getArc(data) {
	let t0 = data.start.getHours() + data.start.getMinutes() / 60;
	let t1 = data.end.getHours() + data.end.getMinutes() / 60;
	let scale = scales.hours;
	let ratio = (config.clockRadius - 8) / events.length;
	let padding = 0.3;

	let arc = d3.svg.arc()
		.startAngle(scale(t0) * Math.PI / 180)
		.endAngle(scale(t1) * Math.PI / 180)
		.innerRadius(d => (d.index + padding) * ratio)
		.outerRadius(d => (d.index - padding) * ratio + ratio);

	return arc(data);
}

function interpolateHsl(a, b) {
	let i = d3.interpolateString(a, b);
	return t => d3.hsl(i(t));
}
</script>

<style>
	svg {
		position: absolute;
	}
</style>

<svg
	width={config.width}
	height={config.height} >
	<g transform="{['translate(', faceTranslate, ',', faceTranslate, ')'].join('')}" >
	{#each events as event, i}
		<g class="arc">
			<path
				class="path"
				d="{getArc(event)}"
				fill="{scales.color(i)}" />
		</g>
	{/each}
	</g>
</svg>