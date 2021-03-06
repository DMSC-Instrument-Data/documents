# MD data loading and saving

To understand loading and saving of multi-dimensional workspaces it is necessary
to expand a bit about some helper structures and concepts such as *MDBoxFlatTree*.


### Supporting concepts

##### MDBoxFlatTree

This structure can accept an *MDEventWorkspace* and flatten it such that it can
be stored on file. It essentially stores information about the box types,
the first and last child, the file position for where events of a box are to
be written.

When saving data, three important methods are called on the *MDBoxFlatTree*:
* **void MDBoxFlatTree::initFlatStructure**:
  This creates a flattened version of the box structure and stores it in several
  vectors. These vectors are for the:
  * box type
  * depth
  * box event index. This creates two entries per box. If the box is a leaf node
    then file position and the number of events in the box is stored.
  * signal and error
  * inverse volume
  * extents


* **void MDBoxFlatTree::setBoxesFilePositions**:
  This informs leaf nodes, i.e. *MDBox* objects what the file position is for
  the events in the box.

* **void MDBoxFlatTree::saveBoxStructure**:
  This essentially stores the box structure, i.e. it saves out the vectors which
  were set up in *initFlatStructure*. Note that this does not save out events.
  Events and the box structure are, sensibly, stored separately in the
  sub-folders `box_structure` and `event_data`, respectively.


When loading data two methods are of particular importance:
* *void MDBoxFlatTree::loadBoxStructure*:
  This loads the serialized box controller and populates the vectors which
  were discussed above.
* *uint64_t MDBoxFlatTree::restoreBoxTree*:
  After having loaded the raw data into vectors, the boxes are restored. The
  essential bit is setting the child boxes on the correct parents, which is
  governed by the `m_BoxEventIndex` vector.

### SaveMD

Most of the work is performed by the *MDBoxFlatTree*, which generates the flattened
box structure and then saves out the box structure itself. The events are stored
via the `MDBox::saveAt` method, which saves the events to disk using the
`BoxControllerNeXusIO`.

### LoadMD

There is not much more to loading the multi-dimensional data. It is essentially,
*SaveMD* in reverse. Again, all the heavy lifting is performed by *MDBoxFlatTree*.
Note that not all events are loaded if the algorithm is operated in the file-backed
mode. The number of events which are loaded can be set by the user.
