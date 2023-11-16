<!-- spellchecker: ignore copytrade -->
<!-- markdownlint-disable MD033 -->

# AI Toolkit for IBM® Z® and IBM® LinuxONE

# Table of contents

- [Overview](#overview)
- [The Family of AI Frameworks](#ai-toolkit)
- [Versioning Policy and Release Cadence](#versioning)
- [Recommended Deployment Guidelines](#deployment)
- [Technical Support](#contact)
- [Licenses](#licenses)

# Overview <a id="overview"></a>

AI Toolkit for IBM® Z® and IBM® LinuxONE is a family of popular open-source
AI frameworks with IBM Elite Support and adapted for IBM Z and LinuxONE
hardware.

# The Family of AI Frameworks <a id="ai-toolkit"></a>

The following family of AI frameworks are Generally Available as container
images that run in both the Linux, and zCX environments of z/OS on IBM Z:

- [IBM Z Deep Learning Compiler](https://github.com/IBM/zDLC)
- [IBM Z Accelerated for Snap ML](https://github.com/IBM/ibmz-accelerated-for-snapml)
- [IBM Z Accelerated for TensorFlow](https://github.com/IBM/ibmz-accelerated-for-tensorflow)
- [IBM Z Accelerated for NVIDIA Triton™ Inference Server](https://github.com/IBM/ibmz-accelerated-for-nvidia-triton-inference-server)
- [IBM Z Accelerated Serving for TensorFlow](https://github.com/IBM/ibmz-accelerated-serving-for-tensorflow)

The container images for each AI Framework can be downloaded from the
[IBM Container Registry](https://icr.io).

For installation documentation regarding a particular AI framework, please visit
the associated documentation for the individual container images as linked
above.

For general documentation about container image deployment, please visit the
[Recommended Deployment Guidelines](#deployment) section below.

# Versioning Policy and Release Cadence <a id="versioning"></a>

Each AI Framework listed above follows its own versioning policy and release
cadence. For information about a specific AI Framework versioning policy and
release cadence, please visit the respective documentation as linked below:

- [IBM Z Deep Learning Compiler Versioning Policy and Release Cadence](https://github.com/IBM/zDLC#scope-and-versioning)
- [IBM Z Accelerated for Snap ML Versioning Policy and Release Cadence](https://github.com/IBM/ibmz-accelerated-for-snapml#versioning)
- [IBM Z Accelerated for TensorFlow Versioning Policy and Release Cadence](https://github.com/IBM/ibmz-accelerated-for-tensorflow#versioning)
- [IBM Z Accelerated for NVIDIA Triton™ Inference Server Versioning Policy and Release Cadence](https://github.com/IBM/ibmz-accelerated-for-nvidia-triton-inference-server#versioning)
- [IBM Z Accelerated Serving for TensorFlow Versioning Policy and Release Cadence](https://github.com/IBM/ibmz-accelerated-serving-for-tensorflow#versioning)

# Technical Support <a id="contact"></a>

While open-source software has made AI more accessible, affordable and
innovative, you need the right level of support to successfully implement these
frameworks. With the introduction of the AI Toolkit for IBM Z and IBM®
LinuxONE, you can leverage our proven support offering to deploy and accelerate
the adoption of popular open-source AI frameworks on your
[z/OS®](https://www.ibm.com/products/zos) and IBM® LinuxONE platforms.

The AI Toolkit consists of
[IBM Elite Support](https://www.ibm.com/software/passportadvantage/paselectedsupportprograms.html)
and
[IBM Secure Engineering](https://www.redbooks.ibm.com/abstracts/redp4641.html)
that vet and scan open-source AI serving frameworks and IBM certified containers
for security vulnerabilities and validate compliance with industry regulations.

**Information about obtaining support and information about the offerings
supported within the AI Toolkit for IBM Z and IBM LinuxONE can be found
[here](https://www.ibm.com/products/ai-toolkit-for-z-and-linuxone)**. Details on
the level of support is available in the
[What to Expect From Support](support-terms) document.

For information about upcoming IBM news, become a member of the
[AI on IBM Z Community](https://ibm.biz/aionibmz-community).

# Recommended Deployment Guidelines <a id="deployment"></a>

By default, the container images listed in this documentation, and related AI
Toolkit documentation, must not be placed into a production environment without
additional security precautions in effect. While each deployment environment is
uniquely different from the next, as a baseline set of security precautions,
please view our [Recommended Deployment Guidelines](deployment-guidelines)
documentation.

Additional resources for leveraging AI with IBM Z are available
[here](https://ibm.github.io/ai-on-z-101/).

# Licenses/Copyright/Trademarks <a id="licenses"></a>

The registered trademark Linux® is used pursuant to a sublicense from the Linux
Foundation, the exclusive licensee of Linus Torvalds, owner of the mark on a
worldwide basis.

TensorFlow, the TensorFlow logo and any related marks are trademarks of Google
Inc.

NVIDIA and Triton are trademarks and/or registered trademarks of NVIDIA
Corporation in the U.S. and/or other countries.

Docker and the Docker logo are trademarks or registered trademarks of Docker,
Inc. in the United States and/or other countries. Docker, Inc. and other parties
may also have trademark rights in other terms used herein.

IBM, the IBM logo, and ibm.com, IBM z16, IBM z15, IBM z14 are trademarks or
registered trademarks of International Business Machines Corp., registered in
many jurisdictions worldwide. Other product and service names might be
trademarks of IBM or other companies. The current list of IBM trademarks can be
found [here](https://www.ibm.com/legal/copyright-trademark).
