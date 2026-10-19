# Environment records

The main classification/profiling record uses TensorFlow 2.20.0 and tf-keras
2.20.1. B08D uses an NVIDIA L4 and float32. These observations do not imply that
every notebook uses identical dependencies or that every historical run used L4.

`requirements-classification.txt` lists only the recorded TensorFlow packages.
Follow the actual notebook setup cells for additional dependencies. Record the
Python version, package freeze, CUDA/cuDNN build, GPU name, driver, precision, and
determinism settings separately for classification, Pycol, DIME, and profiling.

Do not invent version pins for packages that were not captured. Export sanitized
environment records from completed runs before calling the repository reproducible.
Installing the TensorFlow pair on a different host does not recreate the GPU/driver
environment or guarantee numerically identical predictions.
