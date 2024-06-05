<html>
  <head>
    <base target="_top">
    <script>
      function getLocation() {
        if (navigator.geolocation) {
          console.log('test')
          navigator.geolocation.getCurrentPosition(sendPosition);
          console.log('test2')
        } else {
          document.getElementById("status").innerHTML = "Geolocation is not supported by this browser.";
        }
      }

      function sendPosition(position) {
        google.script.run.withSuccessHandler(function(response) {
          document.getElementById("status").innerHTML = response;
        }).submitLocation(position.coords.latitude, position.coords.longitude);
      }
  </script>
  </head>
  <body onload="getLocation()">
    <div id="status">打卡完成</div>
  </body>
</html>
