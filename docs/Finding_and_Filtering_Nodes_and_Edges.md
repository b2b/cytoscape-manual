<a id="finding_and_filtering_nodes_and_edges"> </a>
# Finding and Filtering Nodes and Edges

<a id="search_bar"> </a>
## Search Bar

You can search for nodes and edges by column value directly through
Cytoscape's tool bar. For example, to select nodes or edges with a
column value that starts with "STE", type `ste*` in the search bar. The
search is case-insensitive. The `*` is a wildcard character that matches
zero or more characters, while "?" matches exactly one character. So
`ste?` would match "STE2" but would not match "STE12". Searching for
`ste*` would match both.

![searchbar2.png](_static/images/Filters/searchbar2.png)

To search a specific column, you can prefix your search term with the
column name followed by a `:`. For example, to select nodes and edges
that have a "COMMON" column value that starts with "STE", use
`common:ste*`. If you don't specify a particular column, all columns
will be searched.

Columns with names that contain spaces, quotes, or characters other than
letters and numbers currently do not work when searching a specific
column. This will be fixed in a future release.

To search for column values that contain special characters you need to
escape those characters using a "\\". For example, to search for
"GO:1232", use the query `GO\:1232`. The complete list of special
characters is:

    + - & | ! ( ) { } [ ] ^ " ~ * ? : \

**Note:** Escaping characters only works when searching all columns. It
currently does not work for column-specific searching. This will be
fixed in a future release.

<a id="filters"> </a>
## Filters

Cytoscape 3 provides a new user interface for filtering nodes and edges.
These tools can be found in the **Select** panel:

![select-panel.png](_static/images/Filters/select-panel.png)

There are two types of filters. On the **Filter** tab are *narrowing*
filters, which can be combined into a tree. On the **Chain** tab are
*chainable* filters, which can be combined in a linear chain.

<a id="narrowing_filters"> </a>
### Narrowing Filters

Narrowing filters are applied to the entire network, and are used to
select a subset of nodes or edges in a network based on user-specified
constraints. For example, you can find edges with a weight between 0 and
5.5, or nodes with degree less than 3. A filter can contain an arbitrary
number of sub-filters.

To add a filter click on the "+" button. To delete a filter (and all its
sub-filters) click the "x" button. To move a filter grab the handle
![filterhandle.png](_static/images/Filters/filterhandle.png)
with the mouse and drag and drop the filter on its intended destination.
Dropping a filter on top of another filter will group the filters into a
composite filter.

<a id="interactive_filter_application_mode"> </a>
#### Interactive Filter Application Mode

Due to the nature of narrowing filters, Cytoscape can apply them to a
network efficiently and interactively. Some filters even provide slider
controls to quickly explore different thresholds. This is the default
behavior on smaller networks. For larger networks, Cytoscape
automatically disables this interactivity. You can override this by
manually checking the **Apply when filter changes** box above the **Apply**
button:

![apply-automatically.png](_static/images/Filters/apply-automatically.png)

Cytoscape comes packaged with the following narrowing filters:

<a id="column_filter"> </a>
#### Column Filter

This filter will match nodes or edges that have particular column
values. For numeric columns sliders are provided to set minimum and
maximum values, or the values may be entered manually.

From string columns, a variety of matching options are provided:

![string-column.png](_static/images/Filters/string-column.png)

For example, column values can be checked to see if they contain or
match exactly the text entered in the text box. More complex matching
criteria can be specified by using a Java-style regular expression.

By default string matching is case insensitive. Case sensitive matching
requires the use of a regular expression that starts with "(?-i)". For
example to match the text "ABC" in a case sensitive way use the
following regular expression: "(?-i)ABC".

Cytoscape uses [Java regular expression
syntax](https://docs.oracle.com/javase/8/docs/api/java/util/regex/Pattern.html).

<a id="degree_filter"> </a>
#### Degree Filter

The degree filter matches nodes with a degree that falls within the
given minimum and maximum values, inclusive. You can choose whether the
filter operates on the in-degree, out-degree or overall (in + out)
degree.

<a id="topology_filter"> </a>
#### Topology Filter

The topology filter matches nodes having a certain number of neighbors
which are within a fixed distance away, and which match a sub-filter.
The thresholds for the neighborhood size and distance can be set
independently, and the sub-filter is applied to each such neighbor node.

The topology filter will successfully match a node if the sub-filter
matches against the required number of neighbor nodes.

<a id="grouping_and_organizing_filters"> </a>
#### Grouping and Organizing Filters

By default, nodes and edges need to satisfy the constraints of all your
filters. You can change this so that instead, only the constraints of at
least one filter needs to be met in order to match a node or edge. This
behavior is controlled by the **Match all/any** drop-down box. This
appears once your filter has more than one sub-filter. For example,
suppose you wanted to match nodes with column *COMMON* containing `ste`
or `cdc`, but you only want nodes with degree 5 or more, you'd first
construct a filter that looks like this:

![sample-group2.png](_static/images/Filters/sample-group2.png)

This filter will match nodes where **COMMON** contains `ste` **and**
`cdc`. To change this to a logical **or** operation, drag either of the
column filters by its handle
![filterhandle.png](_static/images/Filters/filterhandle.png)
onto the other column filter to create a new group. Now change the
group's matching behavior to **Match any**:

![sample-group1.png](_static/images/Filters/sample-group1.png)

You can also reorder filters by dropping them in-between existing
filters.

<a id="chainable_filters"> </a>
### Chainable Filters

Chainable filters are combined in an ordered list. The nodes and edges
in the output of a filter become the input of the next filter in the
chain. The first filter in the chain gets its input from the current
selection or from a filter on the **Filter** tab. The output of the last
filter becomes the new selection.

You can specify the input to the first filter in the chain by selecting
a **Start with**, where **Current selection** refers to the nodes and
edges currently selected. You can also choose a narrowing filter, which
produces a different set of selected nodes and edges.

![ChainFilter1.png](_static/images/Filters/ChainFilter1.png)

Chainable filters can be reordered by dragging one by the handle and
dropping it between existing filters.

Cytoscape currently bundles the following chainable filters:

<a id="edge_interaction_transformer"> </a>
#### Edge Interaction Transformer

This transformer will go through all the input edges and selectively add
their source nodes, target nodes, or both, to the output. This is useful
for adding nodes that are connected to edges that match a particular
filter.

Output options:

-   Add (default): Automatically includes all input nodes and edges in
    the output, and adds source or target nodes from input edges to
    the output.

-   Replace with: Does not automatically include input nodes and edges
    in the output. Only outputs nodes that match the filter.

A sub-filter may be added as well. When a sub-filter is present the
source/target nodes must match the filter to be included in the output.

<a id="node_adjacency_transformer"> </a>
#### Node Adjacency Transformer

This transformer is used to add nodes and edges that are adjacent to the
input nodes. A sub-filter may be specified as well.

Note that pressing the **Apply** button repeatedly may cause the
selection to continuously expand. This allows adjacent nodes that are at
greater distances to be added.

Output options:

-   Add (default): Automatically includes all input nodes and edges in
    the output, and adds selected adjacent nodes and edges.

-   Replace with: Only outputs the adjacent nodes/edges.

Select options:

-   Adjacent nodes: Output nodes that are adjacent to the input nodes.

-   Adjacent edges: Output edges that are adjacent (incident) to the
    input nodes.

-   Adjacent nodes and edges (default): Output both nodes and edges that
    are adjacent to the input nodes.

Edge direction options. (Hidden by default, click the small arrow icon
to reveal.):

-   Incoming: Only include adjacent nodes/edges when the adjacent edge
    is incoming.

-   Outgoing: Only include adjacent nodes/edges when the adjacent edge
    is outgoing.

-   Incoming and Outgoing (default): Ignore the directionality of
    adjacent edges.

Sub-filter options. (Available when a sub-filter has been added.):

-   Adjacent nodes (default): The sub-filter is only applied to
    adjacent nodes. (Edges to the adjacent nodes are still included in
    the output.)

-   Adjacent edges: The sub-filter is only applied to adjacent edges.
    (Nodes connected to the adjacent edges are still included in
    the output.)

-   Adjacent nodes and edges: Both the adjacent edge and its connected
    node must match the filter. Note that for a filter to match an edge
    and a node at the same time it should be a compound filter that is
    set to "Match any (OR)".

<a id="working_with_narrowing_and_chainable_filters"> </a>
### Working with Narrowing and Chainable Filters

The name of active filter appears in the drop-down box at the top of
**Select** panel. Beside this is the options button which will allow you
to rename, remove or export the active filter. It also lets you create a
new filter, or import filters.

![select-panel-options.png](_static/images/Filters/select-panel-options.png)

At the bottom of the **Select** panel, there is an **Apply** button that
will re-apply the active filter. On the opposite side of the progress
bar is the cancel button, which will let you interrupt a long-running
filter.

<a id="diffusion"> </a>
## Diffusion

Cytoscape's Diffusion algorithm attempts to use a set of nodes and an entire interaction network to find the most important nodes.
Conceptually, Diffusion applies heat to each node in the set, and lets the heat flow to through connecting edges to adjacent nodes. 
It then produces a list of nodes ranked by the heat they accumulated. A node with many connections to it will tend to have a higher
ranking, and isolated nodes will tend to be low. In fact, nodes from the original set may become ranked very low if it is isolated but other nodes are closely connected. 

By default, Diffusion uses the set of selected nodes as the heat sources, with each node having the same initial heat. At the end of a Diffusion, Cytoscape leaves the top 90th percentile of hot nodes selected. It allows you to use the Results panel to select a higher or 
lower percentile dynamically. It also stores the node's initial heat as a node attribute in the "diffusion_input" column, and returns the heat and ranking values in the "diffusion_output_heat" and "diffusion_output_rank" columns.

An advanced Diffusion option allows you to specify initial heat values for each node via its "diffusion_input" attribute.

This figure shows the result of selecting the PHO4, GCR1 and ICL1 genes (via the [search bar](Finding_and_Filtering_Nodes_and_Edges.html#search_bar)) and performing a Diffusion by either **Tools → Diffuse → Selected Nodes** or right-click **Diffuse → Selected Nodes**. Diffusion calculated the heat ranking of all 331 nodes in the network, and then selected the top 33. 

![after_diffusion.png](_static/images/Filters/after_diffusion.png)

To select more than 33, move the Node Rank slider in the Diffusion Output Results Panel to the right or enter a number greater than 33 in the Current Rank field. You can also select nodes by a heat value cutoff by using the Range Column to select a heat value column. Finally, you can use the Visual Style chooser and Create button to extract the selected nodes into a new network.

<a id="the_select_menu"> </a>
## The Select Menu

The **Select → Nodes** and **Select → Edges** menus provide several
mechanisms for selecting nodes and edges. Most options are fairly
straightforward; however, some need extra explanation.

**Select → Nodes → From ID List File...** selects nodes based on node
identifiers found in a specified file. The file format is simply one
node id per line:

    Node1
    Node2
    Node3
    ...
