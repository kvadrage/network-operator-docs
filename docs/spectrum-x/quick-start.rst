.. license-header
  SPDX-FileCopyrightText: Copyright (c) 2025 NVIDIA CORPORATION & AFFILIATES. All rights reserved.
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

**********************
Spectrum-X Quick Start
**********************

.. note::

   You can automate the configuration of these examples with NVIDIA Kubernetes Launch Kit.
   For more details, see :doc:`Configuration Assistance with Kubernetes Launch Kit <../k8s-launch-kit/k8s-launch-kit>`.

Three end-to-end examples are provided, covering the main Spectrum-X RA2.2 topologies. Each example walks through the same sequence of resources — Helm install, ``NicClusterPolicy``, ``NicInterfaceNameTemplate``, ``NicConfigurationTemplate``, ``CIDRPool``, ``SpectrumXRailPoolConfig``, and a test pod — adapted to the chosen multiplane mode and NIC family. Pick the example that matches your hardware and topology:

.. list-table::
   :header-rows: 1
   :widths: 25 15 15 45

   * - Example
     - Multiplane mode
     - Supported NICs
     - When to use
   * - :doc:`Single Plane <quick-start-single-plane>`
     - ``none``
     - BF3 SuperNIC, ConnectX-8, ConnectX-9
     - Simplest setup: one PF per rail, one ``CIDRPool`` per rail, one network per rail. Best starting point for BlueField-3 SuperNIC deployments.
   * - :doc:`Multiplane with Software Load Balancing <quick-start-swplb>`
     - ``swplb``
     - BF3 SuperNIC, ConnectX-8, ConnectX-9
     - Software Packet Load Balancing: each rail is split into multiple planes, with a separate ``CIDRPool`` and a separate ``railTopology`` entry per (rail, plane) combination.
   * - :doc:`Multiplane with Hardware Load Balancing <quick-start-hwplb>`
     - ``hwplb``
     - ConnectX-8, ConnectX-9
     - Hardware Packet Load Balancing: NIC LAG handles the per-plane fan-out, so each rail still uses a single ``CIDRPool`` while exposing multiple per-plane PFs. Highest performance; ConnectX-8 / ConnectX-9 only.

.. toctree::
   :maxdepth: 1
   :titlesonly:
   :hidden:

   Single Plane <quick-start-single-plane.rst>
   Multiplane with Software Load Balancing <quick-start-swplb.rst>
   Multiplane with Hardware Load Balancing <quick-start-hwplb.rst>
