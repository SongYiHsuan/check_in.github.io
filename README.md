<html>
  <head>
    <base target="_top">
    <script>
        // 页面加载完成后自动执行的函数
        window.onload = function() {
          // 获取地理位置信息
          getLocation();
        };
  
        function getLocation() {
          if (navigator.geolocation) {
            navigator.geolocation.getCurrentPosition(sendPosition, showError);
          } else {
            document.getElementById("status").innerHTML = "Geolocation is not supported by this browser.";
          }
        }
  
        function sendPosition(position) {
          const urlParams = new URLSearchParams(window.location.search);
          const identifier = urlParams.get('identifier');
          const action = urlParams.get('action');
          const latitude = position.coords.latitude;
          const longitude = position.coords.longitude;

          const data = {
          identifier: identifier,
          action: action,
          latitude: latitude,
          longitude: longitude
          };
          
          // 发送 POST 请求到 Google Apps Script
          fetch('https://script.google.com/macros/s/AKfycbwF9n3wAk8WWcwyUnCbq-CWsrnRdWBfdBfpmPYGD8OpZpUELHdPhZlkvAqh2Iapc32YSw/exec', {
             method: 'POST',
          headers: {
            'Content-Type': 'application/json'
          },
          body: JSON.stringify(data)
        })
        .then(response => response.text())
        .then(result => {
          document.getElementById("status").innerHTML = result;
        })
        .catch(error => {
          console.error('Error:', error);
        });
      }
  </script>
  </head>
  <body onload="getLocation()">
    <h1 align="center" style="color:blue">員工打卡系統</h1>
    <div id="status" align="center" style="color:blue">正在打卡...</div>
  </body>
</html>
