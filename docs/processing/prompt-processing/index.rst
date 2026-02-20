.. _prompt-processing:

#################
Prompt Processing
#################

Prompt Processing is the `pipeline execution framework <https://github.com/lsst-dm/prompt_processing>`_ which enables Rubin to process images and deliver alerts to community brokers with low latency.
Prompt Processing is implemented using the `KEDA <https://keda.sh/>`_ framework in Kubernetes.
It runs in the US Data Facility (USDF).

Prompt Processing analyzes each detector in the camera focal plane independently.
Processing begins when the system receives a ``nextVisit`` message from the telescope which specifies the identifier and expected pointing of the forthcoming observation.
In response, Prompt Processing starts a new pod for each detector it will need to process.
It also preloads reference catalogs, template images, catalog history, predicted positions of solar system objects, and other data so that processing is ready to begin.

Once the pixel data arrive, each Prompt Processing pod executes the subsequent steps of the Alert Production pipeline.
After the APDB is updated and alerts are transmitted, the pod executes any afterburner processing and transfers the data products to the central data butler.