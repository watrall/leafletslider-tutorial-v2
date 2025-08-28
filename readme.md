# Leaflet Time-Slider Map Tutorial (v2)

This tutorial will teach you, step by step, how to create a web map with a **time slider**.  
The slider will let you move through different years (or dates) and see points appear on the map.

This tutorial is based on a [previous verion](https://github.com/chi-initiative/LeafletSlider-tutorial) created by [Brian Geyer](https://github.com/geyerbri).  This version contains significant updates and improvements.  

⚠️ This tutorial assumes **no technical background at all**.  
Every step is written as if you have never created a web page before.

---

## What You Will End Up With
At the end of this tutorial, you will have a folder on your computer that contains:
- A web page (`index.html`)  
- A plugin file (`SliderControl.js`)  
- An optional data file (`popups.json`)  

When you double-click `index.html`, a map will open in your browser with markers that appear when you drag a time slider.

---

## Step 1: Make a Project Folder
1. On your **desktop**, right-click → **New Folder**.  
2. Name the folder: `leaflet-map` (you can choose a different name, but keep it simple).

Inside that folder, create two more folders:
- One named `js`  
- One named `data`  

Your folder should look like this (empty for now):

```
leaflet-map/
├─ index.html
├─ js/
├─ data/
```

---

## Step 2: Create Your First File (index.html)
This will be your main web page.

1. Open **Notepad (Windows)** or **TextEdit (Mac)**.  
   - On Mac: go to `Format → Make Plain Text` before pasting code.  
2. Copy **all the text below**.  
3. Paste it into the blank file.  
4. Save it into your `leaflet-map` folder as:  
   **`index.html`** (make sure the file ends with `.html`, not `.txt`).

```html
<!doctype html>
<html lang="en">
  <head>
    <meta charset="utf-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1" />
    <title>Leaflet Time-Slider Tutorial</title>

    <!-- Leaflet (map library) CSS and JS -->
    <link
      rel="stylesheet"
      href="https://unpkg.com/leaflet@1.9.4/dist/leaflet.css"
    />
    <script src="https://unpkg.com/leaflet@1.9.4/dist/leaflet.js"></script>

    <!-- jQuery (needed for the slider) -->
    <script src="https://code.jquery.com/jquery-3.7.1.min.js"></script>

    <!-- jQuery UI (the actual slider widget) -->
    <link rel="stylesheet" href="https://code.jquery.com/ui/1.13.2/themes/base/jquery-ui.css" />
    <script src="https://code.jquery.com/ui/1.13.2/jquery-ui.min.js"></script>

    <!-- Touch Punch (lets slider work on mobile touch screens) -->
    <script src="https://cdnjs.cloudflare.com/ajax/libs/jqueryui-touch-punch/0.2.3/jquery.ui.touch-punch.min.js"></script>

    <!-- Time-Slider plugin JS -->
    <script src="js/SliderControl.js"></script>

    <!-- Simple CSS so the map fills the window -->
    <style>
      html, body { height: 100%; margin: 0; }
      #myMap { width: 100%; height: 100%; }
    </style>
  </head>
  <body>
    <div id="myMap">If you see this text, the map didn’t load.</div>

    <script>
      // 1) Create the map
      // The numbers [46.26, -119.15] are latitude/longitude where the map starts.
      // The 12 is the zoom level (smaller = zoomed out, larger = zoomed in).
      const map1 = L.map('myMap').setView([46.26, -119.15], 12);

      // 2) Add the background map (tiles)
      L.tileLayer('https://tile.openstreetmap.org/{z}/{x}/{y}.png', {
        attribution: '© OpenStreetMap contributors'
      }).addTo(map1);

      // 3) Add some points (markers) with dates
      // NOTE: GeoJSON format uses [longitude, latitude] (not latitude first!)
      const dataset1 = [
        {
          type: 'Feature',
          geometry: { type: 'Point', coordinates: [-119.0977, 46.2465] },
          properties: { title: 'Popup 1', description: 'Hello World!', time: '1992/01' }
        },
        {
          type: 'Feature',
          geometry: { type: 'Point', coordinates: [-119.2019, 46.2570] },
          properties: { title: 'Popup 2', description: 'Hello again!', time: '1992/06' }
        },
        {
          type: 'Feature',
          geometry: { type: 'Point', coordinates: [-119.1080, 46.2309] },
          properties: { title: 'Popup 3', description: 'Another point!', time: '1992/07' }
        }
      ];

      // 4) Define what happens when you click a marker (popup)
      function onEachFeature(feature, layer) {
        const p = feature.properties || {};
        const content = `<h4>${p.title}</h4>
                         <p><strong>${p.time}</strong></p>
                         <p>${p.description}</p>`;
        layer.bindPopup(content);
      }

      // 5) Turn the dataset into a Leaflet layer
      const group1 = L.geoJSON(dataset1, { onEachFeature });

      // 6) Add the slider that controls visibility by time
      const sliderControl1 = L.control.sliderControl({
        layer: group1,
        alwaysShowDate: true,
        showAllPopups: false,
        showPopups: false
      });

      map1.addControl(sliderControl1);
      sliderControl1.startSlider();
    </script>
  </body>
</html>
```

---

## Step 3: Add the Plugin File
The slider requires a small extra script.

1. In your `leaflet-map/js/` folder, create a new file named:  
   **`SliderControl.js`**  
2. Copy the plugin code you downloaded (or that came with this tutorial) into that file.  
3. Save it.  

**Important:** The file name must be **exactly** `SliderControl.js` and it must be inside the `js` folder.

---

## Step 4: Open the Map
1. Go to your `leaflet-map` folder.  
2. Double-click `index.html`.  
3. Your default browser (Chrome, Firefox, Edge, Safari) should open.  
4. You should see a map with a slider in the corner. Drag it to see markers appear.

---

## Step 5: Troubleshooting
- **Map is blank?**  
  Make sure your file is saved as `index.html` (not `index.html.txt`).  

- **Markers in the wrong place?**  
  Remember: GeoJSON uses `[longitude, latitude]`. Longitude is left-right, latitude is up-down.  

- **Slider doesn’t show up?**  
  Check that you created the `js/SliderControl.js` file and the name matches exactly.  

- **Still broken?**  
  In your browser, right-click → “Inspect” → click the **Console** tab. Red error messages will tell you what’s wrong (like a missing file).

---

## Step 6 (Optional): Publish Online with GitHub Pages
If you want to share your map with others:

1. Create a free account at [https://github.com](https://github.com).  
2. Upload your entire `leaflet-map` folder into a new repository.  
3. In your repo, go to **Settings → Pages**.  
4. Under **Source**, choose your **main branch** and the **root folder**.  
5. Save. GitHub will give you a link (like `https://yourname.github.io/leaflet-map`).  

---

## Next Steps
- Replace the sample points with your own data.  
- Store data in `data/popups.json` and load it with `$.getJSON`.  
- Add more features like custom icons, legends, or multiple datasets.

---

## Credits
- Map powered by [Leaflet](https://leafletjs.com/)  
- Slider plugin (`SliderControl.js`)  
- Background tiles by [OpenStreetMap](https://www.openstreetmap.org/copyright)  

