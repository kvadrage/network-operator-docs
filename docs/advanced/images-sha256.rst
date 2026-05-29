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
.. include:: ../common/vars.rst

.. _container_images_digest:

****************************************
NVIDIA Network Operator Container Images
****************************************



.. list-table::
   :header-rows: 1

   * - Repository
     - Image Name
     - Tag
     - Digest
   * - nvcr.io/nvstaging/mellanox
     - network-operator
     - v26.4.0-rc.1
     - sha256:f268a0b89a87744a276d3ef07254d484777d610cb3d9f0b2a3cf06f80006c164
   * - nvcr.io/nvstaging/mellanox
     - network-operator-init-container
     - network-operator-v26.4.0-rc.1
     - sha256:3ebf9ef2896e34ba0e77b921c33b49c69e620c96cc4d5ebf59a87e84e270e0fa
   * - nvcr.io/nvstaging/mellanox
     - k8s-rdma-shared-dev-plugin
     - network-operator-v26.4.0-rc.1
     - sha256:8aa81beb52d3b4ed5db99fe30bbe20d9dd941eaf39ea9ef9294f9a95cc75e862
   * - nvcr.io/nvstaging/mellanox
     - ib-kubernetes
     - network-operator-v26.4.0-rc.1
     - sha256:731d8b648697baa61fab42be947acf6b7e753b9dae39dcb93a52fd65f38ee1b2
   * - nvcr.io/nvstaging/mellanox
     - ipoib-cni
     - network-operator-v26.4.0-rc.1
     - sha256:8112083a8fd945d35770cd621697910ed57357347c58565665f601c552af334b
   * - nvcr.io/nvstaging/mellanox
     - nvidia-k8s-ipam
     - network-operator-v26.4.0-rc.1
     - sha256:967156da6cbcc343a88f6e394ec6138b700222195d87ebdaf3a5d9d34c4f2266
   * - nvcr.io/nvstaging/mellanox
     - nic-feature-discovery
     - network-operator-v26.4.0-rc.1
     - sha256:b4682d9f6c4b2142fffaed11a1c506ee1f3ec70cbf8fbcb090fa35f22f28938a
   * - nvcr.io/nvidia/doca
     - doca_telemetry
     - 1.24.3-doca3.3.0-host
     - sha256:d561707f58be0470121c8dbe31b2576caffd92c6a6533a5ddaf37e3f6d1479d6
   * - nvcr.io/nvstaging/mellanox
     - sriov-network-operator
     - network-operator-v26.4.0-rc.1
     - sha256:2688eac22147aab3d0ddc8cb8c707cf4a6f948145578ecb6b8ddb8881bced34c
   * - nvcr.io/nvstaging/mellanox
     - sriov-network-operator-webhook
     - network-operator-v26.4.0-rc.1
     - sha256:0c9fee4392eba964146c761dc09ed4f8f51886878973cab587415ded72564452
   * - nvcr.io/nvstaging/mellanox
     - sriov-network-operator-config-daemon
     - network-operator-v26.4.0-rc.1
     - sha256:2a80ac715352aa19ba34463ba6af3707b76e5a81f927879355e6a5768a27ca3d
   * - nvcr.io/nvstaging/mellanox
     - sriov-network-device-plugin
     - network-operator-v26.4.0-rc.1
     - sha256:eb6b823269c21cb4e4c73d58d57863190518377c5b0c4cd6842314404d5fa04d
   * - nvcr.io/nvstaging/mellanox
     - sriov-cni
     - network-operator-v26.4.0-rc.1
     - sha256:45714725d41e9affd7f236dd9e6f5060f774247b17509336560c7bfc4609e978
   * - nvcr.io/nvstaging/mellanox
     - ib-sriov-cni
     - network-operator-v26.4.0-rc.1
     - sha256:12d5d09c98b278ebaafc820de6c2a6a02c8f403e8a9098800d320e34a42cd68c
   * - nvcr.io/nvstaging/mellanox
     - dra-driver-sriov
     - network-operator-v26.4.0-rc.1
     - sha256:4f2ab6244892cedb53d390e8969fa0ad382e487cbe00a57256cf0612d8d95d17
   * - nvcr.io/nvstaging/mellanox
     - plugins
     - network-operator-v26.4.0-rc.1
     - sha256:52bcf594c9d7d06afb8707e4292f20f34fada786f88a855d507642ff958fcf17
   * - nvcr.io/nvstaging/mellanox
     - multus-cni
     - network-operator-v26.4.0-rc.1
     - sha256:91bd4af3ba88f0d05e20674328227e8737fbbcde76485e4f4106483ea98f98c5
   * - nvcr.io/nvstaging/mellanox
     - ovs-cni-plugin
     - network-operator-v26.4.0-rc.1
     - sha256:2c2177f4acb7add194d2d0ebd4ecf246e02dd12bca31a6370e39ae3546c601b4
   * - nvcr.io/nvstaging/mellanox
     - rdma-cni
     - network-operator-v26.4.0-rc.1
     - sha256:39b2df8f56fcff6e8b7c74b1c8bf03eeb42c9da9a6e1e596293c168d51208e5a
   * - nvcr.io/nvstaging/mellanox
     - nic-configuration-operator
     - network-operator-v26.4.0-rc.1
     - sha256:1a3ef30cf1af22f17cedc73f71ad0179c595dfd2e839cfc0020b8ddf0bd1290a
   * - nvcr.io/nvstaging/mellanox
     - nic-configuration-operator-daemon
     - network-operator-v26.4.0-rc.1
     - sha256:238359a1b483c15e5f157570923db63a10d9fbd4182775e2b374751805e91c4c
   * - nvcr.io/nvstaging/mellanox
     - maintenance-operator
     - network-operator-v26.4.0-rc.1
     - sha256:a584b4374a226d1bec4cc6113847a278f9e0b0219244c67705989d4a0813f903
   * - nvcr.io/nvstaging/mellanox
     - spectrum-x-operator
     - network-operator-v26.4.0-rc.1
     - sha256:add7cc1ef0caf481b7d8dad9ce4efd7c913d202e90d528de7f0d62b84699c65a

=================================
DOCA-OFED Driver Container Images
=================================


.. list-table::
   :header-rows: 1

   * - Repository
     - Image Name
     - Version
   * - nvcr.io/nvstaging/mellanox
     - doca-driver
     - doca3.4.0-26.04-0.8.5.0-0


The followings tags are available for the above DOCA-OFED Driver container version:

------
Ubuntu
------

.. list-table::
   :header-rows: 1

   * - Tags
     - Digest
   * -
       | doca3.4.0-26.04-0.8.5.0-0-ubuntu22.04-amd64
     - sha256:a1b31c4e70ed1438da6eed0eaea82d2ca889b010a5992970bd51a4480ed0800e
   * -
       | doca3.4.0-26.04-0.8.5.0-0-ubuntu22.04-arm64
     - sha256:cadb97231be68ca42068750e9124806e6ea41212cf1417ad36e0d2d2691fd88e
   * -
       | doca3.4.0-26.04-0.8.5.0-0-ubuntu24.04-amd64
     - sha256:f96510eeb909be9c6ebdacf857a6314ac50099f1123921706c5fc25815971ad5
   * -
       | doca3.4.0-26.04-0.8.5.0-0-ubuntu24.04-arm64
     - sha256:99114f63136c70204320f489371268fcda52c16a0b50414304c6da1edc07f4a1

-----
RHCOS
-----

.. list-table::
   :header-rows: 1

   * - Tags
     - Digest
   * -
       | doca3.4.0-26.04-0.8.5.0-0-rhcos4.16-amd64
       | doca3.4.0-26.04-0.8.5.0-0-rhcos4.17-amd64
       | doca3.4.0-26.04-0.8.5.0-0-rhcos4.18-amd64
       | doca3.4.0-26.04-0.8.5.0-0-rhcos4.19-amd64
     - sha256:54fcca51eb34b4e4bd4c64704fb2cc3e5c16e34a7e6b02fd0a20f73ed845623b
   * -
       | doca3.4.0-26.04-0.8.5.0-0-rhcos4.16-arm64
       | doca3.4.0-26.04-0.8.5.0-0-rhcos4.17-arm64
       | doca3.4.0-26.04-0.8.5.0-0-rhcos4.18-arm64
       | doca3.4.0-26.04-0.8.5.0-0-rhcos4.19-arm64
     - sha256:7b05d475015dafd1dfceeee976360c7ff0dc6ab5d1f493fd54ab484053c46cb5

----
RHEL
----

.. list-table::
   :header-rows: 1

   * - Tags
     - Digest
   * -
       | doca3.4.0-26.04-0.8.5.0-0-rhel8.10-amd64
       | doca3.4.0-26.04-0.8.5.0-0-rhel8.6-amd64
       | doca3.4.0-26.04-0.8.5.0-0-rhel8.8-amd64
       | doca3.4.0-26.04-0.8.5.0-0-rhel8.9-amd64
     - sha256:2913d06ce2195a40ef608e72212d6481508a6f88172d03a15a5d580d9e080c12
   * -
       | doca3.4.0-26.04-0.8.5.0-0-rhel8.10-arm64
       | doca3.4.0-26.04-0.8.5.0-0-rhel8.6-arm64
       | doca3.4.0-26.04-0.8.5.0-0-rhel8.8-arm64
       | doca3.4.0-26.04-0.8.5.0-0-rhel8.9-arm64
     - sha256:ce4aa660beb7539e6109e4f6e493c131ddeee9e3206ab3684fbb53c70ca81761
   * -
       | doca3.4.0-26.04-0.8.5.0-0-rhel9.0-amd64
       | doca3.4.0-26.04-0.8.5.0-0-rhel9.2-amd64
       | doca3.4.0-26.04-0.8.5.0-0-rhel9.3-amd64
       | doca3.4.0-26.04-0.8.5.0-0-rhel9.4-amd64
       | doca3.4.0-26.04-0.8.5.0-0-rhel9.5-amd64
       | doca3.4.0-26.04-0.8.5.0-0-rhel9.6-amd64
     - sha256:54cc10234b2e59da833d6582249250039769ef84af3e824953a9639bd6ac29ff
   * -
       | doca3.4.0-26.04-0.8.5.0-0-rhel9.0-arm64
       | doca3.4.0-26.04-0.8.5.0-0-rhel9.2-arm64
       | doca3.4.0-26.04-0.8.5.0-0-rhel9.3-arm64
       | doca3.4.0-26.04-0.8.5.0-0-rhel9.4-arm64
       | doca3.4.0-26.04-0.8.5.0-0-rhel9.5-arm64
       | doca3.4.0-26.04-0.8.5.0-0-rhel9.6-arm64
     - sha256:47523b99e9dc5a51a3e75084610c4a414bc87c85803f423400ff82ba51f6620c

----
SLES
----

.. list-table::
   :header-rows: 1

   * - Tags
     - Digest
   * -
       | doca3.4.0-26.04-0.8.5.0-0-sles15.7-amd64
     - sha256:c6d5c06127af58fa5533bd465597e154b6f1c844c6ade5641553e156eecf9ea3
   * -
       | doca3.4.0-26.04-0.8.5.0-0-sles15.7-arm64
     - sha256:a1b6afd2a737898d147ec8758ae7d177c940a20ca20cedb50f2a72213a89b3d0


=====================================================
STIG FIPS Compliant DOCA-OFED Driver Container Images
=====================================================

.. list-table::
   :header-rows: 1

   * - Repository
     - Image Name
     - Version
   * - nvcr.io/nvstaging/mellanox
     - doca-driver-stig-fips
     - doca3.4.0-26.04-0.8.4.0-0

The followings tags are available for the above STIG FIPS Compliant DOCA-OFED Driver container version:

------
Ubuntu
------

.. list-table::
   :header-rows: 1

   * - Tags
     - Digest
   * -
       | doca3.4.0-26.04-0.8.4.0-0-ubuntu24.04-amd64
     - sha256:d8dc20fc7ef0950c5f4a49296a1b4bea540c9b14cb1f8f65d399dab4d48c91ff

----
RHEL
----

.. list-table::
   :header-rows: 1

   * - Tags
     - Digest
   * -
       | doca3.4.0-26.04-0.8.4.0-0-rhel9.6-amd64
     - sha256:298f7ecefad63a187d9f5555c3e566ba141604a01a565cd05ae3d405e506c5a0