Spectrum-X Operator API reference v1alpha2
==========================================

Packages:

- :ref:`spectrumx.nvidia.com/v1alpha2 <spectrumx.nvidia.com/v1alpha2>`

.. _spectrumx.nvidia.com/v1alpha2:

spectrumx.nvidia.com/v1alpha2
-----------------------------

Package v1alpha2 contains API Schema definitions for the spectrumx v1alpha2 API group.

Resource Types:

- :ref:`SpectrumXRailPoolConfig <SpectrumXRailPoolConfig>`

.. _SpectrumXRailPoolConfig:

SpectrumXRailPoolConfig
~~~~~~~~~~~~~~~~~~~~~~~

SpectrumXRailPoolConfig is the Schema for the spectrumxrailpoolconfigs API.

.. list-table::
   :header-rows: 1
   :widths: 30 70

   * - Field
     - Description
   * - ``apiVersion``
     - ``spectrumx.nvidia.com/v1alpha2``
   * - ``kind``
     - ``SpectrumXRailPoolConfig``
   * - ``metadata``
     - Standard object metadata. Refer to the Kubernetes API documentation for the fields of the ``metadata`` field.
   * - | ``spec``
       | :ref:`SpectrumXRailPoolConfigSpec <SpectrumXRailPoolConfigSpec>`
     - Desired state of the SpectrumXRailPoolConfig.
   * - | ``status``
       | :ref:`SpectrumXRailPoolConfigStatus <SpectrumXRailPoolConfigStatus>`
     - Observed state of the SpectrumXRailPoolConfig.

.. _SpectrumXRailPoolConfigSpec:

SpectrumXRailPoolConfigSpec
~~~~~~~~~~~~~~~~~~~~~~~~~~~

(*Appears on:* :ref:`SpectrumXRailPoolConfig <SpectrumXRailPoolConfig>`)

SpectrumXRailPoolConfigSpec defines the desired state of SpectrumXRailPoolConfig.

.. list-table::
   :header-rows: 1
   :widths: 30 70

   * - Field
     - Description
   * - | ``draEnabled``
       | bool
     - *(Optional)* Enables Dynamic Resource Allocation (DRA) for the configured Spectrum-X nodes. Defaults to ``true``.
   * - | ``nodeSelector``
       | map[string]string
     - *(Optional)* Label selector that identifies the Spectrum-X nodes the configuration applies to.
   * - | ``maxUnavailable``
       | `IntOrString <https://godoc.org/k8s.io/apimachinery/pkg/util/intstr#IntOrString>`__
     - *(Optional)* Either an integer count or a percentage of nodes in the pool that can be configured in parallel. Defaults to ``1``.
   * - | ``networkNamespace``
       | string
     - *(Optional)* Namespace of the NetworkAttachmentDefinition custom resource.
   * - | ``numVfs``
       | int
     - Number of VFs to configure on each PF. Must be ``>= 1``.
   * - | ``railTopology``
       | :ref:`[]RailTopology <RailTopology>`
     - Rails topology list. Must contain at least one entry.

.. _SpectrumXRailPoolConfigStatus:

SpectrumXRailPoolConfigStatus
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

(*Appears on:* :ref:`SpectrumXRailPoolConfig <SpectrumXRailPoolConfig>`)

SpectrumXRailPoolConfigStatus defines the observed state of SpectrumXRailPoolConfig.

.. list-table::
   :header-rows: 1
   :widths: 30 70

   * - Field
     - Description
   * - | ``syncStatus``
       | string
     - Synchronization status. One of ``Unknown``, ``InProgress``, ``Failed``, ``Succeeded``.
   * - | ``observedGeneration``
       | int64
     - The most recent generation observed by the controller.

.. _RailTopology:

RailTopology
~~~~~~~~~~~~

(*Appears on:* :ref:`SpectrumXRailPoolConfigSpec <SpectrumXRailPoolConfigSpec>`)

RailTopology describes a single rail in the Spectrum-X pool. Only one of ``cidrPoolRef`` or ``ipam`` may be specified.

.. list-table::
   :header-rows: 1
   :widths: 30 70

   * - Field
     - Description
   * - | ``name``
       | string
     - Rail topology name. Must be non-empty.
   * - | ``nicSelector``
       | :ref:`NicSelector <NicSelector>`
     - PF selector identifying the NICs that belong to this rail.
   * - | ``cidrPoolRef``
       | string
     - *(Optional)* Reference to a CIDR Pool resource. Mutually exclusive with ``ipam``.
   * - | ``ipam``
       | string
     - *(Optional)* Advanced IPAM configuration. Mutually exclusive with ``cidrPoolRef``.
   * - | ``mtu``
       | int
     - MTU for the rail. Must be ``>= 0``.

.. _NicSelector:

NicSelector
~~~~~~~~~~~

(*Appears on:* :ref:`RailTopology <RailTopology>`)

NicSelector selects the physical functions that participate in a rail.

.. list-table::
   :header-rows: 1
   :widths: 30 70

   * - Field
     - Description
   * - | ``pfNames``
       | []string
     - List of PF names. Must contain at least one entry.
