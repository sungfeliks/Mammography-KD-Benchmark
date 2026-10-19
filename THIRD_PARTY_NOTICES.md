# Attribution and third-party materials

The MIT license in this starter is a proposed license for original project code
and its accompanying documentation. Confirm code ownership and upstream
obligations when adding the actual notebooks. This starter does not bundle code
from the external repositories listed below.

## Methodological influences

| Work | Relationship to this study | Source |
| --- | --- | --- |
| Hinton KD | Softened output-distribution distillation, implemented for a binary classifier | https://arxiv.org/abs/1503.02531 |
| Yeh et al. | Informed model-family selection, with modified roles and image-only inputs | https://doi.org/10.3390/bioengineering12060640 |
| SSID-KD | Feature-consistency inspiration, adapted from deraining to supervised classification | https://github.com/cuiyixin555/SSID-KD |
| Pycol | Training-set geometry diagnostics | https://doi.org/10.1016/j.neucom.2025.130311 |
| DIME | Representation-dependent information/difficulty estimates | https://openreview.net/pdf?id=kvqPFy0hbF |

DenseNet121 is a teacher here, although it is a student in Yeh et al.; this study
uses MobileNetV2 rather than their MobileNet model. It does not reproduce their
image–tabular fusion or SSO optimization.

## Code provenance

For each copied or adapted source file, record the upstream URL, exact commit,
file path, license, and modifications in `docs/code_provenance.csv`. Keep required
copyright and license notices with the reused code. A method citation is not a
substitute for a code license. If an upstream reuse license cannot be verified,
do not assume that publicly visible source can be redistributed under MIT.

For newly implemented code based on a published concept, cite the concept and
describe the implementation changes. Do not describe the classification feature
pipeline as an exact reproduction of SSID-KD.

## Datasets, pretrained weights, and manuscripts

Dataset access and redistribution follow the providers' terms, independently of
this repository's code license. The same applies to downloaded pretrained weights.
Neither dataset permission nor the right to redistribute weights follows from the
MIT license on project code. Published PDFs and the thesis/manuscript are outside
this code-license scope unless their rights holders explicitly license them here.
