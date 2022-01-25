============================
Tesseract Kinematics Package
============================

This package contains a common interface for Forward and Inverse kinematics for Chain, Tree's and Graphs including implementation using KDL and OPW Kinematics.

.. Note::

   The kinematics are load as plugins through a yaml config file which is added to the SRDF file.


************************
Kinematics Plugin Config
************************

The kinematics config file has four sections:

================  ===========
Section           Description
================  ===========
search_paths      A list of locations to search for libraries. It searches in the order provided.
search_libraries  A list of libraries to search for classes
fwd_kin_plugins   A map of group names to list of forward kinematic plugins.
inv_kin_plugins   A map of group names to list of inverse kinematic plugins.
================  ===========

Example Config file:

.. code-block:: yaml

   kinematic_plugins:
     search_paths:
       - /usr/local/lib
     search_libraries:
       - tesseract_kinematics_kdl_factories
     fwd_kin_plugins:
       iiwa_manipulator:
         default: KDLFwdKinChain
         plugins:
           KDLFwdKinChain:
             class: KDLFwdKinChainFactory
             config:
               base_link: base_link
               tip_link: tool0
     inv_kin_plugins:
       iiwa_manipulator:
         default: KDLInvKinChainLMA
         plugins:
           KDLInvKinChainLMA:
             class: KDLInvKinChainLMAFactory
             config:
               base_link: base_link
               tip_link: tool0
           KDLInvKinChainNR:
             class: KDLInvKinChainNRFactory
             config:
               base_link: base_link
               tip_link: tool0
       abb_manipulator:
         default: OPWInvKin
         plugins:
           OPWInvKin:
             class: OPWInvKinFactory
             config:
               base_link: base_link
               tip_link: tool0
               params:
                 a1: 0.100
                 a2: -0.135
                 b: 0.00
                 c1: 0.615
                 c2: 0.705
                 c3: 0.755
                 c4: 0.086
                 offsets: [0, 0, -1.57079632679, 0, 0, 0]
                 sign_corrections: [1, 1, 1, 1, 1, 1]
       ur_manipulator:
         default: URInvKin
         plugins:
           URInvKin:
             class: URInvKinFactory
             config:
               base_link: base_link
               tip_link: tool0
               model: UR10 #Current Options: UR3, UR5, UR10, UR3e, UR5e, UR10e
       rop_manipulator:
         default: ROPInvKin
         plugins:
           ROPInvKin:
             class: ROPInvKinFactory
             config:
               manipulator_reach: 2.0
               positioner_sample_resolution:
                 - name: positioner_joint_1
                   value: 0.1
               positioner:
                 class: KDLFwdKinChainFactory
                 config:
                   base_link: positioner_base_link
                   tip_link: positioner_tool0
               manipulator:
                 class: OPWInvKinFactory
                 config:
                   base_link: base_link
                   tip_link: tool0
                   params:
                     a1: 0.100
                     a2: -0.135
                     b: 0.00
                     c1: 0.615
                     c2: 0.705
                     c3: 0.755
                     c4: 0.086
                     offsets: [0, 0, -1.57079632679, 0, 0, 0]
                     sign_corrections: [1, 1, 1, 1, 1, 1]
       rep_manipulator:
         default: REPInvKin
         plugins:
           REPInvKin:
             class: REPInvKinFactory
             config:
               manipulator_reach: 2.0
               positioner_sample_resolution:
                 - name: positioner_joint_1
                   value: 0.1
                 - name: positioner_joint_2
                   value: 0.1
               positioner:
                 class: KDLFwdKinChainFactory
                 config:
                   base_link: positioner_base_link
                   tip_link: positioner_tool0
               manipulator:
                 class: OPWInvKinFactory
                 config:
                   base_link: base_link
                   tip_link: tool0
                   params:
                     a1: 0.100
                     a2: -0.135
                     b: 0.00
                     c1: 0.615
                     c2: 0.705
                     c3: 0.755
                     c4: 0.086
                     offsets: [0, 0, -1.57079632679, 0, 0, 0]
                     sign_corrections: [1, 1, 1, 1, 1, 1]

KDL Forward Kinematic Plugin
============================

.. code-block:: yaml

   kinematic_plugins:
     fwd_kin_plugins:
       manipulator:
         default: KDLFwdKinChain
         plugins:
           KDLFwdKinChain:
             class: KDLFwdKinChainFactory
             config:
               base_link: base_link
               tip_link: tool0

KDL Inverse Kinematic Plugin
============================

.. code-block:: yaml

   kinematic_plugins:
     inv_kin_plugins:
       iiwa_manipulator:
         default: KDLInvKinChainLMA
         plugins:
           KDLInvKinChainLMA:
             class: KDLInvKinChainLMAFactory
             config:
               base_link: base_link
               tip_link: tool0
           KDLInvKinChainNR:
             class: KDLInvKinChainNRFactory
             config:
               base_link: base_link
               tip_link: tool0

OPW Inverse Kinematic Plugin
============================

.. code-block:: yaml

   kinematic_plugins:
     inv_kin_plugins:
       manipulator:
         default: OPWInvKin
         plugins:
           OPWInvKin:
             class: OPWInvKinFactory
             config:
               base_link: base_link
               tip_link: tool0
               params:
                 a1: 0.100
                 a2: -0.135
                 b: 0.00
                 c1: 0.615
                 c2: 0.705
                 c3: 0.755
                 c4: 0.086
                 offsets: [0, 0, -1.57079632679, 0, 0, 0]
                 sign_corrections: [1, 1, 1, 1, 1, 1]

UR Inverse Kinematic Plugin
============================

Using preconfigured parameters:

.. code-block:: yaml

   kinematic_plugins:
     inv_kin_plugins:
       manipulator:
         default: URInvKin
         plugins:
           URInvKin:
             class: URInvKinFactory
             config:
               base_link: base_link
               tip_link: tool0
               model: UR10 #Current Options: UR3, UR5, UR10, UR3e, UR5e, UR10e

Using userdefined parameters:

.. code-block:: yaml

   kinematic_plugins:
     inv_kin_plugins:
       manipulator:
         default: URInvKin
         plugins:
           URInvKin:
             class: URInvKinFactory
             config:
               base_link: base_link
               tip_link: tool0
               params:
                 d1: 0.1273
                 a2: -0.612
                 a3: -0.5723
                 d4: 0.163941
                 d5: 0.1157
                 d6: 0.0922

Robot On Positioner (ROP) Inverse Kinematic Plugin
==================================================

.. code-block:: yaml

   kinematic_plugins:
     inv_kin_plugins:
       manipulator:
         default: ROPInvKin
         plugins:
           ROPInvKin:
             class: ROPInvKinFactory
             config:
               manipulator_reach: 2.0
               positioner_sample_resolution:
                 - name: positioner_joint_1
                   value: 0.1
               positioner:
                 class: KDLFwdKinChainFactory
                 config:
                   base_link: positioner_base_link
                   tip_link: positioner_tool0
               manipulator:
                 class: OPWInvKinFactory
                 config:
                   base_link: base_link
                   tip_link: tool0
                   params:
                     a1: 0.100
                     a2: -0.135
                     b: 0.00
                     c1: 0.615
                     c2: 0.705
                     c3: 0.755
                     c4: 0.086
                     offsets: [0, 0, -1.57079632679, 0, 0, 0]
                     sign_corrections: [1, 1, 1, 1, 1, 1]

Robot wit External Positioner (REP) Inverse Kinematic Plugin
============================================================

.. code-block:: yaml

   kinematic_plugins:
     inv_kin_plugins:
       manipulator:
         default: REPInvKin
         plugins:
           REPInvKin:
             class: REPInvKinFactory
             config:
               manipulator_reach: 2.0
               positioner_sample_resolution:
                 - name: positioner_joint_1
                   value: 0.1
                 - name: positioner_joint_2
                   value: 0.1
               positioner:
                 class: KDLFwdKinChainFactory
                 config:
                   base_link: positioner_base_link
                   tip_link: positioner_tool0
               manipulator:
                 class: OPWInvKinFactory
                 config:
                   base_link: base_link
                   tip_link: tool0
                   params:
                     a1: 0.100
                     a2: -0.135
                     b: 0.00
                     c1: 0.615
                     c2: 0.705
                     c3: 0.755
                     c4: 0.086
                     offsets: [0, 0, -1.57079632679, 0, 0, 0]
                     sign_corrections: [1, 1, 1, 1, 1, 1]

**********************
Creating IKFast Plugin
**********************

Prerequisites
=============

1. Install docker and add a user group with appropriate permissions.
2. curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add -
3. sudo apt-key fingerprint 0EBFCD88
4. sudo add-apt-repository -y "deb [arch=amd64] https://download.docker.com/linux/ubuntu $(lsb_release -cs) stable"
5. sudo apt update
6. sudo apt install -y docker-ce
7. sudo groupadd docker
8. sudo usermod -aG docker $USER
9. Verify that docker is installed correctly by running a test container using docker run hello-world. If this produces an error like "docker: Got permission denied while trying to connect to the Docker daemon socket," try rebooting your computer. This problem is caused by the docker usergroup not existing or not having sufficient permissions.
10. Grab the ikfast docker container we'll be using (you can take a look at what goes into it here): :code:`docker pull hamzamerzic/openrave`

Converting URDF to .dae
=======================

OpenRAVE uses the .dae file format, so we'll need to convert our URDF to .dae. Replace catkin_ws with the path to the workspace containing the URDF, replace /path/to/dir/my_robot.urdf with the actual path to the URDF, and replace /path/to/dir/robot_full.dae with the filename and path where you want the new .dae to be written. I've called it robot_full.dae to emphasize that the transforms in it have an unnecessary degree of precision, but we'll rectify this in a later step

.. code-block:: bash

    sudo apt install ros-<rosdistro>-collada-urdf
    source catkin_ws/devel/setup.bash
    rosrun collada_urdf urdf_to_collada /path/to/dir/my_robot.urdf /path/to/dir/robot_full.dae
    # Round off the precision to 6 decimal places:
    rosrun moveit_kinematics round_collada_numbers.py /path/to/dir/robot_full.dae /path/to/dir/robot.dae 6

Find Robot Link Indices
=======================

The robot links in the .dae are assigned indices, which can be somewhat opaque. OpenRAVE provides a utility to get info about the .dae, which we can access through the docker container. You'll need to map the container's /out directory to the folder containing your robot.dae, and pass the name of robot.dae as an argument to the Python script being run by the docker container.

.. code-block:: bash

   docker run --rm --env PYTHONPATH=/usr/local/lib/python2.7/dist-packages -v /path/to/dir:/out hamzamerzic/openrave /bin/bash -c "cd /out; openrave-robot.py robot.dae --info links"

The output should look like this, with link names dependent on the names in the URDF:

.. code-block:: bash

   name                        index parents

   ------------------------------------------

   world                       0

   base_link                   1     world

   base                        2     base_link

   shoulder_link               3     base_link

   upper_arm_link              4     shoulder_link

   forearm_link                5     upper_arm_link

   wrist_1_link                6     forearm_link

   wrist_2_link                7     wrist_1_link

   wrist_3_link                8     wrist_2_link

   tool0                       9    wrist_3_link

   ee_link                     10    tool0

   tool_control_point          11    ee_link

   ------------------------------------------

   name                        index parents

Run the IKFast Plugin Generator
===============================

Now we have enough information to compose the command to the ikfast generator script in the docker container:

.. code-block:: bash

   docker run --rm --env PYTHONPATH=/usr/local/lib/python2.7/dist-packages -v /path/to/dir:/out hamzamerzic/openrave /bin/bash -c "cd /out; python /usr/local/lib/python2.7/dist-packages/openravepy/_openravepy_/ikfast.py --robot=robot.dae --iktype=transform6d --baselink=1 --eelink=9 --savefile=robot_ikfast.cpp"

More information about the arguments for this script can be found in the [openravepy documentation](http://openrave.org/docs/0.8.2/openravepy/ikfast/). The important ones here are:


.. code-block:: bash

    --robot: The name of the robot's .dae file in the path mapped to the container's /out directory.

    --iktype: The inverse kinematics model used to solve the kinematic chain. Since we have a 6-dof robot the transform6d solver is most appropriate, but others are available for different cases.

    --baselink: The index of the robot's base link, from the table generated in the previous step.

    --eelink: The index of the robot's end effector link, also from the table. We usually set this to correspond to the tool0 link, since the transform to the TCP frame can be set outside the ikfast solver.

    --savefile: The filename for the output .cpp file.

Another potentially-important argument not used here is --freeindex. If your robot has more than 6 DOFs, such as the 7-axis Kuka iiwa7 or a 6-DOF robot mounted on a rail, you'll need to pick a link that will have its position explicitly set prior to solving inverse kinematics. In the example above if I wanted to set the wrist_3_link to be a free axis I would add the argument --freeindex=8. I haven't tried this personally yet but I think that multiple free indices can be specified using, for example, --freeindex=[7,8].

This command might take a while to run (up to about 20 minutes) depending on the arrangement of the kinematic chain, and will produce a lot of output. The result will be a several-tens-of-thousand-LOC C++ source file containing the ikfast kinematic solver plugin for your robot.

Create Tesseract IKFast Solver
==============================

Header file:
------------

.. code-block:: c++

   #include <Eigen/Geometry>
   #include <vector>
   #include <tesseract_kinematics/ikfast/ikfast_inv_kin.h>

   namespace fanuc_p50ib_15_ikfast_wrapper
   {
   class FanucP50iBInvKinematics : public tesseract_kinematics::IKFastInvKin
   {
   public:
     FanucP50iBInvKinematics(const std::string base_link_name,
                             const std::string tip_link_name,
                             const std::vector<std::string> joint_names,
                             const std::string name)
   };


Source file:
------------

The order of the includes matter.

.. code-block:: c++

   #include <tesseract_kinematics/ikfast/impl/ikfast_inv_kin.hpp>
   #include <fanuc_p50ib_15_ikfast_wrapper/impl/fanuc_p50ib_15_ikfast.hpp>
   #include <fanuc_p50ib_15_ikfast_wrapper/tesseract_fanuc_p50ib_kinematics.h>

   namespace fanuc_p50ib_15_ikfast_wrapper
   {
     FanucP50iBInvKinematics::FanucP50iBInvKinematics(const std::string base_link_name,
                                                      const std::string tip_link_name,
                                                      const std::vector<std::string> joint_names
                                                      const std::string name)
     : FanucP50iBInvKinematics(base_link_name, tip_link_name, joint_names, name, joint_limits)
     {}
   }

Create Tesseract IKFast Plugin
==============================

Header file:
------------

.. code-block:: c++

   #include <tesseract_kinematics/core/kinematics_plugin_factory.h>
   namespace fanuc_p50ib_15_ikfast_wrapper
   {
     class FanucP50iBInvKinFactory : public tesseract_kinematics::InvKinFactory
     {
       tesseract_kinematics::InverseKinematics::UPtr create(const std::string& solver_name,
                                                            const tesseract_scene_graph::SceneGraph& scene_graph,
                                                            const tesseract_scene_graph::SceneState& scene_state,
                                                            const tesseract_kinematics::KinematicsPluginFactory& plugin_factory,
                                                            const YAML::Node& config) const override final;
     };
   }

Source file:
------------

.. code-block:: c++

   #include <fanuc_p50ib_15_ikfast_wrapper/tesseract_fanuc_p50ib_kinematics.h>
   #include <fanuc_p50ib_15_ikfast_wrapper/tesseract_fanuc_p50ib_factory.h>

   namespace fanuc_p50ib_15_ikfast_wrapper
   {
     tesseract_kinematics::InverseKinematics::UPtr
     FanucP50iBInvKinFactory::create(const std::string& solver_name,
                                     const tesseract_scene_graph::SceneGraph& scene_graph,
                                     const tesseract_scene_graph::SceneState& /*scene_state*/,
                                     const tesseract_kinematics::KinematicsPluginFactory& /*plugin_factory*/,
                                     const YAML::Node& config) const
     {
       std::string base_link;
       std::string tip_link;

       try
       {
         if (YAML::Node n = config["base_link"])
           base_link = n.as<std::string>();
         else
           throw std::runtime_error("KDLInvKinChainLMAFactory, missing 'base_link' entry");

         if (YAML::Node n = config["tip_link"])
           tip_link = n.as<std::string>();
         else
           throw std::runtime_error("KDLInvKinChainLMAFactory, missing 'tip_link' entry");
       }
       catch (const std::exception& e)
       {
         CONSOLE_BRIDGE_logError("KDLInvKinChainLMAFactory: Failed to parse yaml config data! Details: %s", e.what());
         return nullptr;
       }

       auto shortest_path = scene_graph.getShortestPath(base_link, tip_link);
       return std::make_unique<FanucP50iBInvKinematics>(base_link, tip_link, shortest_path.active_joint_names);
     }
   }

   TESSERACT_ADD_PLUGIN(fanuc_p50ib_15_ikfast_wrapper::FanucP50iBInvKinFactory, FanucP50iBInvKinFactory);

Add Additional items to CMakeLists.txt file:
--------------------------------------------

.. code-block:: cmake

   find_package(tesseract_kinematics REQUIRED)
   find_package(Eigen3 REQUIRED)
   find_package(LAPACK REQUIRED) # Requried for ikfast

   add_library(${PROJECT_NAME} src/tesseract_fanuc_p50ib_kinematics.cpp )
   target_link_libraries(${PROJECT_NAME} PUBLIC tesseract::tesseract_kinematics_ikfast console_bridge Eigen3::Eigen ${LAPACK_LIBRARIES})
   target_include_directories(${PROJECT_NAME} PUBLIC
       "$<BUILD_INTERFACE:${CMAKE_CURRENT_SOURCE_DIR}/include>"
       "$<INSTALL_INTERFACE:include>")
   target_include_directories(${PROJECT_NAME} SYSTEM PUBLIC
       ${LAPACK_INCLUDE_DIRS})

   add_library(${PROJECT_NAME}_factory src/tesseract_fanuc_p50ib_factory.cpp )
   target_link_libraries(${PROJECT_NAME}_factory PUBLIC ${PROJECT_NAME} tesseract::tesseract_kinematics_core)
   target_include_directories(${PROJECT_NAME}_factory PUBLIC
       "$<BUILD_INTERFACE:${CMAKE_CURRENT_SOURCE_DIR}/include>"
       "$<INSTALL_INTERFACE:include>")


Add Tesseract IKFast Plugin Config
==================================

Bellow shows an example kinematic plugin config file using the IKFast plugin.

.. code-block:: yaml

   kinematic_plugins:
     search_paths:
       - <path to your workspace lib directory>
     search_libraries:
       - <package_name>_factory
     fwd_kin_plugins:
       manipulator:
         default: KDLFwdKinChain
         plugins:
           KDLFwdKinChain:
             class: KDLFwdKinChainFactory
             config:
               base_link: base_link
               tip_link: tool0
     inv_kin_plugins:
       manipulator:
         default: FanucP50iBInvKinematics
         plugins:
           KDLInvKinChainLMA:
             class: FanucP50iBInvKinFactory
             config:
               base_link: base_link
               tip_link: tool0
