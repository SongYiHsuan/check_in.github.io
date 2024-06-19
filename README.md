<html>
  <head>
    <base target="_top">
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
          fetch('https://script.google.com/macros/s/AKfycbx_IKGuWrt5aiWU5o57d0hyaetLej2eMUjAAP_UsCfGvEJbqcTFn3B0Z74PxX2m50BiAw/exec', {
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
    <div id="status">Loading...</div>
  </body>
</html>
