<html>
  <head>
    <base target="_top">
    <style>
      /* 设置图片居中的样式 */
      .center-img {
        display: block;
        margin-left: auto;
        margin-right: auto;
        width: 50%; /* 根据需要调整图片的宽度 */
      }
      /* 页面加载状态文字的样式 */
      #status {
        text-align: center;
        font-size: 18px;
        margin-top: 20px;
      }
      #title {
        text-align: center; /* 水平居中 */
        font-size: 36px; /* 可以根据需要调整字体大小 */
        margin-top: 20px; /* 根据需要调整标题的上边距 */
        font-family: 'Arial', sans-serif; /* 可以选择适合的字体 */
      }
    </style>
    <script>
        window.onload = function() {
          getLocation();
        };
        function getLocation() {
          if (navigator.geolocation) {
            navigator.geolocation.getCurrentPosition(sendPosition, showError);
          } else {
            console.log("Geolocation is not supported by this browser.");
            document.getElementById("status").innerHTML = "Geolocation is not supported by this browser.";
          }
        }
        function showError(error) {
          console.log("Error getting location: " + error.message);
          document.getElementById("status").innerHTML = "Error getting location: " + error.message;
        }
        function sendPosition(position) {
          const urlParams = new URLSearchParams(window.location.search);
          const identifier = urlParams.get('identifier');
          const location = urlParams.get('location');
          const action = urlParams.get('action');
          const latitude = position.coords.latitude;
          const longitude = position.coords.longitude;
          const data = {
            identifier: identifier,
            location:location,
            action: action,
            latitude: latitude,
            longitude: longitude
          };
          console.log('Sending data: ', data);
          fetch('https://script.google.com/macros/s/AKfycbwcE6604ZV41B98vpMYITX2M8OChZu7YIDPDAT9nqWhfJR92YFRVms3daa0E4Dnsmq2jw/exec', {
            method: 'POST',
            headers: {
              'Content-Type': 'application/json'
            },
            mode: 'no-cors', // 添加在这里
            body: JSON.stringify(data)
          })
          .then(response => response.text())
          .then(result => {
            console.log("Success: " + result);
            document.getElementById("status").innerHTML = "打卡完成";
          })
          .catch(error => {
            console.error('Error:', error);
            document.getElementById("status").innerHTML = "Error: " + error;
          });
        }
    </script>
  </head>
  <body>
    <h1 id="title">溫暖員工打卡系統</h1>
    <!-- 添加中心化的图片 -->
    <img src="https://i.ibb.co/rGCQHSG/clock-in-image.png" alt="打卡图片" class="center-img">
    <!-- 打卡状态文字 -->
    <div id="status">Loading...</div>
  </body>
</html>
