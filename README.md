<!DOCTYPE html>
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
            const action = urlParams.get('action');
            const location = urlParams.get('location');
            const latitude = position.coords.latitude;
            const longitude = position.coords.longitude;

            const data = {
                identifier: identifier,
                action: action,
                location: location,
                latitude: latitude,
                longitude: longitude
            };

            fetch('https://script.google.com/macros/s/AKfycbwqviA4C0acJWzpsfwOtNcAwTW5XzGP1enjKWj77SfZLsYlomR8EBxgv32l1dQqBqHtVA/exec', {
                method: 'POST',
                headers: {
                    'Content-Type': 'application/json'
                },
                body: JSON.stringify(data)
            })
            .then(response => response.text())
            .then(result => {
                console.log("Success: " + result);
                document.getElementById("status").innerHTML = result;
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
