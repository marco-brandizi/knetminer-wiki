Welcome to the KnetMiner wiki!

This wiki contains detailed information about the installation and development of KnetMiner, and how to set up a knowledge network using Ondex.

### Brief overview

[Ondex](http://www.ondex.org/index.shtml) is a software application that allows users to use (biological) data from various sources to create graph-based knowledge networks. The data used is mostly in the form of gene-protein networks and additional information (such as GO-terms or homologies) for these proteins. Ondex represents genes or proteins in as concepts (aka nodes) that are connected by relations (aka edges) such as "encodes for", "cooccurs_with" and so on.

Ondex itself is a GUI-based application but also has a terminal-based application called ondex-mini, which should be used for working with larger datasets.

KnetMiner is a web application that represents Ondex-networks in a webpage for easier searching and browsing. KnetMiner consists of a client deployed using any web server. Users enter their search terms into the web page's search engine. The client then takes these search-terms and sends them to the KnetMiner server, which searches the Ondex network for any concepts/nodes relating to these terms. The server then writes the results to a temporary file, after which the client parses this file and represents the results visually to the user. The search results are aggregated and scored using Lucene search.