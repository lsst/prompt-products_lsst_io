.. _single-frame:

#######################
Single-frame processing
#######################

Single-frame processing reduces and calibrates the newly-obtained science (direct) image in preparation for image differencing.
It includes `instrument signature removal <https://pipelines.lsst.io/v/weekly/modules/lsst.ip.isr/index.html>`_ (ISR) and `astrometric and photomeric calibration <https://pipelines.lsst.io/v/weekly/modules/lsst.pipe.tasks/tasks/lsst.pipe.tasks.calibrateImage.CalibrateImageTask.html>`_.