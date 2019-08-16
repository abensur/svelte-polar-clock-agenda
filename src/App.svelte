<script>
import ClockBody from "./ClockBody.svelte";
import ClockAgenda from "./ClockAgenda.svelte";
import ClockHands from "./ClockHands.svelte";

let now = new Date();

const width = 250;

const config = {
	clockRadius: (width * 0.8) / 2,
	margin: (width * 0.2) / 4,
};

config.width = (config.clockRadius + config.margin * 2) * 2;
config.height = (config.clockRadius + config.margin * 2) * 2;

let data = new Object();

$: data.hours = now.getHours() + now.getMinutes() / 60;
$: data.minutes = now.getMinutes();
$: data.seconds = now.getSeconds();

function getDummyEvents() {
	let suffix = '2015-11-18 ';
	return [{
		index: 0,
		start: new Date(suffix + '03:00:00 UTC'),
		end: new Date(suffix + '09:00:00 UTC')
	},{
		index: 1,
		start: new Date(suffix + '09:00:00 UTC'),
		end: new Date(suffix + '14:00:00 UTC')
	},{
		index: 2,
		start: new Date(suffix + '11:00:00 UTC'),
		end: new Date(suffix + '14:00:00 UTC')
	},{
		index: 3,
		start: new Date(suffix + '12:00:00 UTC'),
		end: new Date(suffix + '18:00:00 UTC')
	},{
		index: 4,
		start: new Date(suffix + '16:40:00 UTC'),
		end: new Date(suffix + '21:00:00 UTC')
	},{
		index: 5,
		start: new Date(suffix + '16:40:00 UTC'),
		end: new Date(suffix + '21:00:00 UTC')
	}];
}

setInterval(() => {
	now = new Date();
}, 1000);

</script>

<style>
* {
  	box-sizing: border-box;
}
.background {
	position: absolute;
	top: 0;
	bottom: 0;
	right: 0;
	left: 0;
  	background: #454555;
	-webkit-font-smoothing: antialiased;
}
.container {
	display: flex;
	flex-flow: row wrap;
	width: 100%;
	height: 100%;
}
.polar-agenda {
  	font-size: 10px;
	margin: auto;
	width: 250px;
	height: 250px;
	position: relative;
}
</style>

<div class="background">
	<div class="container">
		<div class="polar-agenda">
			<ClockBody config="{config}"/>
			<ClockAgenda config="{config}" events="{getDummyEvents()}" />
			<ClockHands config="{config}" data={data} />
		</div>
	</div>
</div>