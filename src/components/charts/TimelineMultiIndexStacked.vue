<!-- Developed by Taipei Urban Intelligence Center 2023 -->

<script setup>
import { ref } from "vue";

const props = defineProps(["chart_config", "activeChart", "series"]);

const chartOptions = ref({
	chart: {
		stacked: true,
		toolbar: {
			show: false,
			tools: {
				zoom: false,
			},
		},
	},
	colors: props.chart_config.color,
	dataLabels: {
		enabled: false,
	},
	grid: {
		show: false,
	},
	legend: {
		show: props.series.length > 1 ? true : false,
	},
	markers: {
		hover: {
			size: 5,
		},
		size: 3,
		strokeWidth: 0,
	},
	stroke: {
		colors: props.chart_config.color,
		curve: "smooth",
		show: true,
		width: 2,
	},
	tooltip: {
		custom: function ({ series, seriesIndex, dataPointIndex, w }) {
			let mainTitle = '<h6>' + w.globals.seriesNames[seriesIndex] + 
						'</h6>' +
						'<span>'+ series[seriesIndex][dataPointIndex] + ` ${props.chart_config.pointsUnits[seriesIndex]}` + ' </span>';
			let otherTitle = '';
			for (let index in w.globals.seriesNames){
				if (index == seriesIndex)
					continue;
				otherTitle += "<br>" 
				otherTitle += '<h6>' +  w.globals.seriesNames[index]+ 
					'</h6>' +
					'<h6>'+ series[index][dataPointIndex] + ` ${props.chart_config.pointsUnits[index]}` +' </h6>';
			}
			return '<div class="chart-tooltip">' +
					mainTitle + 
					otherTitle +
					'</div>';
		},
	},
	xaxis: {
		axisBorder: {
			color: "#555",
			height: "0.8",
		},
		axisTicks: {
			show: false,
		},
		crosshairs: {
			show: false,
		},
		tooltip: {
			enabled: false,
		},
		type: "datetime",
	},
	yaxis: [{}, {opposite: true}]
});
</script>

<template>
	<div v-if="activeChart === 'TimelineMultiIndexStacked'">
		<apexchart
			width="100%"
			height="260px"
			type="area"
			:options="chartOptions"
			:series="series"
		></apexchart>
	</div>
</template>
