---
layout: post
title: Subgroup Explorer
tag: 
- Subgroup Lattices
---

To sum up the subgroup lattice series, I've written a subgroup lattice generator. It's powered by Sage and GAP, and allows you to view the lattice of subgroups or subgroup conjugacy classes of a group from your browser.

<!--more-->

<div class="auto">
  <script type="text/x-sage">
from collections import defaultdict

def subgroup_conj_classes(G):
    """
    Returns [cc_1, cc_2, ... cc_n] : each cc_i is a list containing subgroups of G that belong to the same conjugacy class.
    """
    ccs = G._gap_().ConjugacyClassesSubgroups()
    return [tuple([G.subgroup(gap_group = H) for H in cc.Elements()]) for cc in ccs]

def are_subgroups(cc1,cc2):
    """
    Returns True if some element of cc1 is a subgroup of an element of cc2.
    """    
    # Choose the shorter list to iterate over
    if len(cc1) <= len(cc2):
        h2 = cc2[0]
        for h1 in cc1:
            if h1.is_subgroup(h2):
                return True
    else:
        h1 = cc1[0]
        for h2 in cc2:
            if h1.is_subgroup(h2):
                return True
    return False


@interact
def subgroup_class_lattices(Cardinality= 6):
    group_list = {Cardinality: {}}
    for G_gap in gap.AllSmallGroups(Cardinality):
        G = PermutationGroup(list(gap.GeneratorsOfGroup(G_gap.AsPermGroup())))
        group_list[Cardinality][G.structure_description()] = str(G.gens())
    @interact
    def group_select(Group = selector(values = group_list[Cardinality].keys())):
        # Generate group
        G = PermutationGroup(gap(group_list[Cardinality][Group]))
        
        # Poset of conjugacy classes
        sub_classes = subgroup_conj_classes(G)
        poset = Poset((sub_classes,are_subgroups)) 
        
        @interact
        def display_options(Show = selector(values = ['Conjugacy classes of subgroups', 'All subgroups']), 
                            Vertex_Colors = selector(values = ['Normal (Green), Commutator (Pink), Center (Blue)','None'], label = 'Vertex colors'), 
                            Edge_Colors = selector(values = ['Is normal subgroup of','None',], label = 'Edge colors'), 
                            Edge_Labels = selector(values =['Contains','Contained by', 'Both','None',], label = 'Edge labels')):
            if Show == 'All subgroups':
                # Poset of all subgroups
                subgroups = [h for cc in sub_classes for h in cc ]
                covers = []
                for cc1,cc2 in poset.cover_relations():
                    for h1 in cc1:
                        for h2 in cc2:
                            if h1.is_subgroup(h2):
                                covers.append([h1,h2])

                full_poset = Poset((subgroups,covers),cover_relations = True)
                
                # Define vertex colors
                if Vertex_Colors is not 'None':
                    vertex_colors = defaultdict(list)
                    for h in subgroups:
                        # Color non-normal subgroups white
                        if not h.is_normal():
                            vertex_colors['white'].append(h)
                        else:
                            # Color the commutator subgroup pink
                            if h == G.subgroup(G.commutator().gens()):
                                vertex_colors['pink'].append(h)
                            # Color the center lightblue
                            elif h == G.center():
                                vertex_colors['lightblue'].append(h)
                            # Color all other normal subgroups green
                            else:
                                vertex_colors['lightgreen'].append(h)
                else:
                    vertex_colors = 'white'

                # Define edge colors
                if Edge_Colors is not 'None':
                    edge_colors = {'#60D6D6':[],'lightgray':[]}
                    for h,k in full_poset.cover_relations():
                        if h.is_normal(k):
                            edge_colors['#60D6D6'].append((h,k))
                        else:
                            edge_colors['lightgray'].append((h,k))
                else:
                    edge_colors = None

                # Define vertex labels
                vertex_labels = {h : "$"+h.structure_description(latex=True)+"$" for h in subgroups}

                #### END OF CUSTOM DISPLAY OPTIONS

                # Define heights, if poset is ranked
                rank_function = full_poset.rank_function()
                if rank_function:
                    heights = defaultdict(list)
                    for i in full_poset:
                        heights[rank_function(i)].append(i)
                else:
                    heights = None

                # Generate Hasse diagram
                graph = full_poset.hasse_diagram()

                # Generate graph_plot object
                gplot = graph.graphplot(vertex_labels=None,layout='acyclic',vertex_colors = vertex_colors, edge_colors = edge_colors, vertex_size = 1000)

                # Set vertex labels
                gplot._plot_components['vertex_labels'] = []
                for v in gplot._nodelist:
                    gplot._plot_components['vertex_labels'].append(text(vertex_labels[v],
                        gplot._pos[v], rgbcolor=(0,0,0), zorder=8))

                # Display!
                gplot.show(figsize=(10,10))
                
            else: # Show = 'Conjugacy classes of subgroups'
                # Define vertex colors
                if Vertex_Colors is not 'None':
                    vertex_colors = defaultdict(list)
                    for cc in sub_classes:
                        # Color non-normal subgroups white
                        if not cc[0].is_normal():
                            vertex_colors['white'].append(cc)
                        else:
                            # Color the commutator subgroup pink
                            if cc[0] == G.subgroup(G.commutator().gens()):
                                vertex_colors['pink'].append(cc)
                            # Color the center lightblue
                            elif cc[0] == G.center():
                                vertex_colors['lightblue'].append(cc)
                            # Color all other normal subgroups green
                            else:
                                vertex_colors['lightgreen'].append(cc)
                else:
                    vertex_colors = 'white'

                # Define edge colors
                if Edge_Colors is not 'None':
                    edge_colors = {'#60D6D6':[],'lightgray':[]}
                    for cc1,cc2 in poset.cover_relations():
                        h1 = cc1[0]

                        # Color by whether elts of cc1 are normal subgroups of elts of cc2
                        is_normal = False
                        for h2 in cc2:
                            if h1.is_subgroup(h2) and h1.is_normal(h2):
                                edge_colors['#60D6D6'].append((cc1,cc2))
                                is_normal = True
                                break
                        if not is_normal:
                            edge_colors['lightgray'].append((cc1,cc2))
                else:
                    edge_colors = None

                # Define vertex labels
                vertex_labels = {cc : "$"+cc[0].structure_description(latex=True)+"$" for cc in sub_classes}

                #### END OF CUSTOM DISPLAY OPTIONS

                # Define heights, if poset is ranked
                rank_function = poset.rank_function()
                if rank_function:
                    heights = defaultdict(list)
                    for i in poset:
                        heights[rank_function(i)].append(i)
                else:
                    heights = None

                # Generate Hasse diagram
                graph = poset.hasse_diagram()

                # Set edge labels
                label_edges = True
                if Edge_Labels == 'Contained by':                
                    for cc1,cc2,label in graph.edges():
                        # Count number of subgroups in cc2 that a fixed representative of cc1 is contained by
                        count = sum([cc1[0].is_subgroup(h2) for h2 in cc2])    
                        if count == 1:
                            graph.set_edge_label(cc1,cc2,'')
                        else:
                            graph.set_edge_label(cc1,cc2,'  ' + str(count))
                elif Edge_Labels == 'Contains':        
                    for cc1,cc2,label in graph.edges():
                        # Count number of subgroups in cc1 that a fixed representative of cc2 contains
                        count = sum([h1.is_subgroup(cc2[0]) for h1 in cc1])
                        if count == 1:
                            graph.set_edge_label(cc1,cc2,'')
                        else:
                            graph.set_edge_label(cc1,cc2,'  ' + str(count))
                elif Edge_Labels == 'Both':    
                    for cc1,cc2,label in graph.edges():
                        # Both of the above
                        count1 = sum([cc1[0].is_subgroup(h2) for h2 in cc2])
                        count2 = sum([h1.is_subgroup(cc2[0]) for h1 in cc1])
                        if count1 == 1 and count2 == 1:
                            graph.set_edge_label(cc1,cc2,'')
                        else:
                            graph.set_edge_label(cc1,cc2,'  ' + '{},{}'.format(count1,count2))
                else:
                    label_edges = False


                # Generate graph_plot object
                gplot = graph.graphplot(vertex_labels=None,layout='acyclic',vertex_colors = vertex_colors, edge_colors = edge_colors, edge_labels = label_edges, vertex_size = 1000)

                # Set vertex labels
                gplot._plot_components['vertex_labels'] = []
                for v in gplot._nodelist:
                    gplot._plot_components['vertex_labels'].append(text(vertex_labels[v],
                        gplot._pos[v], rgbcolor=(0,0,0), zorder=8))

                # Display!
                gplot.show(figsize=(7,7))
  </script>
</div>

[Normal subgroups](http://en.wikipedia.org/wiki/Normal_subgroup){:target="_blank"} are colored green. Additionally, the [center](http://en.wikipedia.org/wiki/Center_%28group_theory%29){:target="_blank"} is blue while the [commutator subgroup](http://en.wikipedia.org/wiki/Commutator_subgroup){:target="_blank"} is pink.

Showing the full subgroup lattice can get messy for large groups. If the option `Conjugacy classes` is selected, the viewer only shows the [conjugacy classes of subgroups](http://en.wikipedia.org/wiki/Conjugacy_class#Conjugacy_of_subgroups_and_general_subsets){:target="_blank"} (i.e. all subgroups that are conjugate are combined into a single vertex).

The edge labels indicate how many subgroups of one conjugacy class a given representative subgroup of another conjugacy class **contains**, or how many subgroups of one conjugacy class a given representative subgroup of another conjugacy class is **contained by**. The labels are omitted if these numbers are 1. The edge colors indicate whether the subgroups in the "smaller" conjugacy class are normal subgroups of those in "larger" conjugacy class.

For instance, the group `C15 : C4` (of order 60; the colon stands for [semi-direct product](http://en.wikipedia.org/wiki/Semidirect_product){:target="_blank"} and is usually written $\rtimes$) contains 5 subgroups isomorphic to `C3 : C4`, which in turn contains 3 subgroups isomorphic to `C4` and 1 subgroup isomorphic to `C6` (the 5 belows to another edge). The edge colors indicate that `C6` is a normal subgroup of `C3 : C3` whereas `C4` is not. For further information on group descriptors, click [here](http://groupprops.subwiki.org/wiki/GAP:StructureDescription#Aspects_of_structure_description){:target="_blank"}.

As mentioned in the [previous post](http://sheaves.github.io/Subgroup-Lattice-Edges/){:target="_blank"}, labelling the edges requires taking apart the poset plotting code. I had to extract the Hasse diagram of a poset as a graph and modifying the edge labels directly. This explains why the code is much longer than in previous posts.

Finally, while verifying the results of this program, I found an error in [this book](http://www.cambridge.org/us/academic/subjects/mathematics/algebra/representations-groups-computational-approach){:target="_blank"}!
The correction has been pencilled in. The original number printed was 1.
![A5 Lattice](/images/A5Lattice_CompareSmall.jpg "A5 Subgroup Lattice")

This is the last post on subgroup lattices. My [next post](http://sheaves.github.io/Representation-Theory-Intro/) will be the start of a new series on doing represntation theory in Sage.
