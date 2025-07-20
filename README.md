<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1" />
  <title>Windy Radar Clean</title>
  <style>
    body {
      margin: 0;
      background: #000;
      color: white;
      font-family: Arial, sans-serif;
      display: flex;
      flex-direction: column;
      align-items: center;
      height: 100vh;
      padding: 10px;
      box-sizing: border-box;
      overflow: hidden;
    }

    #radarContainer {
      position: relative;
      width: 90vw;
      max-width: 900px;
      height: 75vh;
      border-radius: 12px;
      overflow: hidden;
      box-shadow: 0 0 20px rgba(0,0,0,0.7);
      margin-bottom: 15px;
    }

    iframe {
      width: 100%;
      height: 100%;
      border: none;
      display: block;
    }

    #menuButton {
      opacity: 0;
      pointer-events: none;
    }
  </style>
</head>
<body>

  <div id="radarContainer">
    <iframe id="radarFrame" src="" allowfullscreen title="Windy Radar"></iframe>

    <div id="menuButton" title="Open layer menu" aria-haspopup="true" aria-expanded="false" role="button" tabindex="0">
      <div class="dot"></div>
      <div class="dot"></div>
      <div class="dot"></div>
    </div>
  </div>

  <script>
    const iframe = document.getElementById('radarFrame');
    const menuButton = document.getElementById('menuButton');

    let currentLat = 39.8283; // Default to center of USA
    let currentLon = -98.5795;
    let currentZoom = 5;
    const defaultOverlay = "radar";

    function loadRadarOverlay(overlay) {
      const src = `https://embed.windy.com/embed2.html?lat=${currentLat}&lon=${currentLon}&zoom=${currentZoom}&level=surface&overlay=${overlay}&menu=false&message=false&marker=true&type=map&location=coordinates&detailLat=${currentLat}&detailLon=${currentLon}`;
      iframe.src = src;
    }

    // Remove functionality from menuButton
    menuButton.replaceWith(menuButton.cloneNode(true));

    // On page load, get location and load radar
    if (navigator.geolocation) {
      navigator.geolocation.getCurrentPosition(
        pos => {
          currentLat = pos.coords.latitude.toFixed(4);
          currentLon = pos.coords.longitude.toFixed(4);
          loadRadarOverlay(defaultOverlay);
        },
        () => loadRadarOverlay(defaultOverlay)
      );
    } else {
      loadRadarOverlay(defaultOverlay);
    }
  </script>

</body>
</html>
