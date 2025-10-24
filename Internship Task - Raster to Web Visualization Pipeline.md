**Objective:** Build an end-to-end pipeline to process a raster dataset and visualize it in a web browser.

**Key Tasks:**

*   **Data Acquisition & Preparation:**
    *   Source a sample GeoTIFF (e.g., a DEM or land cover map).
    *   Use **GDAL/OGR** (command line or Python) to analyze the raster's properties. (Do research about it for any other options).

*   **COG Conversion:**
    *   Convert the source raster into a **Cloud Optimized GeoTIFF (COG)**.
    *   Ensure the COG is created with internal tiling, overviews, and optimal compression.

*   **Serving with GeoServer:**
    *   Publish the generated COG to **GeoServer** by creating a new Store and Layer.
    *   Configure the layer to serve data via **WMS** (Web Map Service).

*   **Frontend Integration:**
    *   Create a simple web page with a JavaScript mapping library (e.g., **Leaflet** or **OpenLayers**).
    *   Integrate the GeoServer WMS layer into the map to display the COG.
    *   (Bonus): Add interactivity like layer opacity control.

**Final Deliverable:** A brief report with code snippets and a URL to the functional web map demonstrating the successfully loaded COG.