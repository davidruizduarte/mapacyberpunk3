# mapacyberpunk3
import matplotlib.pyplot as plt
import networkx as nx
import folium

# --- Setup: Districts, Coordinates, and Connections ---
coordinates = {
    "The Sprawl": (35.6804, 139.7690),
    "Tech Zone": (40.7128, -74.0060),
    "Old Town": (51.5074, -0.1278), 
    "The Decks": (47.6062, -122.3321),
}

G = nx.Graph()
G.add_nodes_from(coordinates.keys())
G.add_edge("The Sprawl", "Tech Zone", weight=3)
G.add_edge("Tech Zone", "Old Town", weight=1)
# ... Add more connections if needed

# --- NetworkX Graph Plotting ---
plt.figure(figsize=(8, 6))
edge_widths = [G[u][v]['weight'] for u, v in G.edges()]
nx.draw(G, coordinates, with_labels=True, node_color='skyblue', edge_color='gray', width=edge_widths)
plt.show()

# --- Folium Map Setup ---
map_location = [45.5231, -122.6765] 
map_zoom = 6
map_tiles = 'OpenStreetMap'  

map = folium.Map(location=map_location, zoom_start=map_zoom, tiles=map_tiles)

# Add Markers with Popups 
for district, coords in coordinates.items():
    popup_html = f"""
                  <h1>{district}</h1>
                  <p>A short description or lore piece goes here...</p> 
                  """  
    folium.Marker(coords, popup=popup_html).add_to(map)

# --- Customization: Neon Edge Effect (Experimental) ---
map.get_root().html.add_child(folium.Element("""
<style>
    /* Adjust CSS for your desired neon effect */ 
    #map path {
        stroke: #FFD700;
        stroke-width: 4px;
        stroke-linecap: round;
        stroke-opacity: 0.8;
        filter: blur(2px); 
    } 
</style> """))

# Save the map
map.save("cyberpunk_map.html")
