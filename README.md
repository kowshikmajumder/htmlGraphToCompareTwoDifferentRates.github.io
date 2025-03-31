<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Rate Compare</title>
  <script src="https://cdn.jsdelivr.net/npm/chart.js"></script>
  <style>
    body {
      font-family: Arial, sans-serif;
      background-color: #f4f4f9;
      display: flex;
      justify-content: center;
      align-items: center;
      height: 100vh;
      margin: 0;
    }
    .chart-container {
      width: 80%;
      max-width: 800px;
      background: white;
      padding: 20px;
      border-radius: 10px;
      box-shadow: 0 4px 6px rgba(0, 0, 0, 0.1);
    }
    .controls {
      margin-top: 20px;
      text-align: center;
    }
	
    input {
      padding: 10px;
      margin: 5px;
      border: 1px solid #ccc;
      border-radius: 5px;
      font-size: 16px;
	  }
    button {
      background-color: #4CAF50;
      color: white;
      border: none;
      padding: 10px 20px;
      margin: 5px;
      border-radius: 5px;
      cursor: pointer;
      font-size: 16px;
    }
    button:hover {
      background-color: #45a049;
    }
  </style>
</head>
<body>
  <div class="chart-container">
    <canvas id="myChart"></canvas>
    <div class="controls">
      <button onclick="updateDataset(0)">Toggle Rate 1</button>
      <button onclick="updateDataset(1)">Toggle Rate 2</button>
	 </div>  
	  <div class="controls">
	  <label for="rate1">Rate1:</label>
      <input type="number" id="rate1" value="8.5" step="0.1">
	  <label for="rate1">Month1:</label>
      <input type="number" id="month1" value="3" step="0.1">
	  <br>
	  
      <label for="rate2">Rate2:</label>
      <input type="number" id="rate2" value="9" step="0.1">
      <label for="rate2">Month2:</label>
      <input type="number" id="month2" value="12" step="0.1">
      
	  
	  <br>
	  <label for="month">Month:</label>
      <input type="number" id="month" value="50" step="0.1">
      
	  <button onclick="updateGraph()">Update Graph</button>

	  
	  
	  
    </div>
  </div>

  <script>
      // Initialize Chart.js
    const ctx = document.getElementById('myChart').getContext('2d');
    let myChart;

   function generateData(rate, month,per) {
      let p=100;
	let labelkm=[]
	let t=month;
	
	for(let i=1;i<=t;++i){
	
	labelkm.push(i);
	
	}
	let data =labelkm.slice();
	for(let i=1;i<=t;++i){
	if(i<3){
	data[i-1]=p;
	
	
	}
	else{
	if(i%per==0){
	data[i-1]=data[i-2]+data[i-2]*rate*per/1200;
	}
	else{
	data[i-1]=data[i-2];
	}
	}
	
	}
      return data;
    }
	
	
	    function updateGraph() {
      const r1 = parseFloat(document.getElementById('rate1').value);
      const rateMonth1 = parseFloat(document.getElementById('month1').value);

	  const r2 = parseFloat(document.getElementById('rate2').value);
      const rateMonth2 = parseFloat(document.getElementById('month2').value);

	  const m = parseFloat(document.getElementById('month').value);

      const data1 = generateData(r1, m,rateMonth1);
	  const data2 = generateData(r2, m,rateMonth2);
	  console.log(data1);
      // Update the chart data
	let t=m;
	labelkm=[];
	for(let i=1;i<=t;++i){
	
	labelkm.push(i);
	
	}
      myChart.data.labels = labelkm;
      myChart.data.datasets[0].data = data1;
      myChart.data.datasets[1].data = data2;
      myChart.update();
    }

	
	
    // Data for the chart
	let p=100;
	let labelkm=[]
	let t=100;
	
	for(let i=1;i<=t+1;++i){
	
	labelkm.push(i);
	
	}
	
	let rate1=generateData(8.5,100,3);
	
	let rate2=generateData(9.5,100,12);
	
    const data = {
      labels: labelkm,
      datasets: [
        {
          label: 'Rate 1',
          data: rate1,
          borderColor: '#36A2EB',
          backgroundColor: 'rgba(54, 162, 235, 0.2)',
          borderWidth: 2,
          fill: false,
        },
        {
          label: 'Rate 2',
          data: rate2,
          borderColor: '#FF6384',
          backgroundColor: 'rgba(255, 99, 132, 0.2)',
          borderWidth: 2,
          fill: false,
        }
      ]
    };

    // Chart configuration
    const config = {
      type: 'line',
      data: data,
      options: {
        responsive: true,
        plugins: {
          legend: {
            position: 'top',
          },
          title: {
            display: true,
            text: 'Two Separate Rate Graph'
          }
        },
        scales: {
          y: {
            beginAtZero: true
          }
        }
      }
    };

    // Initialize the chart
    myChart = new Chart(document.getElementById('myChart'), config);

    // Function to toggle dataset visibility
    function updateDataset(index) {
      const meta = myChart.getDatasetMeta(index);
      meta.hidden = !meta.hidden;
      myChart.update();
    }
  </script>
</body>
</html>
