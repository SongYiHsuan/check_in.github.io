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
        width: 50%;
      }
      /* 页面加载状态文字的样式 */
      #status {
        text-align: center;
        font-size: 18px;
        margin-top: 20px;
      }
      #title {
        text-align: center;
        font-size: 36px;
        margin-top: 20px;
        font-family: 'Arial', sans-serif;
      }
    </style>
    <script>
      window.onload = function () {
        // 檢查 sessionStorage 中是否已經存在地理位置資料
        if (!sessionStorage.getItem("userLocation")) {
          getLocation(); // 如果沒有，請求地理位置
        } else {
          // 如果已經存在，使用緩存的地理位置
          const location = JSON.parse(sessionStorage.getItem("userLocation"));
          console.log("Using cached location:", location);
          sendPosition({
            coords: { latitude: location.latitude, longitude: location.longitude },
          });
        }
      };

      function getLocation() {
        if (navigator.geolocation) {
          navigator.geolocation.getCurrentPosition(
            (position) => {
              // 儲存位置到 sessionStorage
              const locationData = {
                latitude: position.coords.latitude,
                longitude: position.coords.longitude,
              };
              sessionStorage.setItem("userLocation", JSON.stringify(locationData));

              // 傳送位置到伺服器
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
        console.log("Error getting location: " + errorMessage);
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

        console.log("Sending data: ", data);

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
              throw new Error(`HTTP error! status: ${response.status}`);
            }
            return response.text();
          })
          .then((result) => {
            console.log("Success: " + result);
            document.getElementById("status").innerHTML = "打卡完成";
          })
          .catch((error) => {
            console.error("Error:", error);
            document.getElementById("status").innerHTML = "Error: " + error.message;
          });
      }
    </script>
  </head>
  <body>
    <h1 id="title">溫暖員工打卡系統</h1>
    <img
      src="https://i.ibb.co/rGCQHSG/clock-in-image.png"
      alt="打卡圖片"
      class="center-img"
    />
    <div id="status">Loading...</div>
  </body>
</html>
