<html lang="en" >
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1" />
  <title>Windy Radar with Invisible Menu Button</title>
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

    /* Hide the menu button and disable interaction */
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

  <script>
    const iframe = document.getElementById('radarFrame');
    const layerSelect = document.getElementById('layerSelect');
    const menuButton = document.getElementById('menuButton');
    const layerMenu = document.getElementById('layerMenu');

    let currentLat = 39.8283; // default USA center
    let currentLon = -98.5795;
    let currentZoom = 5;

    function loadRadarOverlay(overlay) {
      const src = `https://embed.windy.com/embed2.html?lat=${currentLat}&lon=${currentLon}&zoom=${currentZoom}&level=surface&overlay=${overlay}&menu=false&message=false&marker=true&type=map&location=coordinates&detailLat=${currentLat}&detailLon=${currentLon}`;
      iframe.src = src;
    }

    // Remove event listeners on menuButton so it does nothing
    // Simplest way: clone and replace
    menuButton.replaceWith(menuButton.cloneNode(true));

    // Load new overlay on selection
    layerSelect.addEventListener('change', () => {
      loadRadarOverlay(layerSelect.value);
      layerMenu.style.display = 'none';
      menuButton.setAttribute('aria-expanded', 'false');
    });

    // On page load, get location and load default overlay
    if (navigator.geolocation) {
      navigator.geolocation.getCurrentPosition(
        pos => {
          currentLat = pos.coords.latitude.toFixed(4);
          currentLon = pos.coords.longitude.toFixed(4);
          loadRadarOverlay(layerSelect.value);
        },
        () => loadRadarOverlay(layerSelect.value)
      );
    } else {
      loadRadarOverlay(layerSelect.value);
    }
  </script>

</body>
</html>
