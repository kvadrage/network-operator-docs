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
.. include:: ../../common/vars.rst

******************
Validate Workflow
******************

.. contents:: On this page
   :depth: 3
   :local:
   :backlinks: none

.. note::

   **Use this when:** you've applied a deployment and want to confirm both the control plane (operator and CRs reconciled) and the data plane (pods on different nodes can reach each other on every rail). ``l8k validate`` runs three checks back-to-back and emits an HTML report alongside the text/JSON output.

================================================================================
Run Validation
================================================================================

.. code-block:: bash

   l8k validate

No flags required when defaults apply. ``l8k validate`` reads ``./cluster-config.yaml`` (or, when not in the working directory, ``<deployment-files>/../cluster-config.yaml``) for ``networkOperator.selectedRelease`` and the operator namespace, then:

#. Classifies every YAML manifest under ``--deployment-files`` against the live cluster.
#. Runs the connectivity matrix (enabled by default; pass ``--connectivity=false`` to skip).
#. Writes an HTML validation report to ``<deployment-files>/verify-report.html``.

.. code-block:: bash

   l8k validate --user-config ./cluster-config.yaml \
       --deployment-files ./deployment \
       --kubeconfig ~/.kube/config

================================================================================
What It Checks
================================================================================

Network Operator Helm release version
--------------------------------------

Launch Kit reads any Helm release Secret in the operator namespace whose release name contains ``network-operator`` (Secret name format ``sh.helm.release.v1.<release>.v<N>``, type ``helm.sh/release.v1``), parses the chart's ``appVersion``, and compares it against the version expected for ``networkOperator.selectedRelease`` in the user's config (looked up in the embedded release catalog).

The check is **skipped** (with a clear reason in the output) when:

- ``cluster-config.yaml`` is missing or doesn't declare ``networkOperator.selectedRelease``.
- The selected release is not in the embedded catalog.
- No matching Helm release Secret is found in the operator namespace (e.g., the operator was installed via Argo CD or kubectl rather than Helm).

Manifest state
--------------

Every YAML document under ``--deployment-files`` is routed through a per-Kind validator that classifies it as one of four states:

- ``READY`` — the controller reports the object is fully reconciled. For ``NicClusterPolicy`` and ``NicNodePolicy``, the per-component ``appliedStates[]`` are folded into a "ready: 12/12; components: …" summary so an operator sees exactly what landed.
- ``IN-PROGRESS`` — the controller is still reconciling. For ``SriovNetworkNodePolicy`` and the ``NicConfigurationTemplate`` / ``NicInterfaceNameTemplate`` Kinds, per-node and per-device breakdowns are surfaced (the SR-IOV silent-failure case where ``syncStatus=Succeeded`` but ``pfNames`` matched zero interfaces is detected and reported as ERROR rather than READY).
- ``ERROR`` — the controller reported an error state, or a cross-check (expected PFs vs. discovered) failed.
- ``MISSING`` — the named object is not present in the cluster.

Files matching ``*example*`` (e.g., ``50-example-daemonset.yaml``) are skipped here — those are demo workloads consumed by the connectivity matrix, not part of the network-operator surface ``l8k validate`` checks for static state.

Connectivity matrix (default ON)
--------------------------------

When every manifest is ``READY``, Launch Kit applies the example DaemonSet, waits for it to roll out completely (``numberReady == desiredNumberScheduled > 0`` — a single ``ContainerCreating``-stuck pod fails the wait), enumerates the test pods' rail IPs from each pod's ``k8s.v1.cni.cncf.io/network-status`` annotation, and runs a ping matrix:

- **Same-rail tests** — every ordered ``(srcPod, dstPod)`` pair on every rail both pods attach to. Verifies the rail's end-to-end data path.
- **Cross-rail canary** — one ping per ordered pod pair from rail-0 → rail-1 (when both pods have ≥2 rails). Catches routing misconfigurations that would otherwise be invisible when every rail passes independently.

The DaemonSet is deleted on exit. Pass ``--keep`` to leave it running for follow-up debugging.

When fewer than two schedulable test pods exist (single-node clusters, pods stuck Pending), the matrix soft-skips with a reason in the report.

Preset deviations
-----------------

If ``cluster-config.yaml`` records preset deviations, they're surfaced under each affected node group in the HTML report's **Node groups** section. Each entry shows the field (``pfCount`` / ``pciAddress`` / ``deviceID``), the expected value, the discovered value, and a short detail. Deviations are reported for visibility — they do **not** affect the exit code, since the deployment can still run correctly while diverging from the matched preset. See :doc:`../presets` "Validation and Deviations".

================================================================================
HTML Validation Report
================================================================================

Every ``l8k validate`` run writes a self-contained HTML report to ``<deployment-files>/verify-report.html`` (override with ``--report-path <file>``, disable with ``--report-path=-``). The report is one HTML file with inline CSS — no JavaScript, no external assets — so it can be attached to a ticket, shared in chat, or opened offline.

Report sections:

- **Header** — Launch Kit version, generation timestamp, kubeconfig context, API-server version, operator namespace.
- **Profile** — fabric, deployment type, multirail, and Spectrum-X (version / multiplane mode / number of planes) when enabled. When ``cluster-config.yaml`` doesn't carry an explicit ``profile:`` block, the profile is inferred from the Kinds present in the rendered deployment manifests.
- **Node groups** — one card per ``clusterConfig[]`` entry: identifier, machine type, GPU type, link type, node selector, worker-node list, capability pills, and east-west / north-south PF tables (PCI, device ID, rail, netdev, RDMA device, PSID, part number, NUMA node, connected GPU, GPU proximity). Preset deviations are rendered as a sub-table when present.
- **Cluster nodes** — name, ``nvidia.kubernetes-launch-kit.machine`` and ``.gpu`` labels, role.
- **Network Operator release** — selected vs. deployed appVersion.
- **Manifest state** — one row per CR with a state badge and the validator's reason. Two expandable dropdowns per row: **Details** (the SR-IOV per-node breakdown, the NCP appliedStates components, etc.) and **Live YAML** (the cluster's current view of the object, ``managedFields`` stripped, status kept).
- **Connectivity matrix** — per-rail ``src × dst`` grids with pass/fail/skipped color-coded cells, plus the cross-rail canary list.
- **Warnings** — bulleted rollup ("connectivity matrix skipped because cluster has in-progress manifests", "SriovNetworkNodePolicy/rail-0 is in-progress on 2/3 nodes", etc.).

================================================================================
Flag Reference
================================================================================

.. list-table::
   :header-rows: 1
   :widths: 30 70

   * - **Flag**
     - **Description**
   * - ``--connectivity``
     - Run the data-plane ping matrix. Default ``true``. Pass ``--connectivity=false`` to limit validate to the static manifest + Helm-release-version checks.
   * - ``--connectivity-timeout``
     - Wall-clock budget for the connectivity phase (DaemonSet rollout + ping execs). Default ``5m``.
   * - ``--ping-count``
     - Number of ICMP echoes per ``src → dst`` pair (``ping -c N``). Default ``3``.
   * - ``--keep``
     - Leave the test DaemonSet running after ``--connectivity`` completes. Useful for follow-up kubectl exec / iperf3 sessions.
   * - ``--wait``
     - Block validate up to this duration waiting for in-progress manifests to reach a terminal state. ``0`` (default) returns immediately on the first snapshot. Re-polls the cluster every 10 s.
   * - ``--report-path``
     - Write the HTML report to this path. Empty (default) writes to ``<deployment-files>/verify-report.html``. Pass ``-`` to skip the report entirely.
   * - ``--user-config``
     - Cluster config file. Lookup order: explicit path → ``./cluster-config.yaml`` → ``<deployment-files>/../cluster-config.yaml`` → ``<deployment-files>/cluster-config.yaml``.
   * - ``--deployment-files``
     - Directory containing the manifests to verify (default: ``./deployment``).
   * - ``--kubeconfig``
     - Path to kubeconfig (falls back to ``$KUBECONFIG``).

================================================================================
Exit Codes
================================================================================

- **0** — every manifest is READY (or all-READY plus IN-PROGRESS without errors) and, when run, the connectivity matrix passed.
- **0 with warning** — at least one manifest is IN-PROGRESS but none are ERROR / MISSING. Connectivity is skipped because pinging an unready cluster would produce noise. Re-run validate later, or use ``--wait <duration>`` to block.
- **4** — at least one manifest is MISSING or ERROR, the deployed Helm release version doesn't match the selected release, or the connectivity matrix reported failures.
- Other codes — see :doc:`../reference/cli` "Exit Codes".

================================================================================
Sample Output
================================================================================

.. code-block:: text

   Network Operator release
     selectedRelease: 26.4
     expected version: v26.4.0-beta.9
     deployed: network-operator (chart=26.4.0-beta.9 app=v26.4.0-beta.9 rev=3 status=deployed)
     result: MATCH

   Manifests
     [READY      ] NicClusterPolicy/nic-cluster-policy in (cluster-scoped) — ready — ready: 12/12; components: cni-plugins, ipoib, multus, …
     [READY      ] NicNodePolicy/nicnodepolicy-h100 in (cluster-scoped) — ready
     [READY      ] SriovNetworkNodePolicy/ethernet-sriov-rail-0 in network-operator — 2/2 nodes ready

   Summary: 12/12 ready, 0 in-progress, 0 error, 0 missing; version: match; preset deviations: 0 group(s)

   Connectivity matrix

   Rail sriov-network-rail-0:
     src \ dst          c-220-166-240-241  c-220-166-240-242
     c-220-166-240-241  —                  ✓ 0% 0.1ms
     c-220-166-240-242  ✓ 0% 0.1ms         —

   Rail sriov-network-rail-1:
     src \ dst          c-220-166-240-241  c-220-166-240-242
     c-220-166-240-241  —                  ✓ 0% 0.1ms
     c-220-166-240-242  ✓ 0% 0.3ms         —

   Cross-rail canary:
     c-220-166-240-241 [sriov-network-rail-0]  → c-220-166-240-242 [sriov-network-rail-1]  ✓ 0% 0.1ms
     c-220-166-240-242 [sriov-network-rail-0]  → c-220-166-240-241 [sriov-network-rail-1]  ✓ 0% 0.1ms

   ✓ All 6 ping test(s) passed
   HTML report written to /home/user/deployment/verify-report.html

For programmatic use, ``--output json`` emits a single object on stdout with ``versionCheck``, ``manifests``, ``presetDeviations``, ``summary``, ``connectivity``, and ``reportPath`` fields. Logs go to stderr.

==========
See Also
==========

- :doc:`Deploy Workflow <deploy>` --- the writer side of the contract validate checks (also supports ``--verify`` to chain the matrix straight after a successful deploy)
- :doc:`Troubleshooting <../troubleshooting>` --- diagnose missing or failing manifests and read the HTML report
- :doc:`CLI Reference <../reference/cli>` --- ``l8k validate`` flag reference
