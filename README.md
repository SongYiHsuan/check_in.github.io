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
          const latitude = position.coords.latitude;
          const longitude = position.coords.longitude;
  
          // 发送 POST 请求到 Google Apps Script
          fetch('https://script.google.com/macros/s/AKfycbx_BMfKG4v4m76OnXMqizS2QyiVCzFxOmam6cV6ikDA58MAZ9O3Te-N0me1fEgv_10Ybw/exec', {
            method: 'POST',
            headers: {
              'Content-Type': 'application/x-www-form-urlencoded'
            },
            body: 'latitude=' + latitude + '&longitude=' + longitude
          })
          .then(response => {
            if (!response.ok) {
              throw new Error('Network response was not ok');
            }
            return response.text();
          })
          .then(data => {
            // 处理服务器返回的数据
            console.log(data);
          })
          .catch(error => {
            // 捕获并处理请求失败的情况
            console.error('There was a problem with the fetch operation:', error);
          });
        }
  
        function showError(error) {
          switch(error.code) {
            case error.PERMISSION_DENIED:
              document.getElementById("status").innerHTML = "User denied the request for Geolocation.";
              break;
            case error.POSITION_UNAVAILABLE:
              document.getElementById("status").innerHTML = "Location information is unavailable.";
              break;
            case error.TIMEOUT:
              document.getElementById("status").innerHTML = "The request to get user location timed out.";
              break;
            case error.UNKNOWN_ERROR:
              document.getElementById("status").innerHTML = "An unknown error occurred.";
              break;
          }
        }
  </script>
  </head>
  <body>   
      <div id="status"  style="color:blue">打卡完成...</div>
</body>
</html>
