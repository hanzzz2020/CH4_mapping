# CH4_mapping
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <link rel="stylesheet" href="style.css">
</head>
<body>
    <h2>Welcome to My Webpage</h2>
    <p>This is a simple webpage designed on GitHub.</p>
</body>
</html>
<html>
<head>
    <title>Leaflet with Gaode Map and GeoJSON</title>
    <meta charset="utf-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <!-- 引入Leaflet的CSS -->
    <link rel="stylesheet" href="https://unpkg.com/leaflet/dist/leaflet.css" />
    <!-- 确保页面不会因为默认的边距和填充而出现滚动条 -->
    <style>
        #mapid { height: 600px; }
    </style>
</head>
<body>
    <div id="mapid"></div>
    <!-- 引入Leaflet的JavaScript -->
    <script src="https://unpkg.com/leaflet/dist/leaflet.js"></script>
    <script>
        // 创建地图实例，并设置视图到世界地图的中心点，缩放级别为3
        var map = L.map('mapid').setView([0, 0], 3);

        // 使用高德地图作为底图
        var gaodeLayer = L.tileLayer('https://webst0{s}.is.autonavi.com/appmaptile?style=6&x={x}&y={y}&z={z}&lang=zh_cn&tk=你的高德地图API密钥', {
            subdomains: ['1', '2', '3', '4'],
            attribution: '高德地图'
        }).addTo(map);

        // 加载GeoJSON数据
        // 请确保GeoJSON文件路径正确，并且服务器可以访问该文件
        // 如果是在本地测试，建议将GeoJSON文件放在与HTML文件相同的目录下
        fetch('https://hanzzz2020.github.io/CH4_mapping/InlandWater_data.geojson') // 修改为你的文件URL
            .then(response => response.json())
            .then(data => {
                // 创建GeoJSON图层并添加到地图
                L.geoJSON(data, {
                    onEachFeature: function (feature, layer) {
                        // 为每个特征添加弹出窗口
                        if (feature.properties) {
                            var popupContent = `<strong>Properties:</strong><br>`;
                            for (var key in feature.properties) {
                                popupContent += `${key}: ${feature.properties[key]}<br>`;
                            }
                            layer.bindPopup(popupContent);
                        }
                    }
                }).addTo(map);
            })
            .catch(error => {
                console.error('Error loading GeoJSON:', error);
            });
    </script>
</body>
</html>
