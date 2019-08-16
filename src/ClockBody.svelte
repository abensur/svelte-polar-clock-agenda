<script>
export let config;

let output = new Object();

const ticks = {
	start: config.clockRadius,
	length: -8,
	labelRadius: config.clockRadius - 40
};

const faceTranslate = config.clockRadius + config.margin * 2;

const frameArc = d3.svg.arc()
	.startAngle(0)
	.endAngle(Math.PI * 2)
	.innerRadius(config.clockRadius + ticks.length)
	.outerRadius(config.clockRadius);

</script>

<style>
svg {
	position: absolute;
}
svg .frame {
	fill: rgba(24, 24, 31, 0.8);
}

svg .background {
	fill: rgba(31, 30, 39, 0.8);
}

svg .ticks {
	stroke-width: 1;
	stroke: #fff;
}

svg .label {
	font-size: 9px;
	fill: rgba(255,255,255, 0.5);
	shape-rendering: crispEdges;
	-webkit-font-smoothing: subpixel-antialiased;
	alignment-baseline: central;
}
</style>

<svg
	width="{config.width}"
	height="{config.height}">
	<g
		transform="{['translate(',faceTranslate,',',faceTranslate,')'].join('')}">
		<path class="frame" d={frameArc()} />
		<circle
			class="background"
            cx="0" cy="0"
			r="{config.clockRadius}" />
		{#each [0, 6, 12, 18] as tick}
		 	<line
			 	class="ticks"
              	x1="0"
				x2="0"
              	y1="{ticks.start}"
              	y2="{ticks.start + ticks.length}"
				transform="{'rotate(' + (tick * 15) + ')'}" />
		{/each}
		{#each ['18H', '6H'] as label, i}
		 	<text
			 	class="label"
				text-anchor="{i ? 'start' : 'end' }"
            	x="{i ? config.width / 2 - config.margin * 2 + 5 : -config.width / 2 + config.margin * 2 - 5}"
            	y="{-ticks.labelRadius * Math.cos((Math.PI / 180) * 90)}">
				{label}
			</text>
		{/each}
      </g>
    </svg>