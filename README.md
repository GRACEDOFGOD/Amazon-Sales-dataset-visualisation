# Amazon-Sales-dataset-visualisation
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Interactive Sales Performance Dashboard</title>
    <script src="https://cdn.plot.ly/plotly-2.24.1.min.js"></script>
    <style>
        body {
            margin: 0;
            font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
            background: url('https://images.unsplash.com/photo-1517148815978-75f6acaaf32c') no-repeat center center fixed;
            background-size: cover;
            color: #fff;
            text-align: center;
            display: flex;
            flex-direction: column;
            align-items: center;
        }

        h1 {
            margin: 20px;
            color: #f8f8f8;
            text-shadow: 2px 2px 4px rgba(0, 0, 0, 0.7);
            font-size: 2.5em;
        }

        input[type="file"] {
            margin: 20px;
            padding: 10px 20px;
            border: none;
            border-radius: 10px;
            background: rgba(0, 0, 0, 0.5);
            color: #f8f8f8;
            cursor: pointer;
            font-size: 1em;
            text-shadow: 1px 1px 2px rgba(0, 0, 0, 0.7);
        }

        input[type="file"]:hover {
            background: rgba(0, 0, 0, 0.7);
        }

        #dashboard {
            width: 90%;
            margin: 20px auto;
            display: flex;
            flex-wrap: wrap;
            justify-content: space-between;
            background: rgba(0, 0, 0, 0.6);
            padding: 20px;
            border-radius: 15px;
            box-shadow: 0 4px 10px rgba(0, 0, 0, 0.8);
        }

        #dashboard > div {
            width: 40%;
            margin: 20px;
            padding: 20px;
            border-radius: 15px;
            background: rgba(255, 255, 255, 0.1);
            box-shadow: 0 4px 10px rgba(0, 0, 0, 0.6);
            color: #fff;
        }

        @media (max-width: 768px) {
            #dashboard > div {
                width: 90%;
            }
        }

        footer {
            margin-top: 20px;
            padding: 10px;
            background: rgba(0, 0, 0, 0.5);
            color: #f8f8f8;
            width: 100%;
            text-align: center;
            position: fixed;
            bottom: 0;
        }
    </style>
</head>
<body>
    <h1>Interactive Sales Performance Dashboard</h1>
    <h2>Powered by Graced Analysis Solution</h2>
    <input type="file" id="fileInput" accept="application/json">
    <div id="dashboard">
        <div id="heatmap"></div>
        <div id="growthRate"></div>
        <div id="topProducts"></div>
        <div id="demographics"></div>
        <div id="salesTrends"></div>
    </div>
    <footer>
        &copy; 2024 Graced Analysis Solution - Empowering Data-Driven Decisions
    </footer>
    <script>
        document.getElementById('fileInput').addEventListener('change', function(event) {
            const file = event.target.files[0];
            if (file) {
                const reader = new FileReader();
                reader.onload = function(e) {
                    try {
                        const salesData = JSON.parse(e.target.result);
                        generateDashboard(salesData);
                    } catch (error) {
                        alert("Invalid JSON format.");
                        console.error(error);
                    }
                };
                reader.readAsText(file);
            }
        });

        function generateDashboard(data) {
            const continents = data.continents.filter(continent => continent.name !== null);

            // Heatmap: Sales by Continent
            const heatmapData = {
                type: 'choropleth',
                locationmode: 'continent names',
                locations: continents.map(c => c.name),
                z: continents.map(() => Math.random() * 1000), // Placeholder for sales data
                colorscale: 'Blues',
                colorbar: { title: 'Sales Volume' }
            };

            Plotly.newPlot('heatmap', [heatmapData], {
                title: 'Sales by Continent',
                geo: { showframe: false, showcoastlines: false, projection: { type: 'natural earth' } }
            });

            // Revenue Growth Rate by Continent
            const growthRateData = {
                x: continents.map(c => c.name),
                y: continents.map(() => Math.random() * 20 - 10), // Placeholder for growth rates
                type: 'bar',
                marker: { color: 'orange' }
            };

            Plotly.newPlot('growthRate', [growthRateData], {
                title: 'Revenue Growth Rate by Continent',
                xaxis: { title: 'Continent' },
                yaxis: { title: 'Growth Rate (%)' }
            });

            // Top-Selling Products by Continent
            const topProductsData = {
                labels: ['Product A', 'Product B', 'Product C'],
                values: [Math.random() * 500, Math.random() * 500, Math.random() * 500], // Placeholder
                type: 'pie'
            };

            Plotly.newPlot('topProducts', [topProductsData], {
                title: 'Top-Selling Products'
            });

            // Customer Demographics by Continent
            const demographicsData = {
                x: continents.map(c => c.name),
                y: continents.map(() => Math.random() * 1000), // Placeholder for demographics data
                type: 'bar',
                marker: { color: 'purple' }
            };

            Plotly.newPlot('demographics', [demographicsData], {
                title: 'Customer Demographics by Continent',
                xaxis: { title: 'Continent' },
                yaxis: { title: 'Customer Count' }
            });

            // Sales Trends Over Time
            const timeSeriesData = {
                x: ['2023-01', '2023-02', '2023-03', '2023-04', '2023-05'],
                y: Array(5).fill().map(() => Math.random() * 1000), // Placeholder for sales trends
                type: 'scatter',
                mode: 'lines+markers',
                marker: { color: 'green' }
            };

            Plotly.newPlot('salesTrends', [timeSeriesData], {
                title: 'Sales Trends Over Time',
                xaxis: { title: 'Time' },
                yaxis: { title: 'Sales Volume' }
            });
        }
    </script>
</body>
</html>
