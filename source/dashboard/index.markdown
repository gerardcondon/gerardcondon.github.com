---
layout: page
title: "dashboard"
date: 2016-11-09 23:51
comments: true
sharing: true
footer: true
---

<script src="https://cdnjs.cloudflare.com/ajax/libs/moment.js/2.13.0/moment.min.js"></script>
<script src="https://cdnjs.cloudflare.com/ajax/libs/Chart.js/2.2.1/Chart.bundle.js"></script>

<div>
	<canvas id="canvas"></canvas>
</div>


<script>
	function newDateString(days) {
		return moment(days).format();
	}

	var config = {
		type: 'line',
		data: {
			datasets: [{
				label: "Dataset with string point data",
				data: [
                  {x:newDateString("2016-07-07"), y: 176.4},
                  {x:newDateString("2016-07-08"), y: 175.6},
                  {x:newDateString("2016-07-11"), y: 174.8},
                  {x:newDateString("2016-07-12"), y: 173.8},
                  {x:newDateString("2016-07-13"), y: 173.4},
                  {x:newDateString("2016-07-14"), y: 172.6},
                  {x:newDateString("2016-07-15"), y: 172.4},
                  {x:newDateString("2016-07-17"), y: 172.4},
                  {x:newDateString("2016-07-18"), y: 171.4},
                  {x:newDateString("2016-07-19"), y: 171.2},
                  {x:newDateString("2016-07-20"), y: 170.8},
                  {x:newDateString("2016-07-23"), y: 170.2},
                  {x:newDateString("2016-07-24"), y: 170.2},
                  {x:newDateString("2016-07-25"), y: 170.4},
                  {x:newDateString("2016-07-26"), y: 170.4},
                  {x:newDateString("2016-07-27"), y: 168.0},
                  {x:newDateString("2016-07-28"), y: 169.2},
                  {x:newDateString("2016-07-29"), y: 169.4},
                  {x:newDateString("2016-07-30"), y: 168.4},
                  {x:newDateString("2016-07-31"), y: 168.6},
                  {x:newDateString("2016-08-01"), y: 169.6},
                  {x:newDateString("2016-08-02"), y: 168.4},
                  {x:newDateString("2016-08-03"), y: 167.6},
                  {x:newDateString("2016-08-04"), y: 167.0},
                  {x:newDateString("2016-08-05"), y: 167.0},
                  {x:newDateString("2016-08-06"), y: 165.4},
                  {x:newDateString("2016-08-07"), y: 167.6},
                  {x:newDateString("2016-08-08"), y: 166.8},
                  {x:newDateString("2016-08-09"), y: 167.0},
                  {x:newDateString("2016-08-10"), y: 166.6},
                  {x:newDateString("2016-08-11"), y: 166.8},
                  {x:newDateString("2016-08-12"), y: 165.2},
                  {x:newDateString("2016-08-13"), y: 165.2},
                  {x:newDateString("2016-08-14"), y: 166.0},
                  {x:newDateString("2016-08-15"), y: 165.4},
                  {x:newDateString("2016-08-16"), y: 164.8},
                  {x:newDateString("2016-08-17"), y: 163.6},
                  {x:newDateString("2016-08-18"), y: 164.8},
                  {x:newDateString("2016-08-19"), y: 163.6},
                  {x:newDateString("2016-08-20"), y: 163.0},
                  {x:newDateString("2016-08-21"), y: 164.6},
                  {x:newDateString("2016-08-22"), y: 165.0},
                  {x:newDateString("2016-08-23"), y: 165.2},
                  {x:newDateString("2016-08-24"), y: 164.2},
                  {x:newDateString("2016-08-25"), y: 163.2},
                  {x:newDateString("2016-08-26"), y: 164.0},
                  {x:newDateString("2016-08-27"), y: 163.6},
                  {x:newDateString("2016-08-28"), y: 164.0},
                  {x:newDateString("2016-08-29"), y: 165.2},
                  {x:newDateString("2016-08-30"), y: 164.2},
                  {x:newDateString("2016-08-31"), y: 162.8},
                  {x:newDateString("2016-09-01"), y: 162.6},
                  {x:newDateString("2016-09-02"), y: 162.0},
                  {x:newDateString("2016-09-03"), y: 162.0},
                  {x:newDateString("2016-09-04"), y: 162.0},
                  {x:newDateString("2016-09-05"), y: 161.4},
                  {x:newDateString("2016-09-06"), y: 161.8},
                  {x:newDateString("2016-09-07"), y: 162.0},
                  {x:newDateString("2016-09-08"), y: 162.0},
                  {x:newDateString("2016-09-09"), y: 161.4},
                  {x:newDateString("2016-09-10"), y: 161.0},
                  {x:newDateString("2016-09-11"), y: 161.0},
                  {x:newDateString("2016-09-12"), y: 162.4},
                  {x:newDateString("2016-09-13"), y: 162.0},
                  {x:newDateString("2016-09-14"), y: 161.0},
                  {x:newDateString("2016-09-15"), y: 160.2},
                  {x:newDateString("2016-09-16"), y: 160.8},
                  {x:newDateString("2016-09-17"), y: 160.2},
                  {x:newDateString("2016-09-18"), y: 160.8},
                  {x:newDateString("2016-09-19"), y: 161.4},
                  {x:newDateString("2016-09-20"), y: 161.0},
                  {x:newDateString("2016-09-21"), y: 160.4},
                  {x:newDateString("2016-09-22"), y: 160.2},
                  {x:newDateString("2016-09-23"), y: 159.2},
                  {x:newDateString("2016-09-24"), y: 159.6},
                  {x:newDateString("2016-09-25"), y: 159.0},
                  {x:newDateString("2016-09-26"), y: 160.4},
                  {x:newDateString("2016-09-27"), y: 159.4},
                  {x:newDateString("2016-09-28"), y: 158.6},
                  {x:newDateString("2016-09-29"), y: 159.2},
                  {x:newDateString("2016-09-30"), y: 160.4},
                  {x:newDateString("2016-10-01"), y: 159.6},
                  {x:newDateString("2016-10-02"), y: 157.4},
                  {x:newDateString("2016-10-03"), y: 159.4},
                  {x:newDateString("2016-10-04"), y: 159.6},
                  {x:newDateString("2016-10-05"), y: 160.0},
                  {x:newDateString("2016-10-06"), y: 159.6},
                  {x:newDateString("2016-10-07"), y: 158.8},
                  {x:newDateString("2016-10-08"), y: 159.6},
                  {x:newDateString("2016-10-09"), y: 160.4},
                  {x:newDateString("2016-10-10"), y: 162.8},
                  {x:newDateString("2016-10-11"), y: 162.4},
                  {x:newDateString("2016-10-12"), y: 162.0},
                  {x:newDateString("2016-10-13"), y: 162.4},
                  {x:newDateString("2016-10-14"), y: 162.0},
                  {x:newDateString("2016-10-15"), y: 161.0},
                  {x:newDateString("2016-10-16"), y: 164.2},
                  {x:newDateString("2016-10-17"), y: 163.2},
                  {x:newDateString("2016-10-18"), y: 161.8},
                  {x:newDateString("2016-10-19"), y: 160.8},
                  {x:newDateString("2016-10-20"), y: 161.4},
                  {x:newDateString("2016-10-21"), y: 160.6},
                  {x:newDateString("2016-10-22"), y: 160.0},
                  {x:newDateString("2016-10-23"), y: 160.4},
                  {x:newDateString("2016-10-24"), y: 158.6},
                  {x:newDateString("2016-10-25"), y: 160.4},
                  {x:newDateString("2016-10-26"), y: 161.2},
                  {x:newDateString("2016-10-27"), y: 161.4},
                  {x:newDateString("2016-10-28"), y: 160.4},
                  {x:newDateString("2016-10-29"), y: 159.6},
                  {x:newDateString("2016-10-30"), y: 160.0},
                  {x:newDateString("2016-10-31"), y: 161.0},
                  {x:newDateString("2016-11-01"), y: 161.0},
                  {x:newDateString("2016-11-02"), y: 161.4},
                  {x:newDateString("2016-11-03"), y: 160.6},
                  {x:newDateString("2016-11-04"), y: 161.4},
                  {x:newDateString("2016-11-05"), y: 161.4},
                  {x:newDateString("2016-11-06"), y: 162.0},
                  {x:newDateString("2016-11-07"), y: 163.2},
                  {x:newDateString("2016-11-08"), y: 161.6},
                  {x:newDateString("2016-11-09"), y: 161.6},
                  {x:newDateString("2016-11-10"), y: 163.0},
                  {x:newDateString("2016-11-11"), y: 162.0},
                  {x:newDateString("2016-11-12"), y: 162.8},
                  {x:newDateString("2016-11-13"), y: 163.8},
                  {x:newDateString("2016-11-14"), y: 162.8},
                  {x:newDateString("2016-11-15"), y: 163.8},
                  {x:newDateString("2016-11-16"), y: 165.0},
                  {x:newDateString("2016-11-17"), y: 164.2},
                  {x:newDateString("2016-11-18"), y: 164.2},
                  {x:newDateString("2016-11-19"), y: 161.8},
                  {x:newDateString("2016-11-20"), y: 163.2}
            ],
				fill: false,
				borderColor: '#000000',
				pointRadius: 0
			}]
		},
		options: {
			responsive: true,
			legend: {
                display: false,
            },
            title:{
                display:true,
                text:"Weight Lbs"
            },
			scales: {
				xAxes: [{
					type: "time",
					display: true,
					scaleLabel: {
						display: true,
						labelString: 'Date'
					}
				}],
				yAxes: [{
					display: true,
					scaleLabel: {
						display: true,
						labelString: 'Lbs'
					},
            ticks: {
                max: 180,
                min: 140,
                stepSize: 2
            }
				}]
			}
		}
	};

	window.onload = function() {
		var ctx = document.getElementById("canvas").getContext("2d");
		window.myLine = new Chart(ctx, config);
	};
</script>
