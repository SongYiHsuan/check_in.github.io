<!DOCTYPE html>
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
        window.onload = function () {
          // 如果緩存中有地理位置，直接使用
          if (sessionStorage.getItem("userLocation")) {
            const cachedLocation = JSON.parse(sessionStorage.getItem("userLocation"));
            console.log("Using cached location:", cachedLocation);
            sendPosition({
              coords: {
                latitude: cachedLocation.latitude,
                longitude: cachedLocation.longitude,
              },
            });
          } else {
            getLocation(); // 否則請求地理位置
          }
        };

        function getLocation() {
          if (navigator.geolocation) {
            navigator.geolocation.getCurrentPosition(
              (position) => {
                const locationData = {
                  latitude: position.coords.latitude,
                  longitude: position.coords.longitude,
                };
                // 將位置存入緩存
                sessionStorage.setItem("userLocation", JSON.stringify(locationData));
                console.log("Location obtained:", locationData);
                sendPosition(position);
              },
              showError
            );
          } else {
            console.log("Geolocation is not supported by this browser.");
            document.getElementById("status").innerHTML =
              "Geolocation is not supported by this browser.";
          }
        }

        function showError(error) {
          let errorMessage;
          switch (error.code) {
            case error.PERMISSION_DENIED:
              errorMessage = "使用者拒絕了定位請求。";
              break;
            case error.POSITION_UNAVAILABLE:
              errorMessage = "定位資訊不可用。";
              break;
            case error.TIMEOUT:
              errorMessage = "請求定位超時。";
              break;
            case error.UNKNOWN_ERROR:
              errorMessage = "未知錯誤。";
              break;
          }
          console.log("Error getting location:", errorMessage);
          document.getElementById("status").innerHTML = errorMessage;
        }

        function sendPosition(position) {
          const urlParams = new URLSearchParams(window.location.search);
          const identifier = urlParams.get("identifier");
          const location = urlParams.get("location");
          const action = urlParams.get("action");
          const latitude = position.coords.latitude;
          const longitude = position.coords.longitude;

          const data = {
            identifier: identifier,
            location: location,
            action: action,
            latitude: latitude,
            longitude: longitude,
          };

          console.log("Sending data:", data);

          fetch(
            "https://script.google.com/macros/s/AKfycbxRGFbQLWPAqHVfMUow7k5BSsslm6p7UtzYYKrZDXNTjmGGAHAysoZSLpLtk6hU8zAgHA/exec",
            {
              method: "POST",
              headers: {
                "Content-Type": "application/json",
              },
              body: JSON.stringify(data),
            }
          )
            .then((response) => {
              if (!response.ok) {
                throw new Error(`HTTP error! Status: ${response.status}`);
              }
              return response.text();
            })
            .then((result) => {
              console.log("Success:", result);
              document.getElementById("status").innerHTML = "打卡完成";
            })
            .catch((error) => {
              console.error("Error:", error);
              document.getElementById("status").innerHTML =
                "Load failed: " + error.message;
            });
        }
    </script>
  </head>
  <body>
    <h1 id="title">溫暖員工打卡系統</h1>
    <!-- 添加中心化的图片 -->
    <img
      src="https://i.ibb.co/rGCQHSG/clock-in-image.png"
      alt="打卡圖片"
      class="center-img"
    />
    <!-- 打卡状态文字 -->
    <div id="status">Loading...</div>
  </body>
</html>
