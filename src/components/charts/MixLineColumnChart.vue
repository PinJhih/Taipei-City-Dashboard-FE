<!-- Developed by Taipei Urban Intelligence Center 2023 -->

<script setup>
import { ref, computed } from 'vue';

const props = defineProps(['chart_config', 'activeChart', 'series']);

const chartSeries = computed(() => {
	let chartData = props.series;
	chartData[0].type = "bar";

	// 在之後的所有元素添加 type: "line"
	for (let i = 1; i < chartData.length; i++) {
		chartData[i].type = "line";
	}
	return chartData;
})

const chartOptions = ref({
	chart: {
		toolbar: {
			show: false,
			tools: {
				zoom: false,
			},
		},
	},
	colors: props.chart_config.color,
	grid: {
		show: true,  
		row: {
			opacity: 0
		},  
	},
	legend: {
		show: props.chart_config.categories ? true : false,
	},
	markers: {
		hover: {
			size: 5,
		},
		size: 3,
		strokeWidth: 0,
	},
	stroke: {
		width: [0, 2],
	},
	tooltip: {
		// The class "chart-tooltip" could be edited in /assets/styles/chartStyles.css
		custom: function ({ series, seriesIndex, dataPointIndex, w }) {
			let mainTitle = '<h6>' + w.globals.seriesNames[seriesIndex] + 
						` - ${w.globals.labels[dataPointIndex]}` +
						` ${props.chart_config.unit}` + '</h6>' +
						'<span>'+ series[seriesIndex][dataPointIndex] + ` ${props.chart_config.pointsUnits[seriesIndex]}` + ' </span>';
			let otherTitle = '';
			for (let index in w.globals.seriesNames){
				if (index == seriesIndex)
					continue;
				otherTitle += "<br>" 
				otherTitle += '<h6>' +  w.globals.seriesNames[index]+ 
					` - ${w.globals.labels[dataPointIndex]}` +
					` ${props.chart_config.unit}` + '</h6>' +
					'<h6>'+ series[index][dataPointIndex] + ` ${props.chart_config.pointsUnits[index]}` +' </h6>';
			}
			return '<div class="chart-tooltip">' +
					mainTitle + 
					otherTitle +
					'</div>';
		},
		enabled: true,
		shared: true,
		followCursor: true,
	},
	// labels: props.chart_config.labels,
	xaxis: {
		crosshairs: {
			show: true,
		},
		tooltip: {
			enabled: false,
		},
		categories: props.chart_config.categories.map(category => category + props.chart_config.unit)
	},
	yaxis: [{}, {opposite: true}],
});

</script>

<template>
	<div v-if="activeChart === 'MixLineColumnChart'">
		<apexchart width="100%" height="270px" type="line" :options="chartOptions" :series=chartSeries></apexchart>
	</div>
</template>