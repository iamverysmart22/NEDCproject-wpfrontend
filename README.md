# NEDCproject-wpfrontend
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Location Finder</title>
    <style>
        body {
            font-family: Arial, sans-serif;
            margin: 20px;
            padding: 20px;
        }
        .container {
            max-width: 600px;
            margin: auto;
            text-align: center;
        }
        input[type="text"] {
            width: 80%;
            padding: 10px;
            margin-top: 10px;
        }
        button {
            padding: 10px 20px;
            margin-top: 10px;
            cursor: pointer;
        }
        #result {
            margin-top: 20px;
        }
    </style>
</head>
<body>

<div class="container">
    <h1>Location Finder</h1>

    <!-- Show user's current latitude and longitude -->
    <p id="currentLocation"></p>

    <!-- Ask for Address -->
    <p>Enter your address:</p>
    <input type="text" id="address" placeholder="Enter address here" required>
    <button onclick="getCoordinates()">Get Coordinates</button>

    <!-- Display the calculated latitude and longitude -->
    <div id="result"></div>
</div>

<script src="https://maps.googleapis.com/maps/api/js?key=YOUR_GOOGLE_API_KEY&callback=initMap" async defer></script>

<script>
// Function to get current location (latitude and longitude)
function getCurrentLocation() {
    if (navigator.geolocation) {
        navigator.geolocation.getCurrentPosition(function(position) {
            const lat = position.coords.latitude;
            const lon = position.coords.longitude;

            // Display current location
            document.getElementById('currentLocation').innerHTML = `Current Location: Latitude: ${lat}, Longitude: ${lon}`;
        });
    } else {
        document.getElementById('currentLocation').innerHTML = "Geolocation is not supported by this browser.";
    }
}

// Function to get latitude and longitude of the entered address using Google Geocoding API
function getCoordinates() {
    const address = document.getElementById('address').value;
    const geocoder = new google.maps.Geocoder();

    // Geocode the address
    geocoder.geocode({ 'address': address }, function(results, status) {
        if (status == google.maps.GeocoderStatus.OK) {
            const lat = results[0].geometry.location.lat();
            const lon = results[0].geometry.location.lng();

            // Display the result
            document.getElementById('result').innerHTML = `The coordinates for "${address}" are: <br>Latitude: ${lat}, Longitude: ${lon}`;
        } else {
            document.getElementById('result').innerHTML = "Unable to find location. Please try again.";
        }
    });
}

// Load current location when the page loads
window.onload = getCurrentLocation;
</script>

</body>
</html>
