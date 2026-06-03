.. license-header
  SPDX-FileCopyrightText: Copyright (c) 2024 NVIDIA CORPORATION & AFFILIATES. All rights reserved.
  SPDX-License-Identifier: Apache-2.0

  Licensed under the Apache License, Version 2.0 (the "License");
  you may not use this file except in compliance with the License.
  You may obtain a copy of the License at

  http://www.apache.org/licenses/LICENSE-2.0

  Unless required by applicable law or agreed to in writing, software
  distributed under the License is distributed on an "AS IS" BASIS,
  WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
  See the License for the specific language governing permissions and
  limitations under the License.

.. headings # #, * *, =, -, ^, "

==========================================
Configuration Details
==========================================


Configuration details
^^^^^^^^^^^^^^^^^^^^^

- ``numVFs``: if provided, configure SR-IOV VFs via nvconfig.

  - This is a mandatory parameter.
  - E.g: if ``numVFs=2`` then ``SRIOV_EN=1`` and ``SRIOV_NUM_OF_VFS=2``.
  - If ``numVFs=0`` then ``SRIOV_EN=0`` and ``SRIOV_NUM_OF_VFS=0``.

- ``linkType``: if provided configure ``linkType`` for the NIC for all NIC ports.

  - This is a mandatory parameter.
  - E.g ``linkType = Infiniband`` then set ``LINK_TYPE_P1=IB`` and ``LINK_TYPE_P2=IB`` if second PCI function is present

- ``pciPerformanceOptimized``: performs PCI performance optimizations. If enabled then by default the following will happen:

  - Set nvconfig ``MAX_ACC_OUT_READ`` nvconfig parameter to ``0`` (use device defaults)
  - Set PCI max read request size for each PF to ``4096`` (note: this is a runtime config and is not persistent)
  - Users can override values via ``maxAccOutRead`` and ``maxReadRequest``
  - **IMPORTANT** :

    - According to the PRM, setting ``MAX_ACC_OUT_READ`` to zero enables the auto mode, which applies the best suitable optimizations. However, there is a bug in certain FW versions, where the zero value is not available.
    - In this case, until the fix is available, ``MAX_ACC_OUT_READ`` will not be set and a warning event will be emitted for this device’s CR.

- ``roceOptimized``: performs RoCE related optimizations. If enabled performs the following by default:

  - Nvconfig set for both ports (can be applied from PF0)

    - Conditionally applied for second port if present

      - ``ROCE_CC_PRIO_MASK_P1=255``, ``ROCE_CC_PRIO_MASK_P2=255``
      - ``CNP_DSCP_P1=4``, ``CNP_DSCP_P2=4``
      - ``CNP_802P_PRIO_P1=6``, ``CNP_802P_PRIO_P2=6``

  - Configure pfc (Priority Flow Control) for priority 3, set trust to dscp on each PF, set ToS (Type of Service) to 0.

    - Non-persistent (need to be applied after each boot)
    - Users can override values via ``trust``, ``pfc`` and ``tos`` parameters

  - Can only be enabled with ``linkType=Ethernet``

- ``gpuDirectOptimized``: performs gpu direct optimizations. ATM only optimizations for Baremetal environment are supported. If enabled perform the following:

  - Set nvconfig ``ATS_ENABLED=0``
  - Can only be enabled when ``pciPerformanceOptimized`` is enabled
  - Both the numeric values and their string aliases, supported by NVConfig, are allowed (e.g. ``REAL_TIME_CLOCK_ENABLE=False``, ``REAL_TIME_CLOCK_ENABLE=0``).
  - For per port parameters (suffix ``_P1``, ``_P2``) parameters with ``_P2`` suffix are ignored if the device is single port.

- ``spectrumXOptimized``: enables Spectrum-X specific NIC optimizations. When enabled:

  - Requires ``linkType=Ethernet`` and ``numVfs=1``
  - Cannot be combined with ``roceOptimized`` (RoCE settings are included automatically)
  - Can be combined with ``rawNvConfig`` — raw params are merged as overrides on top of Spectrum-X calculated params
  - Only supported on ConnectX-7 NIC (``nicType: 1021``), ConnectX-8 SuperNIC (``nicType: 1023``) and BlueField-3 SuperNIC (``nicType: a2dc``)
  - ``version``: Required. Reference Architecture version (``RA1.3``, ``RA2.0``, ``RA2.1``, or ``RA2.2``)
  - ``overlay``: Optional, default ``none``. Set to ``l3`` for L3 EVPN overlay
  - ``multiplaneMode``: Optional, default ``none``. Available with RA2.1 and RA2.2. Options: ``none``, ``swplb``, ``hwplb`` (tech preview), ``uniplane``
  - ``numberOfPlanes``: Optional, default ``1``. Available with RA2.1 and RA2.2. Options: ``1``, ``2``, or ``4``

- If a configuration is not set in spec, its non-volatile configuration parameters (if any) should be set to device default.

Spectrum-X Configuration
^^^^^^^^^^^^^^^^^^^^^^^^

The NIC Configuration Operator supports Spectrum-X-specific NIC configuration for different versions of the NVIDIA Spectrum-X Reference Architecture (RA1.3, RA2.0, RA2.1, and RA2.2).

Supported NIC types for Spectrum-X: \* ConnectX-7 NIC (device ID ``1021``) – supports ``none`` only \* ConnectX-8 SuperNIC (device ID ``1023``) – supports all multiplane modes \* BlueField-3 SuperNIC (device ID ``a2dc``) – supports ``none`` only

RA2.1 and RA2.2 introduce multiplane mode support, allowing NICs to be configured with multiple data planes. Available modes:

+--------------+----------------------------------------------+-----------------------------------------------------------+--------+
| Mode         | Description                                  | Supported NICs                                            | Planes |
+==============+==============================================+===========================================================+========+
| ``none``     | Single plane (default)                       | ConnectX-7 NIC, BlueField-3 SuperNIC, ConnectX-8 SuperNIC | 1      |
+--------------+----------------------------------------------+-----------------------------------------------------------+--------+
| ``swplb``    | Software Plane Load Balancing                | ConnectX-8 SuperNIC                                       | 2, 4   |
+--------------+----------------------------------------------+-----------------------------------------------------------+--------+
| ``hwplb``    | Hardware Plane Load Balancing (tech preview) | ConnectX-8 SuperNIC                                       | 2, 4   |
+--------------+----------------------------------------------+-----------------------------------------------------------+--------+
| ``uniplane`` | Uniplane mode                                | ConnectX-8 SuperNIC                                       | 2      |
+--------------+----------------------------------------------+-----------------------------------------------------------+--------+

..

   **Note:** Multiplane modes are available with RA2.1 and RA2.2. For RA1.3 and RA2.0, ``multiplaneMode`` must be ``none`` and ``numberOfPlanes`` must be ``1``.

Example: Single Plane on BlueField-3 SuperNIC
''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''

.. code:: yaml

   apiVersion: configuration.net.nvidia.com/v1alpha1
   kind: NicConfigurationTemplate
   metadata:
     name: spectrum-x-none-configuration
     namespace: nvidia-network-operator
   spec:
     nodeSelector:
         feature.node.kubernetes.io/network-sriov.capable: "true"
     nicSelector:
         nicType: "a2dc" # BlueField-3 SuperNIC
         # partNumbers:
         #   - "900-9D3B6-00CV-AA0"
     template:
         numVfs: 1
         linkType: Ethernet
         spectrumXOptimized:
             enabled: true
             version: "RA2.2"
             overlay: "none"
             multiplaneMode: "none" # Single plane, BlueField-3 SuperNIC
             numberOfPlanes: 1

Example: Software Multiplane (``swplb``) on ConnectX-8 SuperNIC
''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''

.. code:: yaml

   apiVersion: configuration.net.nvidia.com/v1alpha1
   kind: NicConfigurationTemplate
   metadata:
     name: spectrum-x-swplb-configuration
     namespace: nvidia-network-operator
   spec:
     nodeSelector:
         feature.node.kubernetes.io/network-sriov.capable: "true"
     nicSelector:
         nicType: "1023" # ConnectX-8 SuperNIC
         # partNumbers:
         #   - "900-9X81E-00EX-DT0"
     template:
         numVfs: 1
         linkType: Ethernet
         spectrumXOptimized:
             enabled: true
             version: "RA2.2"
             overlay: "none"
             multiplaneMode: "swplb" # Software Plane Load Balancing, ConnectX-8 SuperNIC
             numberOfPlanes: 2

Example: Hardware Multiplane (``hwplb``, tech preview) on ConnectX-8 SuperNIC
''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''

.. code:: yaml

   apiVersion: configuration.net.nvidia.com/v1alpha1
   kind: NicConfigurationTemplate
   metadata:
     name: spectrum-x-hwplb-configuration
     namespace: nvidia-network-operator
   spec:
     nodeSelector:
         feature.node.kubernetes.io/network-sriov.capable: "true"
     nicSelector:
         nicType: "1023" # ConnectX-8 SuperNIC (hwplb supported on ConnectX-8 SuperNIC only)
         # partNumbers:
         #   - "900-9X81E-00EX-DT0"
     template:
         numVfs: 1
         linkType: Ethernet
         spectrumXOptimized:
             enabled: true
             version: "RA2.2"
             overlay: "none"
             multiplaneMode: "hwplb" # Hardware Plane Load Balancing (tech preview), ConnectX-8 SuperNIC only
             numberOfPlanes: 4
