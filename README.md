The Megatrail project is my attempt to detail the largest connected component of a network consisting of bike/hiking trails, sidewalks, and designated pededstrian crossings in a given region of the world, which I dub a "megatrail" for the purposes of this project. In this script, I use Chicago as an example. 

This currently consists of 2 parts.

# Gathering
The map-gathering script uses the overpass API for OpenStreetMap to gather all relevant nodes and ways (ie, nodes and ways that correspond to the appropriate pedestrian/cyclist infrastructure) from a given area. It then assembles a networkx graph from all of the gathered nodes and extracts the largest connected component (LCC) from them. It then uses a breadth-first search algorithm to extenditself; namely, it looks for cardinal directions in which the ways overextend the bounding box that was used to extract the initial nodes and ways, creates a bounding box of equal size in that direction, and adds it to a queue. It then takes the oldest member of a queue, extracts new relevant nodes and ways from its associated bounding box, adds it to the networkX graph, and creates a new LCC from this data. The process repeats until a preconfigured cutoff point, or until there are no usable bboxes left (that is to say, the megatrail has been fully mapped).

# Community Analysis

I'm currently performing community analysis on the Chicago megatrail, in an effort to better grasp its overall structure. I initially used the Louvain algorithm, but found that the communities it formed were too granular to be useful. Geography-based k-means clustering did not pay attention to the connectivity of the nodes, and node2vec-based k-means clustering was too computationally intensive. As such, I am using Clauset-Newman-Moore greedy modularity maximization, as provided by the networkx library, with a resolution of .00001, to break up the megatrail into 7 communities. 

Of the 7 communities, 3 major hubs exist:
1. The city of Chicago and its immediate outlying regions, such as Evanston and Blue Island,
2. The Aurora/Naperville area.
3. Western suburbs to the north of the Aurora/Naperville area, such as Batavia and St. Charles

There are also 4 comunities branching off of this hubs, namely
1. The Elgin/Crystal Lake region, to the north of the St. Charles community,
2. The Oswego/Plano region, to the southeast of the Aurora/Naperville community,
3. The Plainfield region, to the south of the Aurora/Naperville community,
4. The northeastern suburbs of Chicago, located to the north of Evanston.

# Current efforts

1. Attempting to classify communities in a region based on average degree, number of nodes/distance provided within a community, and number of intercommunity connections.
2. Attempting to determine, ideally algorithmically but manually if need be, what geographic features demarcate communities in a Megatrail. My current hypothesis is that it's mostly a combination of natural features (such as rivers) and major thoroughfares (ie, highways).
3. Attempting to maximize the geographic scope of the Megatrail by updating OpenStreetMap with appropriate corrections. 
