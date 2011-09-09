# cables.csv to graph to svg

the short version

create list of IDs and edges as plain text files

    $ python ref.py cables.csv (optional)

read and modify the filter in c2g.py, then create any.gml
    
    $ python c2g.py

create html with inline svg

    $ python g2svg.py -g any.gml

# note

* dont run ref.py, the code comes with all generated files
* c2g.py reads calccache and graphcache, so you dont have to calculate
  the graph weight again. Can take 10-15min if you remove the cache.
* change svg.tmpl to customize the output
* g2svg.py -h for more options

# example filter for c2g.py

find all related embassies for the two embassies of intrest

    $ grep -E '(ALEXANDRIA|CAIRO)' edges.list | grep -Eo '[A-Z]+' | sort | uniq

remove useless values from the list and convert to a python dict

    e = ['ABUDHABI','ADDISABABA','ALEXANDRIA','ALGIERS',...]

create a sub-graph with the selection of vertices

    sg = g.subgraph(g.vs.select(_degree_gt=1, place_in=e))

find the clusters of the sub-graph. 

    for c in sg.clusters():

create another sub-graph for each cluster

    ssg = sg.subgraph(sg.vs.select(c))

save as .gml file

    ssg.write_gml('egypt-rel-%d.gml' % i)

find the clusters that include the initial embassies

    grep -lE '(ALEXANDRIA|CAIRO)' egypt-rel*gml

render the .gml with g2svg.py or gephi.org

for the full code check the "egypt" branch

# copyleft

GPLv3

# contact

need help? ask!

https://twitter.com/c2graph

