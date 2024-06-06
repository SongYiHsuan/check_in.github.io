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
          fetch('[https://script.google.com/macros/s/AKfycbwLpdtmdodjhsMgtgt6ixCUucuEl_4MfytcD8KjkOA35xp0u5_3k8y49JeGIklHuHtm/exec](https://script.google.com/macros/s/AKfycbzHWHcs3dvTpewt2ZJJep-nL2jorvppcebYhh2dSlPsc6uZznH0Ehtr6WryPB_3xSuK/exec)', {
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
  </body>
</html>
