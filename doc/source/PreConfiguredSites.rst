.. _Preconfigured_Sites:

Pre-configured sites
*************************

Pre-configured sites are split into two categories: Tier 1 with officially supported spack-stack installations (see :numref:`Section %s <Preconfigured_Sites_Tier1>`), and Tier 2 (sites with configuration files that were tested or contributed by others in the past, but that are not officially supported by the spack-stack team; see :numref:`Section %s <Preconfigured_Sites_Tier2>`).

Directories ``configs/sites/tier1`` and ``configs/sites/tier2`` contain site configurations for several HPC systems, as well as minimal configurations for macOS and Linux. The macOS and Linux configurations are **not** meant to be used as is, as user setups and package versions vary considerably. Instructions for adding this information can be found in :numref:`Section %s <NewSiteConfigs>`.

As of spack-stack-1.8.0, this page provides general information on the supported platforms, such as the location of the spack-stack installations on tier 1 platforms and instructions on how to set up an environment for **building** spack-stack environments. Information on **using** spack-stack environments for development of downstream applications is available on the spack-stack wiki: https://github.com/JCSDA/spack-stack/wiki

.. _EnvironmentNamingConventions:

=============================================================
Environment naming conventions
=============================================================

The following naming conventions are used on all fully-supported (tier 1) sites. Environments are named using an abbreviated prefix that depends on the template/purpose, followed by the compiler name and version: ``prefix-compiler-version``. The following table lists the prefices and gives a few examples.

+----------------------------------+---------------------------------------------------------+-------------------+------------------------------+
| Template (``configs/templates``) | Description                                             | Prefix            | Examples                     |
+==================================+=========================================================+===================+==============================+
| ``unified-dev``                  | Unified environment for all organizations/applications  | ``ue``            | ``ue-intel-2021.10.0``       |
+----------------------------------+---------------------------------------------------------+-------------------+------------------------------+
| ``skylab-dev``                   | JEDI/Skylab environment for JEDI, models, EWOK          | ``se``            | ``se-apple-clang-14.0.6``    |
+----------------------------------+---------------------------------------------------------+-------------------+------------------------------+
| ``neptune-dev``                  | NEPTUNE standalone environment (with xNRL Python)       | ``ne``            | ``ne-oneapi-2024.2.1``       |
+----------------------------------+---------------------------------------------------------+-------------------+------------------------------+
| ``cylc-dev``                     | Environment for running cylc (separate from other envs) | ``ce``            | ``ce-gcc-10.3.0``            |
+----------------------------------+---------------------------------------------------------+-------------------+------------------------------+
| ``gsi-addon-dev``                | GSI addon (chained) environment on top of unified env.  | ``gsi``           | ``gsi-gcc-13.3.0``           |
+----------------------------------+---------------------------------------------------------+-------------------+------------------------------+
| ``unified-dev`` with new ESMF    | Unified environment with new ESMF (chained from ``ue``) | ``ue-esmf870b99`` | ``ue-esmf870b99-aocc-4.2.0`` |
+----------------------------------+---------------------------------------------------------+-------------------+------------------------------+

To support users who consistently want the latest release, on NOAA RDHPCS tier 1 platforms, soft links pointing to the modulefiles associated with the latest release of the Unified Environment are provided under the main spack-stack directory. The usage consists of ``module use /path/to/spack-stack/latest-ue-<compiler>``, and then loading the spack-stack meta-modules as usual. These soft links should be updated when each release is finalized.

.. _Preconfigured_Sites_Tier1:

=============================================================
Pre-configured sites (tier 1)
=============================================================

+---------------------+-----------------------+--------------------+--------------------------------------------------------+-----------------+
| Organization        | System                | Compilers          | Location of top-level spack-stack directory            | Maintainers     |
+=====================+=======================+====================+========================================================+=================+
| **HPC platforms**                                                                                                                           |
+---------------------+-----------------------+--------------------+--------------------------------------------------------+-----------------+
|                     | Hercules              | GCC, Intel         | ``/apps/contrib/spack-stack/``                         | EPIC / JCSDA    |
| MSU                 +-----------------------+--------------------+--------------------------------------------------------+-----------------+
|                     | Orion                 | GCC, Intel         | ``/apps/contrib/spack-stack/``                         | EPIC / JCSDA    |
+---------------------+-----------------------+--------------------+--------------------------------------------------------+-----------------+
|                     | Discover SCU16        | GCC, Intel         | ``/gpfsm/dswdev/jcsda/spack-stack/scu16/``             | JCSDA           |
| NASA                +-----------------------+--------------------+--------------------------------------------------------+-----------------+
|                     | Discover SCU17        | GCC, Intel         | ``/gpfsm/dswdev/jcsda/spack-stack/scu17/``             | JCSDA           |
+---------------------+-----------------------+--------------------+--------------------------------------------------------+-----------------+
| NCAR-Wyoming        + Derecho               | GCC, Intel         | ``/glade/work/epicufsrt/contrib/spack-stack/derecho/`` | EPIC / JCSDA    |
+---------------------+-----------------------+--------------------+--------------------------------------------------------+-----------------+
| NOAA (NCEP)         | Acorn                 | Intel              | ``/lfs/h1/emc/nceplibs/noscrub/spack-stack/``          | NOAA-EMC        |
+---------------------+-----------------------+--------------------+--------------------------------------------------------+-----------------+
|                     | Gaea C5               | Intel              | ``/ncrc/proj/epic/spack-stack/``                       | EPIC / NOAA-EMC |
|                     +-----------------------+--------------------+--------------------------------------------------------+-----------------+
|                     | Gaea C6               | Intel              | ``/ncrc/proj/epic/spack-stack/c6/``                    | EPIC / NOAA-EMC |
| NOAA (RDHPCS)       +-----------------------+--------------------+--------------------------------------------------------+-----------------+
|                     | Hera                  | GCC, Intel         | ``/contrib/spack-stack/``                              | EPIC / NOAA-EMC |
|                     +-----------------------+--------------------+--------------------------------------------------------+-----------------+
|                     | Jet                   | GCC, Intel         | ``/contrib/spack-stack``                               | EPIC / NOAA-EMC |
+---------------------+-----------------------+--------------------+--------------------------------------------------------+-----------------+
|                     | Narwhal               | GCC, Intel, oneAPI | ``/p/app/projects/NEPTUNE/spack-stack/``               | NRL             |
|                     +-----------------------+--------------------+--------------------------------------------------------+-----------------+
| U.S. Navy (HPCMP)   | Nautilus              | GCC, Intel, oneAPI | ``/p/app/projects/NEPTUNE/spack-stack/``               | NRL             |
|                     +-----------------------+--------------------+--------------------------------------------------------+-----------------+
|                     | Blueback (earlyaccess)| GCC, oneAPI        | (experimental only)                                    | NRL             |
+---------------------+-----------------------+--------------------+--------------------------------------------------------+-----------------+
| Univ. of Wisconsin  | S4                    | Intel              | ``/data/prod/jedi/spack-stack/``                       | SSEC            |
+---------------------+-----------------------+--------------------+--------------------------------------------------------+-----------------+
| **Cloud platforms**                                                                                                                         |
+---------------------+-----------------------+--------------------+--------------------------------------------------------+-----------------+
|                     | AMI Red Hat 8         | GCC                | ``/home/ec2-user/spack-stack/``                        | JCSDA           |
+ Amazon Web Services +-----------------------+--------------------+--------------------------------------------------------+-----------------+
|                     | Parallelcluster JCSDA | GCC, Intel         |  *currently unavailable*                               | JCSDA           |
+---------------------+-----------------------+--------------------+--------------------------------------------------------+-----------------+
| NOAA (RDHPCS)       | RDHPCS Parallel Works | Intel              | ``/contrib/spack-stack-rocky8/``                       | EPIC / JCSDA    |
+---------------------+-----------------------+--------------------+--------------------------------------------------------+-----------------+
| U.S. Navy (HPCMP)   | HPCMP Parallel Works  | GCC                | ``/contrib/spack-stack/``                              | NRL             |
+---------------------+-----------------------+--------------------+--------------------------------------------------------+-----------------+


.. _Preconfigured_Sites_Orion:

------------------------------
MSU Orion
------------------------------

The following is required for building new spack environments with any supported compiler on this platform.

.. code-block:: console

   # To access /apps/contrib/spack-stack directory/, first login to orion-devel-1 or orion-devel-2 login node.
   # Then sudo to role-epic account.
   module purge


.. _Preconfigured_Sites_Hercules:

------------------------------
MSU Hercules
------------------------------

The following is required for building new spack environments with any supported compiler on this platform.

.. code-block:: console

   # To access /apps/contrib/spack-stack directory/, first login to orion-devel-1 or orion-devel-2 login node.
   # Then sudo to role-epic account.
   module purge


.. _Preconfigured_Sites_Discover_SCU16:

------------------------------
NASA Discover SCU16
------------------------------

The following is required for building new spack environments with any supported compiler on this platform.

.. code-block:: console

   module purge
   module use /discover/swdev/gmao_SIteam/modulefiles-SLES12
   module use /discover/swdev/jcsda/spack-stack/scu16/modulefiles
   module load miniconda/3.9.7

.. _Preconfigured_Sites_Discover_SCU17:

------------------------------
NASA Discover SCU17
------------------------------

The following is required for building new spack environments with any supported compiler on this platform.

.. code-block:: console

   module purge
   module use /discover/swdev/gmao_SIteam/modulefiles-SLES15
   module use /discover/swdev/jcsda/spack-stack/scu17/modulefiles

.. _Preconfigured_Sites_Narwhal:

------------------------------
NAVY HPCMP Narwhal
------------------------------

The following is required for building new spack environments with Intel on this platform.. Don't use ``module purge`` on Narwhal!

.. code-block:: console

   umask 0022
   module unload PrgEnv-cray
   module load PrgEnv-intel/8.4.0
   module unload intel
   module load intel-classic/2023.2.0
   module unload cray-mpich
   module unload craype-network-ofi
   # Warning. Do not load craype-network-ucx
   # or cray-mpich-ucx/8.1.26!
   # There is a bug in the modulefile that prevents
   # spack from setting the environment for its
   # build steps when the module is already
   # loaded. Instead, let spack load it when the
   # package requires it.
   #module load craype-network-ucx
   #module load cray-mpich-ucx/8.1.26
   module load libfabric/1.12.1.2.2.1
   module unload cray-libsci
   module load cray-libsci/23.05.1.4

The following is required for building new spack environments with Intel oneAPI on this platform.. Don't use ``module purge`` on Narwhal!

.. code-block:: console

   umask 0022
   module unload PrgEnv-cray
   module load PrgEnv-intel/8.4.0
   module unload intel
   module load intel/2024.2
   module unload cray-mpich
   module unload craype-network-ofi
   # Warning. Do not load craype-network-ucx
   # or cray-mpich-ucx/8.1.26!
   # There is a bug in the modulefile that prevents
   # spack from setting the environment for its
   # build steps when the module is already
   # loaded. Instead, let spack load it when the
   # package requires it.
   #module load craype-network-ucx
   #module load cray-mpich-ucx/8.1.26
   module load libfabric/1.12.1.2.2.1
   module unload cray-libsci
   module load cray-libsci/23.05.1.4

The following is required for building new spack environments with GNU on this platform.. Don't use ``module purge`` on Narwhal!

.. code-block:: console

   umask 0022
   module unload PrgEnv-cray
   module load PrgEnv-gnu/8.4.0
   module unload gcc
   module load gcc/10.3.0
   module unload cray-mpich
   module unload craype-network-ofi
   # Warning. Do not load craype-network-ucx
   # or cray-mpich-ucx/8.1.26!
   # There is a bug in the modulefile that prevents
   # spack from setting the environment for its
   # build steps when the module is already
   # loaded. Instead, let spack load it when the
   # package requires it.
   #module load craype-network-ucx
   #module load cray-mpich-ucx/8.1.26
   module load libfabric/1.12.1.2.2.1
   module unload cray-libsci
   module load cray-libsci/23.05.1.4

.. warning::
   After the successful build of a spack-stack environment, a utility script ``util/narwhal/fix_libsci.sh`` must be run to replace references to an old version of ``libsci`` in several shared libraries. See https://github.com/JCSDA/spack-stack/pull/1449 and https://github.com/JCSDA/spack-stack/issues/1447 for more information.

.. code-block:: console

   # After running 'spack install' (or after 'spack stack setup-meta-modules')
   ./util/narwhal/fix_libsci.sh 2>&1 | tee log.ENV_NAME_HERE.fix_libsci.001


.. _Preconfigured_Sites_Nautilus:

------------------------------
NAVY HPCMP Nautilus
------------------------------

The following is required for building new spack environments with any supported compiler on this platform.

.. code-block:: console

   umask 0022
   module purge


.. _Preconfigured_Sites_Blueback:

------------------------------
NAVY HPCMP Blueback
------------------------------

The following is required for building new spack environments with Intel oneAPI on this platform.. Don't use ``module purge`` on Blueback!

.. code-block:: console

   umask 0022
   # This section will be updated when Blueback becomes available.

The following is required for building new spack environments with GNU on this platform.. Don't use ``module purge`` on Blueback!

   umask 0022
   # This section will be updated when Blueback becomes available.


.. _Preconfigured_Sites_Derecho:

--------------------
NCAR-Wyoming Derecho
--------------------

The following is required for building new spack environments with any supported compiler on this platform.

.. code-block:: console

   module purge
   # ignore that the sticky module ncarenv/... is not unloaded
   export LMOD_TMOD_FIND_FIRST=yes
   module load ncarenv/23.09
   module use /glade/work/epicufsrt/contrib/spack-stack/derecho/modulefiles


.. _Preconfigured_Sites_Acorn:

-------------------------------
NOAA Acorn (WCOSS2 test system)
-------------------------------

On WCOSS2 OpenSUSE sets ``CONFIG_SITE`` which causes libraries to be installed in ``lib64``, breaking the ``lib`` assumption made by some packages. Therefore, ``CONFIG_SITE`` should remain set to empty in ``compilers.yaml``.

For official deployments on ``spack-stack`` on Acorn, be mindful of umask and group ownership, as these can be finicky. The umask value should be 002, otherwise various files can be assigned to the wrong group. In any case, running something to the effect of ``chgrp nceplibs <spack-stack dir> -R`` and ``chmod o+rX <spack-stack dir> -R`` after the whole installation is done is a good idea.

Note that for the installation using Intel 19, the system GCC, 7.5.0, is used on the backend for the Intel compiler. More recent versions of GCC are not reliably compatible. Likewise, for Intel 2022, GCC 10.2.0 is used on the backend. Intel 19 is not reliably compatible with C++17 standards, and Intel 2022 is not reliably compatible with C++20. Without a handful of package version restrictions, certain package builds will break, usually in the configure stage.

.. note::
   System-wide ``spack`` software installations are maintained by NCO on this platform, which are not associated with spack-stack.


.. _Preconfigured_Sites_Parallel_Works:

----------------------------------------
NOAA Parallel Works (AWS, Azure, Gcloud)
----------------------------------------

The following is required for building new spack environments with any supported compiler on this platform. The default module path needs to be removed, otherwise spack detects the system as Cray.

.. code-block:: console

   module purge


.. _Preconfigured_Sites_Parallel_Works_Navy:

----------------------------------------
U.S. Navy Parallel Works (AWS)
----------------------------------------

The following is required for building new spack environments with GNU on this platform.

.. code-block:: console

   umask 0022
   module purge
   scl enable gcc-toolset-13 bash


.. _Preconfigured_Sites_Gaea_C5:

------------------------------
NOAA RDHPCS Gaea C5
------------------------------

The following is required for building new spack environments with Intel on this platform.. Don't use ``module purge`` on Gaea!

.. code-block:: console

   # These modules should be loaded by default, if not load (swap) with:
   module load PrgEnv-intel/8.3.3
   module load intel-classic/2023.1.0
   module load cray-mpich/8.1.25
   module load python/3.9.12


.. note::
   On Gaea, running ``module available`` without the option ``-t`` can lead to an error: ``/usr/bin/lua5.3: /opt/cray/pe/lmod/lmod/libexec/Spider.lua:568: stack overflow``

.. note::
   On Gaea, a current limitation is that any executable that is linked against the MPI library (``cray-mpich``) must be run through ``srun`` on a compute node, even if it is run serially (one process). This is in particular a problem when using ``ctest`` for unit testing created by the ``ecbuild add_test`` macro. A workaround is to use the `cmake` cross-compiling emulator for this:

.. code-block:: console

   cmake -DCMAKE_CROSSCOMPILING_EMULATOR="/usr/bin/srun;-n;1" -DMPIEXEC_EXECUTABLE="/usr/bin/srun" -DMPIEXEC_NUMPROC_FLAG="-n" PATH_TO_SOURCE


.. _Preconfigured_Sites_Gaea_C6:

------------------------------
NOAA RDHPCS Gaea C6
------------------------------

The following is required for building new spack environments with Intel on this platform.. Don't use ``module purge`` on Gaea!

.. code-block:: console

   # These modules should be loaded by default, if not load (swap) with:
   module load PrgEnv-intel/8.3.3
   module load intel-classic/2023.2.0
   module load cray-mpich/8.1.25
   module load python/3.9.12


.. note::
   On Gaea, running ``module available`` without the option ``-t`` can lead to an error: ``/usr/bin/lua5.3: /opt/cray/pe/lmod/lmod/libexec/Spider.lua:568: stack overflow``

.. note::
   On Gaea, a current limitation is that any executable that is linked against the MPI library (``cray-mpich``) must be run through ``srun`` on a compute node, even if it is run serially (one process). This is in particular a problem when using ``ctest`` for unit testing created by the ``ecbuild add_test`` macro. A workaround is to use the `cmake` cross-compiling emulator for this:

.. code-block:: console

   cmake -DCMAKE_CROSSCOMPILING_EMULATOR="/usr/bin/srun;-n;1" -DMPIEXEC_EXECUTABLE="/usr/bin/srun" -DMPIEXEC_NUMPROC_FLAG="-n" PATH_TO_SOURCE


.. _Preconfigured_Sites_Hera:

------------------------------
NOAA RDHPCS Hera
------------------------------

The following is required for building new spack environments with any supported compiler on this platform.

.. code-block:: console

   module purge

.. note::
   On Hera, a dedicated node exists for ``ecflow`` server jobs (``hecflow01``). Users starting ``ecflow_server`` on the regular login nodes will see their servers being killed every few minutes, and may be barred from accessing the system.


.. _Preconfigured_Sites_Jet:

------------------------------
NOAA RDHPCS Jet
------------------------------

The following is required for building new spack environments with any supported compiler on this platform.

.. code-block:: console

   module purge


.. _Preconfigured_Sites_S4:

------------------------------
UW (Univ. of Wisconsin) S4
------------------------------

The following is required for building new spack environments with any supported compiler on this platform.

.. code-block:: console

   module purge


.. _Preconfigured_Sites_AWS_Parallelcluster:

------------------------------------------------
Amazon Web Services Parallelcluster Ubuntu 20.04
------------------------------------------------

**NEEDS UPDATING**

The JCSDA-managed AWS Parallel Cluster is currently unavailable.


.. _Preconfigured_Sites_AWS_SingleNode_RH8:

-----------------------------------------
Amazon Web Services Single Node Red Hat 8
-----------------------------------------

**NEEDS UPDATING**

Use a c6i.4xlarge instance or larger if running out of memory with AMI "skylab-8.0.0-redhat8" (see JEDI documentation at https://jointcenterforsatellitedataassimilation-jedi-docs.readthedocs-hosted.com/en/latest for more information).


.. _Preconfigured_Sites_Tier2:

=============================================================
Pre-configured sites (tier 2)
=============================================================

Tier 2 preconfigured site are not officially supported by spack-stack. As such, instructions for these systems may be provided here, in form of a `README.md` in the site directory, or may not be available. Also, these site configs are not updated on the same regular basis as those of the tier 1 systems and therefore may be out of date and/or not working.


.. _Preconfigured_Sites_Blackpearl:

------------------------------
Blackpearl
------------------------------

Blackpearl is an Oracle Linux 9 installation running under Windows Subsystem for Linux (WSL2) on Windows 11. This is the development system of one of the spack-stack developers and maybe useful as an example configuration for users with a similar setup.


.. _Preconfigured_Sites_Casper:

------------------------------
NCAR-Wyoming Casper
------------------------------

The following is required for building new spack environments with any supported compiler on this platform.

**NEEDS UPDATING**

.. code-block:: console

   module purge
   # ignore that the sticky module ncarenv/... is not unloaded
   export LMOD_TMOD_FIND_FIRST=yes
   module load ncarenv/23.10
   module use /glade/work/epicufsrt/contrib/spack-stack/casper/modulefiles
   module load ecflow/5.8.4


.. _Preconfigured_Sites_EMC_RHEL:

------------------------------
EMC RedHat Enterprise Linux 8
------------------------------

**NEEDS UPDATING**


.. _Preconfigured_Sites_Frontera:

------------------------------
??? Frontera
------------------------------

**NEEDS UPDATING**


------------------------------
Linux/macOS default configs
------------------------------

The Linux and macOS configurations are **not** meant to be used as is, as user setups and package versions vary considerably. Instructions for adding this information can be found in :numref:`Section %s <NewSiteConfigs>`.


.. _Preconfigured_Sites_AWS_Ubuntu2404:

----------------
AWS Ubuntu 24.04
----------------

To build consult the `README.md` in the `sites/tier2/aws-ubuntu2404`.

This image can contain GCC and Intel compilers. It is strongly suggested that if you are to use either environment, it is suggested to separate environments into their own terminal. 


.. _Preconfigured_Sites_AWS_Rocky8:

----------
AWS Rocky8
----------

To build consult the `README.md` in the `sites/tier2/aws-rocky8`.

This image can contain GCC and Intel compilers. It is strongly suggested that if you are to use either environment, it is suggested to separate environments into their own terminal.


.. _Configurable_Sites_CreateEnv:

========================
Create local environment
========================

The following instructions install a new spack environment on a pre-configured site. Instructions for creating a new site config on a configurable system (i.e. a generic Linux or macOS system) can be found in :numref:`Section %s <NewSiteConfigs>`. The options for the ``spack stack`` extension are explained in :numref:`Section %s <SpackStackExtension>`.

The following instructions apply to the basic environments (``unified-dev``, ``skylab-dev``, ``neptune-dev``). Add-on (i.e. chained) environments and the ``cylc`` environment require special instructions (see below).

.. code-block:: console

   git clone --recurse-submodules https://github.com/jcsda/spack-stack.git
   cd spack-stack

   # Ensure Python 3.6+ is available and the default before sourcing spack.
   # Note this is only used for building the environment. Once the
   # environment is built, spack-stack provides the proper python
   # executable which needs to be utilized to build applications with the
   # newly created environment.

   # Sources Spack from submodule and sets ${SPACK_STACK_DIR}
   source setup.sh

   # See a list of sites and templates
   spack stack create env -h

   # Create a pre-configured Spack environment in envs/<template>.<site>
   # (copies site-specific, application-specific, and common config files into the environment directory)
   spack stack create env --site hera --template unified-dev --name unified-dev.hera.intel --compiler intel

   # Activate the newly created environment
   # Optional: decorate the command line prompt using -p
   #     Note: in some cases, this can mess up long lines in bash
   #     because color codes are not escaped correctly. In this
   #     case, use export SPACK_COLOR='never' first.
   cd envs/unified-dev.hera.intel/
   spack env activate [-p] .

   # Optionally edit config files (spack.yaml, packages.yaml compilers.yaml, modules.yaml, ...)
   emacs spack.yaml
   emacs common/*.yaml
   emacs site/*.yaml

   # Process/concretize the specs; optionally check for duplicate packages
   spack concretize | ${SPACK_STACK_DIR}/util/show_duplicate_packages.py -d [-c] log.concretize

   # Optional step for systems with a pre-configured spack mirror, see below.

   # Install the environment, recommended to always use --source
   # to install the source code with the compiled binary package
   spack install --source [--verbose] [--fail-fast]

   # Create lua module files
   spack module lmod refresh

   # Create meta-modules for compiler, mpi, python
   spack stack setup-meta-modules

   # Check permissions for systems where non-owning users/groups need access
   ${SPACK_STACK_DIR}/util/check_permissions.sh

.. note::
  You may want to capture the output from :code:`spack concretize` and :code:`spack install` comands in log files.
  For example:

  .. code-block:: bash

    spack concretize 2>&1 | tee log.concretize
    spack install [--verbose] [--fail-fast] 2>&1 | tee log.install

For installing chained environments, the user is referred to section :numref:`%s <Add_Test_Packages>`.

The ``cylc`` environment is another special environment. This environment is not chained, thus the instructions provided above until and including the ``spack install`` are still valid. The following differences apply:

1. The ``cylc`` environment can only be built with a reasonably recent version of the GNU compilers (``gcc`` version ``10`` or later, as long as spack-stack in general supports the version; see section :numref:`%s <NewSiteConfigs>` for a compiler compatibility matrix). Further, on Cray systems, the Cray compiler wrappers can not be used (see the comment in ``configs/sites/tier1/narwhal/compilers.yaml``). Note that, as long as the ``cylc`` environment does not use MPI (which is currently the case), it is not necessary to toggle the ``wrappers`` variant for the external ``cray-mpich`` package when using the native compilers without the Cray wrappers.

2. Instead of creating ``tcl`` or ``lmod`` modules and the associated meta-modules, this environment creates a spack environment view in ``/path/to/spack-stack/envs/env-name/view``. In order to use the ``cylc`` installation provided in that view, the user is advised to create a ``cylc-wrapper`` similar to the following bash script, and then create an alias ``cylc`` pointing to the wrapper script. This approach ensures that the environment in which ``cylc`` operates is encapsulated from the environment that users or ``cylc`` tasks operate in.

.. code-block:: bash

   > cat cylc-wrapper

   #!/bin/bash

   # Define environment variable CYLC_INSTALL_PREFIX here,
   # pointing to /path/to/spack-stack/envs/env-name/view,
   # or make sure the user did set it before calling the wrapper
   [ -z ${CYLC_INSTALL_PREFIX} ] && echo "CYLC_INSTALL_PREFIX not set!" && exit 1

   PATH=${CYLC_INSTALL_PREFIX}/bin:$PATH
   unset PYTHONPATH
   cylc "$@"

   > alias
   
   alias cylc='/path/to/cylc-wrapper'


.. _Preconfigured_Sites_ExtendingEnvironments:

======================
Extending environments
======================

Additional packages (and their dependencies) or new versions of packages can be added to existing environments. It is recommended to take a backup of the existing environment directory (e.g. using ``rsync``) or test this first as described in :numref:`Section %s <MaintainersSection_Testing_New_Packages>`, especially if new versions of packages are added that act themselves as dependencies for other packages. In some cases, adding new versions of packages will require rebuilding large portions of the stack, for example if a new version of ``hdf5`` is needed. In this case, it is recommended to start over with an entirely new environment.

In the simplest case, a new package (and its basic dependencies) or a new version of an existing package that is not a dependency for other packages can be added as described in the following example for a new version of ``ecmwf-atlas``.

1. Check if the package has any variants defined in the common (``env_dir/common/packages.yaml``) or site (``env_dir/site/packages.yaml``) package config and make sure that these are reflected
   correctly in the ``spec`` command:

.. code-block:: console

   spack spec ecmwf-atlas@0.29.0

2. Add package to environment specs:

.. code-block:: console

   spack add ecmwf-atlas@0.29.0

3. Run ``concretize`` step

.. code-block:: console

   spack concretize

4. Install

.. code-block:: console

   spack install [--verbose] [--fail-fast]

Further information on how to define variants for new packages, how to use these non-standard versions correctly as dependencies, ..., can be found in the `Spack Documentation <https://spack.readthedocs.io/en/latest>`_. Details on the ``spack stack`` extension of the ``spack`` are provided in :numref:`Section %s <SpackStackExtension>`.

.. note::
   Instead of ``spack add ecmwf-atlas@0.29.0``, ``spack concretize`` and ``spack install``, one can also just use ``spack install ecmwf-atlas@0.29.0`` after checking in the first step (``spack spec``) that the package will be installed as desired.
