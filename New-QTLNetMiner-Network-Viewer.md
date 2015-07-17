The new pre-release version of the Network Viewer for **QTLNetMiner**, which uses cytoscapeJS instead of JAVA for the visualizing the network graphs, has now been deployed for the QTLNetMiner **[Maize](https://ondex.rothamsted.ac.uk/QTLNetMinerMaize)** prototype. 

The new version improves on the performance and rendering capabilities of the existing Java-based Network Viewer.

In QTLNetMiner, when users search using some query terms they are shown a table under _Gene View_ displaying the search results. Now, when users select one or more targets from the table (using their checkboxes), they have the option of either using the old Java version (using the “_Show Network_” button) or the new version (using the “_New Network Viewer_” button) to view the network. The first button launches the old Java-based embedded Network Viewer. When the second button is clicked, the new Network Viewer opens in a new _pop-up_ window (_users might need to allow pop-ups_).

**Note:** Users can also use existing _sample queries_ (as shown below) to search the knowledge network with example query terms and gene lists and performing specific Genome or QTL search.
![Using Sample Queries](https://ondex.rothamsted.ac.uk/QTLNetMiner/New_Network_1.png)

### New Network Viewer button in Gene View
After searching in QTLNetMiner using some query terms, the _Gene View_ table will be displayed with _two_ buttons underneath it (as shown below).
**Note:** If both buttons are not visible, users should **_empty the cache_** in their web browser and reload the QTLNetMiner webpage.
![Both Network Viewer Buttons](https://ondex.rothamsted.ac.uk/QTLNetMiner/New_Network_2.png)

### Network Viewer window
Pressing the second button (the “_New Network Viewer_” button) launches the new Network Viewer in a new window, as shown below:
![New Network Viewer](https://ondex.rothamsted.ac.uk/QTLNetMiner/NewNetworkViewer.png)

### Features
Some of its features are detailed below to help users get familiarised with it:

* **Concepts** (nodes) are displayed using different symbols and colours like in the Java version (detailed in the Legend below the graph). **Relations** (edges) too use various colours depending on the type of concept, as in the old Java version.

* Users can click a concept or relation to view some more information about them or drag them (click and hold) to move them around. Users can also drag the entire graph around by dragging the viewport (white background).

* **Touch gestures**: The new Network Viewer also has touchscreen compatibility and can be used on tablets, touch PC’s and smartphones. Touch gestures such as tap (click), hold, drag, etc. have been incorporated within the new version.

* **Labels** on concepts and relations are not disabled by default. These can however be enabled if the user wants. User’s search query terms, if found in these labels, are now _highlighted_ as well.

* **Context menu**: Right-clicking a concept or relation opens a circular context menu with features like **Item Info.** (to display specific information about the selected concept or relation), **Show Links** (to show hidden elements in its neighbourhood), **Hide** (to hide the selected concept or relation), **Hide by Type** (to hide all the concepts or relations of a particular type, i.e., the same type as the selected concept or relation), **Label on/ off** (to toggle the visibility of the Label on/ off for the selected concept or relation) and **Label on/ off by Type** (to toggle the visibility of Labels on/ off for all concepts or relations of a particular Type). 

* There are **Sliding panels** on the top, right and bottom of the Network Viewer pop-up window which you can click to open or close and drag/ slide to re-size. The top panel allows users to: 
    1. Change the graph’s **Layout** using force-directed layout algorithms. Some of the useful layout options are the default (WebCola), circular, Arbor, Grid and Concentric.
    1. **Re-layout** the entire graph.
    1. Enable/ disable layout animation (useful for very large graphs to improve performance).
    1. **Search** by concept name (or part of concept name).
    1. Export graph data and visual attributes as **JSON**.
    1. Export graph **Image** (screenshot).
    1. Reset the graph viewport.
    1. Make **labels** visible on concepts and relations, via checkboxes.

* **Item Info.** panel: The panel on the right is to display relevant information related to the selected concept or relation. It automatically slides open if users right-click a concept or relation and select “Item Info” option. The panel is similar to the Item Info window in the old version and displays information such as concept/ relation Type, value, PID, relation label, relation source (from), relation target (to), _Annotations, Attributes_ (such as publication abstracts, title, authors, amino-acid sequence, TAX ID, etc.) and _Accessions_ (with links to TAIR, Ensembl, UniProtKB, PubMed, KEGG, IPRO, PFAM, etc., where relevant). User’s search query terms, where found, are _highlighted_ in this Item Info. panel.

* **Show Links**: Some concepts have a blur effect which denotes that they have hidden concepts connected to them. These can be displayed by right-clicking on a blurred concept and selecting “Show Links”. This displays the hidden neighbourhood for the selected concept.

* **Flagged genes**: All the genes are displayed as blue triangles but the gene(s) originally selected for viewing in the network window have a double border to visually distinguish them from other genes. These were shown in the old version with a blue flag next to them. 

**_Note:_** While this new Network Viewer is stable, it is still under development and more features will be added to it in the near future and it will soon be _live_ on all the various QTLNetMiner instances.