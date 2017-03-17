# Design of the GMT Python bindings


## Goals

* Provide access to the GMT C API from Python
* Use GMT modules for plotting and processing through Python
* Use native Python data types (numpy arrays, strings, pandas Dataframes, etc)
  for input and output
* Integration with the Jupyter notebook through display hooks


## API

Each GMT module has a function in the `gmt` package.
Command-line arguments are passes as function keyword arguments.
Data can be passed as file names or in-memory data.
Postscript generating functions return a `Postscript` object that can be
incremented (`+=`) to create the layer cake.
Calling `.show()` on converts the postscript to PNG and embeds it in the
Jupyter notebook.

Example usage:

    import numpy as np
    import gmt

    data = np.loadtxt('data_file.csv')

    cpt = gmt.makecpt(C="red,green,blue", T="0,70,300,10000")
    fig = gmt.pscoast(R='g', J='N180/10i', G='bisque', S='azure1', B='af', X='c', K=True)
    fig += gmt.psxy(input=data, R=True, J=True, O=True, S='ci', C=cpt, h='i1', i='2,1,3,4+s0.02')
    fig.show()



## Package organization

General layout of the Python package:


    gmt/
        core/  # Package with low-level wrappers for the C API

        modules/  # Defines the functions corresponding to GMT modules
