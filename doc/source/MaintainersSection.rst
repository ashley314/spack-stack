.. _MaintainersSection:

Maintainers/Developers Section
******************************

==============================
Manual software installations
==============================

The following manual software installations may or may not be required as prerequisites, depending on the specific platform. For configurable/user systems, please consult :numref:`Section %s <Preconfigured_Sites>`, for preconfigured systems please consult :numref:`Section %s <NewSiteConfigs>`. Note that for preconfigured systems, the following one-off installations are only necessary for the maintainers of the preconfigured installations, users **do not** have to repeat any of these steps.

..  _MaintainersSection_Git_LFS:

------------------------------
git-lfs
------------------------------

Building ``git-lfs`` with spack isn't straightforward as it requires ``go-bootstrap`` and ``go`` language support, which many compilers don't build correctly. We therefore require ``git-lfs`` as an external package. On many of the HPC systems, it is already available as a separate module or as part of a ``git`` module. On macOS and Linux, it can be installed using ``brew`` or other package managers (see :numref:`Sections %s <NewSiteConfigs_macOS>` and :numref:`%s <NewSiteConfigs_Linux>` for examples). The following instructions install ``git-lfs`` on a CentOS 7.9 system from the OS rpm:

.. code-block:: console

   module purge
   cd /my/path/to/spack-stack/
   mkdir -p git-lfs-2.10.0/src
   cd git-lfs-2.10.0/src
   wget --content-disposition https://packagecloud.io/github/git-lfs/packages/el/7/git-lfs-2.10.0-1.el7.x86_64.rpm/download.rpm
   rpm2cpio git-lfs-2.10.0-1.el7.x86_64.rpm | cpio -idmv
   mv usr/* ../

Following this "installation", create modulefile from template ``doc/modulefile_templates/git-lfs``.

..  _MaintainersSection_Qt5:

------------------------------
qt (qt@5)
------------------------------

Building ``qt`` with spack isn't straightforward as it requires many libraries related to the graphical desktop that are often tied to the operating system, and which many compilers don't build correctly. We therefore require ``qt`` as an external package. On many of the HPC systems, it is already available as a separate module or provided by the operating system. On macOS and Linux, it can be installed using ``brew`` or other package managers (see :numref:`Sections %s <NewSiteConfigs_macOS>` and :numref:`%s <NewSiteConfigs_Linux>` for examples). 

On HPC systems without a sufficient Qt5 installation, we install it outside of spack with the default OS compiler and then point to it in the site's ``packages.yaml``. The following instructions install ``qt@5.15.2`` on Discover SCU16 in ``/discover/swdev/jcsda/spack-stack/qt-5.15.2/5.15.2/gcc_64``.

.. code-block:: console

   mkdir -p /discover/swdev/jcsda/spack-stack/qt-5.15.2/src
   cd /discover/swdev/jcsda/spack-stack/qt-5.15.2/src
   wget --no-check-certificate http://download.qt.io/official_releases/online_installers/qt-unified-linux-x64-online.run
   chmod u+x qt-unified-linux-x64-online.run
   ./qt-unified-linux-x64-online.run

Sign into qt, select customized installation, choose qt@5.15.2 only (uncheck all other boxes) and set install prefix to ``/discover/swdev/jcsda/spack-stack/qt-5.15.2``. After the successful installation, create modulefile ``/discover/swdev/jcsda/spack-stack/modulefiles/qt/5.15.2`` from template ``doc/modulefile_templates/qt`` and update ``QT_PATH`` in this file.

.. note::
   The dependency on ``qt`` is introduced by ``ecflow``, which at present requires using ``qt@5`` - earlier or newer versions will not work.

.. note::
   On air-gapped systems, the above method may not work (we have not encountered such a system so far).

.. note::
   If ``./qt-unified-linux-x64-online.run`` fails to start with the error ``qt.qpa.xcb: could not connect to display`` and a role account is being used, follow the procedure described in https://www.thegeekdiary.com/how-to-set-x11-forwarding-export-remote-display-for-users-who-switch-accounts-using-sudo to export the display. A possible warning ``xauth:  file /ncrc/home1/role.epic/.Xauthority does not exist`` can be ignored, since this file gets created by the ``xauth`` command.

..  _MaintainersSection_MySQL:

-----------------------------------
MySQL (server and client; optional)
-----------------------------------

For certain limited use cases, we need to provide ``mysql`` through spack-stack. We do not build ``mysql`` with spack, since it depends on specific versions of the ``boost`` library and C++ standards that make our large environments very complicated and often don't build on older systems. Instead, we identify the default ``glibc`` of the system, obtain the binary tarball from the `MySQL Community Downloads <https://dev.mysql.com/downloads/mysql/>`_  page and make it available to spack as an external package. Alternatively, the default MySQL software can be installed using the OS package manager, provided that it is a recent enough version (8.x). The following instructions are for installing ``MySQL`` using the community tarball:

1. Check the glibc version by executing ``ldd --version``

.. code-block:: console

   ldd (GNU libc) 2.17

2. Download and unpack the correct tarball, in this case option "Linux - Generic (glibc 2.17) (x86, 64-bit), Compressed TAR Archive Minimal Install 8.0.31"

.. code-block:: console

   cd /path/to/spack-stack/
   mkdir -p mysql-8.0.31/src
   cd mysql-8.0.31/src
   wget https://dev.mysql.com/get/Downloads/MySQL-8.0/mysql-8.0.31-linux-glibc2.17-x86_64-minimal.tar.xz
   tar -xvf mysql-8.0.31-linux-glibc2.17-x86_64-minimal.tar.xz
   # This moves the content of directory "mysql-8.0.31-linux-glibc2.17-x86_64-minimal" one level up, next to the "src" directory
   mv mysql-8.0.31-linux-glibc2.17-x86_64-minimal/* ..
   rmdir mysql-8.0.31-linux-glibc2.17-x86_64-minimal

3. Create modulefile ``/path/to/spack-stack/modulefiles/mysql/8.0.31`` from template ``doc/modulefile_templates/mysql`` and update ``MYSQL_PATH`` in this file.

..  _MaintainersSection_Texlive:

------------------------------
Texlive (TeX/LaTeX; optional)
------------------------------

Building ``texlive`` isn't straightforward as it has many dependencies. Since it is only used to generated documentation for ``spack-stack`` (and other projects), i.e. not to compile any code, it makes no sense to build it with ``spack``. We therefore require ``texlive`` or any other compatible TeX/LaTeX distribution as an external package.

On many of the HPC systems, it is already available as a separate module or as part of the default operating system. On macOS, the MacTeX distribution provides a full and easy-to-install TeX/LaTeX environment (see :numref:`Section %s <NewSiteConfigs_macOS>`). On Linux, ``texlive`` can be installed using the default package manager (see :numref:`Section %s <NewSiteConfigs_Linux>`).


.. _Preconfigured_Sites_SpackMirror:

=========================================================
Optional step for sites with a preconfigured spack mirror
=========================================================

To check if a mirror is configured, look for ``local-source`` in the output of

.. code-block:: bash

   spack mirror list

If a mirror exists, add new packages to the mirror. Here, ``/path/to/mirror`` is the location from the above list command without the leading ``file://``

.. code-block:: bash

   spack mirror create -a -d /path/to/mirror

If this fails with ``git lfs`` errors, check the site config for which module to load for ``git lfs`` support. Load the module, then run the ``spack mirror add`` command, then unload the module and proceed with the installation.


==============================
Pre-configuring sites
==============================

.. _MaintainersSection_Preface:

------------------------------
Preface/general instructions
------------------------------

Preconfigured sites are defined through spack configuration files in the spack-stack directory ``configs/sites``, for example ``configs/sites/orion``. All files in the site-specific subdirectory will be copied into the environment into ``envs/env-name/site``. Site-specific configurations consist of general definitions (``config.yaml``), packages (``packages.yaml``, ``packages_*.yaml``), compilers (``compilers.yaml``), modules (``modules.yaml``), mirrors (``mirrors.yaml``) etc. These configurations overwrite the common configurations that are copied from ``configs/common`` into ``envs/env-name/common``.

The instructions below are platform-specific tasks that only need to be done once and can be reused for new spack environments. To build new environments on preconfigured platforms, follow the instructions in :numref:`Section %s <Preconfigured_Sites_ExtendingEnvironments>`.

Note that, for official installations of new environments on any supported platform, the ``spack install`` command should be invoked with the ``--source`` and ``--verbose`` arguments, i.e.:

.. code-block:: console
    
   spack install --source --verbose

.. _MaintainersSection_Discover_SCU16:

------------------------------
NASA Discover SCU16
------------------------------

On Discover SCU16, ``qt`` needs to be installed as a one-off before spack can be used. When using the GNU compiler, it is also necessary to build your own ``openmpi`` or other MPI library, which requires adapting the installation to the network hardware and ``slurm`` scheduler.

qt (qt@5)
   The default ``qt@5`` in ``/usr`` is incomplete and thus insufficient for building ``ecflow``. After loading/unloading the modules as shown below, refer to 
   :numref:`Section %s <MaintainersSection_Qt5>` to install ``qt@5.15.2`` in ``/discover/swdev/jcsda/spack-stack/scu16/qt-5.15.2`` (note: it is currently installed in ``/discover/swdev/jcsda/spack-stack/qt-5.15.2``; an upcoming large system update will require is to rebuild anyway).

.. _MaintainersSection_Discover_SCU17:

------------------------------
NASA Discover SCU17
------------------------------

On Discover SCU17, ``qt`` needs to be installed as a one-off before spack can be used.

**These instructions are missing. The current `qt` used in the SCU17 site config does not work (/usr/local/other/xpdf/4.04/Deps).**

.. _MaintainersSection_Narwhal:

------------------------------
NAVY HPCMP Narwhal
------------------------------

On Narwhal, ``git-lfs`` and ``qt`` need to be installed as a one-off before spack can be used. Also, temporarily it is necessary to install ``node.js`` as an external package to work around build errors for ``py-jupyter-server`` (see https://github.com/JCSDA/spack-stack/issues/928 and https://github.com/spack/spack/issues/41899).

git-lfs
   The following instructions install ``git-lfs`` in ``/p/app/projects/NEPTUNE/spack-stack/git-lfs-2.10.0``. Version 2.10.0 is the default version for Narwhal. First, download the ``git-lfs`` RPM on a system with full internet access (e.g., Derecho) using ``wget https://download.opensuse.org/repositories/openSUSE:/Leap:/15.2/standard/x86_64/git-lfs-2.10.0-lp152.1.2.x86_64.rpm`` and copy this file to ``/p/app/projects/NEPTUNE/spack-stack/git-lfs-2.10.0/src``. Then switch to Narwhal and run the following commands. 

   .. code-block:: console

      cd /p/app/projects/NEPTUNE/spack-stack/git-lfs-2.10.0/src
      rpm2cpio git-lfs-2.10.0-lp152.1.2.x86_64.rpm | cpio -idmv
      mv usr/* ../

   Create modulefile ``/p/app/projects/NEPTUNE/spack-stack/modulefiles/git-lfs/2.10.0`` from template ``doc/modulefile_templates/git-lfs`` and update ``GITLFS_PATH`` in this file.

qt (qt@5)
   The default ``qt@5`` in ``/usr`` is incomplete and thus insufficient for building ``ecflow``. After loading/unloading the modules as shown below, refer to 
   :numref:`Section %s <MaintainersSection_Qt5>` to install ``qt@5.15.2`` in ``/p/app/projects/NEPTUNE/spack-stack/qt-5.15.2``.

.. code-block:: console

   module unload PrgEnv-cray
   module load PrgEnv-intel/8.1.0
   module unload intel

   module unload cray-python
   module load cray-python/3.9.7.1
   module unload cray-libsci
   module load cray-libsci/22.08.1.1

   module load gcc/10.3.0

node.js
  ``node.js`` is difficult to install via ``spack``, but is needed to install certain Python packages. The complication is that when using a newer ``gcc`` compiler (either directly or as backend for ``icc`` etc.), the OS ``node.js`` errors out with unresolved symbols in the ``libstdc++`` library. Therefore, we need to install ``node.js`` with ``gcc@10.3.0`` loaded, and create modulefile ``node.js/20.10.0`` from template ``modulefiles/node.js``.

.. code-block:: console

   module unload PrgEnv-cray
   module load PrgEnv-gnu/8.3.2
   module unload gcc
   module load gcc/10.3.0

   mkdir -p node-js-20.10.0/src && cd node-js-20.10.0/src
   wget https://nodejs.org/dist/v20.10.0/node-v20.10.0.tar.gz
   tar -xvzf node-v20.10.0.tar.gz
   cd node-v20.10.0/
   ./configure --partly-static \
       --prefix=/p/app/projects/NEPTUNE/spack-stack/node-js-20.10.0/gcc-10.3.0 \
       2>&1 | tee log.config
   make 2>&1 | tee log.make
   make install 2>&1 | tee log.install

.. _MaintainersSection_Derecho:

------------------------------
NCAR-Wyoming Derecho
------------------------------

libfabric (temporary)
  Until CISL fixes its unusual way of setting up Cray module environments, it is necessary to create a libfabrics module to be able to use the cray-mpich MPI library without Cray compiler wrappers. Create a module file based on the template ``doc/modulefile_templates/libfabric`` in directory ``/glade/work/epicufsrt/contrib/spack-stack/derecho/libfabric``. This module is currently listed in the dependency modules for the ``cray-mpich`` MPI provider in the Derecho site config. It is also necessary to "include" (a confusing term, it used to be "whitelist") the ``cray-mpich`` module in Derecho's ``modules.yaml`` file, because the CISL ``cray-mpich`` module cannot be loaded without loading their compiler modules.

cray-pals (temporary)
  Until CISL fixes its unusual way of setting up Cray module environments, it is necessary to create a cray-pals (parallel application launcher) module to be able to find ``mpirun`` etc. Create directory ``/glade/work/epicufsrt/contrib/spack-stack/derecho/cray-pals`` and copy file ``/opt/cray/pe/lmod/modulefiles/core/cray-pals/1.2.11.lua`` into this directory.

.. _MaintainersSection_Parallel_Works:

----------------------------------------
NOAA Parallel Works (AWS, Azure, Gcloud)
----------------------------------------

See ``configs/sites/noaa-{aws,azure,gcloud}/README.md``.

.. _MaintainersSection_GaeaC5:

------------------------------
NOAA RDHPCS Gaea C5
------------------------------

On Gaea C5, ``qt`` needs to be installed as a one-off before spack can be used.

qt (qt@5)
   The default ``qt@5`` in ``/usr`` is incomplete and thus insufficient for building ``ecflow``. After loading/unloading the modules as shown below, refer to :numref:`Section %s <MaintainersSection_Qt5>` to install ``qt@5.15.2`` in ``/ncrc/proj/epic/spack-stack/qt-5.15.2``. :numref:`Section %s <MaintainersSection_Qt5>` describes how to export the X windows environment in order to install ``qt@5`` using the role account.

.. code-block:: console

   module unload intel-classic cray-mpich PrgEnv-intel
   module load gcc/10.3.0
   module load PrgEnv-gnu/8.3.3

.. _MaintainersSection_Hera:

------------------------------
NOAA RDHPCS Hera
------------------------------

Hera sits behind the NOAA firewall and doesn't have access to all packages on the web. It is therefore necessary to create a spack mirror on another platform. This can be done as described in section :numref:`Section %s <MaintainersSection_spack_mirrors>` for air-gapped systems.

.. _MaintainersSection_Jet:

------------------------------
NOAA RDHPCS Jet
------------------------------

On Jet, the ``target`` architecture must be set to ``core2`` to satisfy differences between the various Jet partitions and ensure that installations run on the front-end nodes (xjet-like) will function on the other partitions.

Like Hera, Jet sits behind the NOAA firewall and doesn't have access to all packages on the web. It is therefore necessary to create a spack mirror on another platform. This can be done as described in section :numref:`Section %s <MaintainersSection_spack_mirrors>` for air-gapped systems.

.. _MaintainersSection_AWS_Pcluster_Ubuntu:

------------------------------------------------
Amazon Web Services Parallelcluster Ubuntu 20.04
------------------------------------------------

See ``configs/sites/aws-pcluster/README.md``.


.. _MaintainersSection_Testing_New_Packages:

.. _MaintainersSection_spack_mirrors:

==================================
Creating/maintaining spack mirrors
==================================

Spack mirrors allow downloading the source code required to build environments once to a local directory (in the following also referred to as source cache), and then use this directory for subsequent installations. If a package cannot be found in the mirror (e.g. because a newer version is required), it will automatically be pulled from the web. It won't be added to the source cache automatically, this is a step that needs to be done manually.

Spack mirrors also make it possible to download the source code for an air-gapped machine on another system, then transferring the entire mirror to the system without internet access and using it during the installation.

-----------------------------
Spack mirrors for local reuse
-----------------------------

Since all spack-stack installations are based on environments, we only cover spack mirrors for environments here. For a more general discussion, users are referred to the `Spack Documentation <https://spack.readthedocs.io/en/latest>`_.

1. Create an environment as usual, activate it and run the concretization step (``spack concretize``), but do not start the installation yet.

2. Create the spack mirror in ``/path/to/spack-mirror``.

.. code-block:: console

   spack mirror create -a -d /path/to/spack-source

3. If the spack mirror already exists, then existing packages will be ignored and only new packages will be added to the mirror.

4. If not already included in the environment (e.g. from the spack-stack site config), add the mirror:

.. code-block:: console

   spack mirror list
   spack mirror add local-source file:///path/to/spack-source

The newly created local mirror should be listed at the top, which means that spack will search this directory first.

7. Proceed with the installation as usual.

------------------------------------
Spack mirrors for air-gapped systems
------------------------------------

The procedure for creating source caches is similar to using spack mirrors for local reuse, but a few additional steps are needed in between. The steps to create a mirror for bootstrapping spack on an air-gapped system are described in the next section.

1. On the air-gapped system: Create an environment as usual, activate it and run the concretization step (``spack concretize``), but do not start the installation yet.

2. Copy the file ``spack.lock`` (in ``envs/env-name/``) to the machine with full internet access using ``scp``, for example.

3. On the machine with full internet access: Load the basic external modules, if using a machine that is preconfigured for spack-stack (see :numref:`Section %s <Preconfigured_Sites>`) and make sure that ``git`` supports ``lfs`` (if necessary, load the external modules that spack-stack also uses).

4. On the machine with full internet access: check out the same version of ``spack-stack``, run ``setup.sh``, and then the following sequence of commands. The mirror will be created in directory ``./spack/var/spack/environments/air_gapped_mirror_env``, while the mirror source code downloaded based on ``spack.lock`` will be placed in the directory specified by the ``-d`` argument passed to ``spack mirror create`` (below).

.. code-block:: console

   spack env create air_gapped_mirror_env spack.lock
   cd envs/air_gapped_mirror_env/
   spack env activate .
   spack mirror create -a -d ./mirror/ 

5. On the air-gapped system: Copy the directory from the system with internet access to the local destination for the spack mirror. It is recommended to use ``rsync`` to avoid deleting existing packages, if updating an existing mirror on the air-gapped system. For example, to use ``rsync`` to copy the mirror directory from the machine with full internet access to the air-gapped system (with the ``rsync`` initiated from the air-gapped system):

.. code-block:: console

   rsync -av <username>@<source-host>:<path-to-mirror-directory-on-source-host> <destination-path-on-air-gapped-system>

6.. On the air-gapped system: Add the mirror to the spack environment's mirror list, unless already included in the site config.

.. code-block:: console

   spack mirror add locals-source  file:///path/to/spack-source
   spack mirror list

   The newly created local mirror should be listed at the top, which means that spack will search this directory first.

7. On the air-gapped system: Proceed with the installation as usual.

----------------------------------------
Bootstrap mirrors for air-gapped systems
----------------------------------------

To use spack on an air-gapped system, the first step is to create a mirror that allows spack to bootstrap from. The logic is similar to that for creating source caches for spack installations. On a system with access to the internet, a series of commands is run to create a bootstrap mirror. The bootstrap mirror is then copied over to the air-gapped system and registered so that spack can use it.

1. On the system with access to the internet, and using the same version of spack-stack (and its spack submodule) as on the air-gapped target system:

.. code-block:: console

   # After running "source setup.sh" in the spack-stack top-level directory

   # Create bootstrap mirror
   spack bootstrap mirror --binary-packages ./bootstrap-mirror

2. Transfer ``./bootstrap-mirror`` to the air-gapped system, preferably to the same location inside the spack-stack top-level directory (i.e. ``${SPACK_STACK_DIR}/bootstrap-mirror/``).

3. On the air-gapped system, run the following series of commands:

.. code-block:: console

   # After running "source setup.sh" in the spack-stack top-level directory

   # Register bootstrap mirror
   spack bootstrap add --trust local-sources /absolute/path/to/spack-stack/bootstrap-mirror/metadata/sources
   spack bootstrap add --trust local-binaries /absolute/path/to/spack-stack/bootstrap-mirror/metadata/sources

   # Update the boostrap binary cache index (silent response to this command means success)
   spack buildcache update-index /absolute/path/to/spack-stack/bootstrap-mirror/bootstrap_cache

4. Bootstrap spack explicitly before creating environments and check the status. There may be error messages along the way, but as long as the command completes and ``spack bootstrap status`` shows ``PASS`` for both ``Core Functionalities`` and ``Binary packages``, it was successful.

.. code-block:: console

   spack bootstrap now
   spack bootstrap status

==============================
Testing new packages
==============================

--------------------------------
Using spack to test/add packages
--------------------------------

The simplest case of adding new packages that are available in spack-stack is described in :numref:`Section %s <Preconfigured_Sites_ExtendingEnvironments>`. As mentioned there, it is advised to take a backup of the spack environment (and install directories if outside the spack environment directory tree). It is also possible to chain spack installations, which means creating a test environment that uses installed packages and modulefiles from another (e.g. authoritative) spack environment and build the packages to be tested in isolation.

Chaining spack-stack installations
----------------------------------

Chaining spack-stack installations is a powerful way to test adding new packages without affecting the existing packages. The idea is to define one or more upstream spack installations that the environment can use as dependencies. This is described in detail in :numref:`Section %s <Add_Test_Packages>`.

----------------------------------------
Testing/adding packages outside of spack
----------------------------------------

Sometimes, users may want to build new versions of packages frequently without using spack, for example as part of an existing build system (e.g. a ``cmake`` submodule or an ``ecbuild`` bundle). Also, users may wish to test developmental code that is not available and/or not ready for release in spack-stack. In this case, users need to unload the modules of the packages that are to be replaced, including their dependencies, and build the new version(s) themselves within the existing build system or manually. The loaded modules from the spack environment in this case provide the necessary dependencies, just like for any other build system.

.. note::
   Users are strongly advised to not interfere with the spack install tree. The environment install tree and module files should only be modified using spack.

Users can build multiple packages outside of spack and install them in a separate install tree, for example ``MY_INSTALL_TREE``. In order to find these packages, users must extend their environment as required for the system/the packages to be installed:

.. code-block:: console

   export PATH="$MY_INSTALL_TREE/bin:$PATH"
   export CPATH="$MY_INSTALL_TREE/include:$PATH"
   export LD_LIBRARY_PATH="$MY_INSTALL_TREE/lib64:$MY_INSTALL_TREE/lib:$LD_LIBRARY_PATH"
   # macOS
   export DYLD_LIBRARY_PATH="$MY_INSTALL_TREE/lib64:$MY_INSTALL_TREE/lib:$DYLD_LIBRARY_PATH"
   # Python packages, use correct lib/lib64 and correct python version
   export PYTHONPATH="$MY_INSTALL_TREE/lib/pythonX.Y/site-packages:$PYTHONPATH"

Python packages can be added in various ways:

1. Using ``python setup.py install --prefix=$MY_INSTALL_TREE ...`` or ``python3 -m pip install --no-deps --prefix=$MY_INSTALL_TREE ...``. The ``--no-deps`` options is very important, because ``pip`` may otherwise attempt to install dependencies that already exist in spack-stack. These dependencies are not only duplicates, they may also be different versions and/or compiled with different compilers/libraries (because they are wheels). This approach requires adding the appropriate subdirectories of ``$MY_INSTALL_TREE`` to the different search paths, as shown above.

2. Using Python virtual environments. Two important flags need to be passed to the command that creates the environment ``--system-site-packages`` and ``--without-pip``. After activating the environment, packages can be installed using `python3 -m pip` without having to specify ``--no-deps`` or ``--prefix``, and without having to manually modify ``PATH``, ``PYTHONPATH``, etc.

.. code-block:: console

   python3 -m venv --system-site-packages --without-pip $MY_INSTALL_TREE
   source $MY_INSTALL_TREE/bin/activate
   python3 -m pip install ...

.. note::
   Users are equally strongly advised to not use ``conda`` or ``miniconda`` in combination with Python modules provided by spack-stack, as well as not installing packages other than ``poetry`` in the basic ``miniconda`` installation for spack-stack (if using such a setup).
