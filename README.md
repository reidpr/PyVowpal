A python wrapper for the Vowpal Wabbit machine learning program. More on Vowpal Wabbit at <https://github.com/JohnLangford/vowpal_wabbit/wiki>

Authored by Shilad Sen and Reid Priedhorsky <reidpr@lanl.gov>.

Distributed under the Apache Software Foundation License, version 2: <http://www.apache.org/licenses/LICENSE-2.0>


Dependencies
============

- Python 2.x, version >= 2.7
- The `vw` executable (from the main VW website), version >= 6.1

Older versions may work, but are untested.


Basic usage
===========

This library lets you stream training and test data into Vowpal Wabbit and then stream predictions out. These streams can be either pipes from Python (which the library translates to and from the VW text format appropriately) or files already in the VW format.

At no time do you have to keep the entire input or output datasets in memory in Python (though there's nothing stopping you if you want to). However, you may need lots of disk space to hold temporary files.


Example
=======

This glosses over important details, but the basic idea for streaming directly into and out of VW (i.e., no preformatted files) is this:

    import vowpal2
    
    training_stream = vowpal2.InputStream()
    test_stream = vowpal2.InputStream()
    pred_stream = vowpal2.OutputStream()

    vw = vowpal2.Model(training_stream, test_stream, pred_stream)

    for ex in my_training_examples:
       training_stream.put(ex)

    for ex in my_test_examples:
       test_stream.put(ex)
       print "prediction for %s is %s" % (ex, pred_streams.get())

The last stanza could have been written as follows. If you are streaming the test data and predictions, this is better as it avoid potential deadlock bugs should the pipes fill.

    for ex in my_test_examples:
       print "prediction for %s is %s" % (ex, vw.predict(ex))


More info
=========

The code is well documented, so check the help strings for classes and methods.
