<!DOCTYPE html>
<html>
  <head>
    <base target="_top">
    <script>
      function getLocation() {
        if (navigator.geolocation) {
          navigator.geolocation.getCurrentPosition(sendPosition, showError);
        } else {
          document.getElementById("status").innerHTML = "Geolocation is not supported by this browser.";
        }
      }

      function sendPosition(position) {
        google.script.run.withSuccessHandler(function(response) {
          document.getElementById("status").innerHTML = response;
        }).submitLocation(position.coords.latitude, position.coords.longitude);
      }

      function showError(error) {
        switch(error.code) {
          case error.PERMISSION_DENIED:
            document.getElementById("status").innerHTML = "User denied the request for Geolocation."
            break;
          case error.POSITION_UNAVAILABLE:
            document.getElementById("status").innerHTML = "Location information is unavailable."
            break;
          case error.TIMEOUT:
            document.getElementById("status").innerHTML = "The request to get user location timed out."
            break;
          case error.UNKNOWN_ERROR:
            document.getElementById("status").innerHTML = "An unknown error occurred."
            break;
        }
      }
    </script>
  </head>
  <body onload="getLocation()">
    <div id="status">Getting your location...</div>
  </body>
</html>
