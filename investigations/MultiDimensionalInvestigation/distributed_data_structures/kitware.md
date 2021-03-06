# Kitware's distribted data sets

Steve has pointed us to:
* [Composite Datasets](https://www.paraview.org/Wiki/VTK/Tutorials/Composite_Datasets)
* [vtkCompositeDataSet](https://www.vtk.org/doc/nightly/html/classvtkCompositeDataSet.html)
* [vtkMultiBlockDataSet](https://www.vtk.org/doc/nightly/html/classvtkMultiBlockDataSet.html)

for potential distributed (and sparse, hierarchical) data sets. However looking into these elements, they don't seem to
have a direct connection with distributed data sets, but they can be rather used to build sparse, hierarchical data sets.


## vtkDataObject family
The composite data set can be comprised of
* vtkHierarchicalBoxDataSet
* vtkMutliBlockDataSet
* vtkMultiPieceDataSet

![VTK](vtk_data_set_family.png)

### vtkMultiBlockDataSet
vtkMultiBlockDataSet is a dataset comprised of blocks. Each block can be a non-composite vtkDataObject subclass (or a leaf) or an instance of vtkMultiBlockDataSet itself.
**This makes is possible to build full trees**. vtkHierarchicalBoxDataSet is used for AMR datasets which comprises of refinement levels and uniform grid datasets at each refinement level.

### vtkMultiPieceDataSet

Here the elements cannot be a block themselves.


## To investigate

### vtkDistributedDataFilter

From the documentation:
> This filter redistributes data among processors in a parallel application into spatially contiguous vtkUnstructuredGrids. The execution model anticipated is that all processes read in part of a large vtkDataSet. Each process sets the input of filter to be that DataSet. When executed, this filter builds in parallel a k-d tree, decomposing the space occupied by the distributed DataSet into spatial regions. It assigns each spatial region to a processor. The data is then redistributed and the output is a single vtkUnstructuredGrid containing the cells in the process' assigned regions.

The work of the *vtkDistributedDataFilter* is actually done in the *vtkPkdTree* which
builds the tree in the *BuildLocator* method. The actual implementation is quite
involved and makes use of a global index for each cell. The important path for
the construction of the Parallel kd-tree is:
* *vtkDistributedDataFilter::RequestDataInternal*
* -> *vtkDistributedDataFilter::PartitionDataAndAssignToProcesses*
* -> *vtkPkdTree::BuildLocator*
* -> *vtkPkdTree::MultiProcessBuildLocator*
* -> *vtkPkdTree::BreadthFirstDivide* (this performs the redistribution of cells)

An important insight is that the tree will be rebuilt whenever something changes on
any of the nodes. See [here](https://github.com/Kitware/VTK/blob/ab1d17f2a14dced8557b2dbd5b822f7f03db4716/Filters/Parallel/vtkPKdTree.cxx#L395:L403)

The underlying approach is motivated by this paper [here](https://ntrs.nasa.gov/archive/nasa/casi.ntrs.nasa.gov/19860010476.pdf) which
desribes Recursive Coordinate Bisection (RCB). The approach is described on a high level in `ParallelVolRen.pdf`. The RCB method that has been implemented here has some particular features:
* The kd-split of a region takes place perpendicular to the longest dimension, in order
  to avoid elongated volumes
* Median finding makes use of the [Floyd and Rivest algorithm](https://en.wikipedia.org/wiki/Floyd%E2%80%93Rivest_algorithm) which is related to quickselect. It also checks for duplicates to avoid the worst-case complexity.
