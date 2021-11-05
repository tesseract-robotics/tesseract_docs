********
Overview
********

Tesseract is a motion planning framework developed by Southwest Research Institute for industrial automation applications focusing on quality, robustness and performance. This document serves an overview of its capabilities and for more details please refer to the wiki and API Documentation.

Before we begin, this is not a fork of MoveIt. It has been developed from the ground up but was inspired by MoveIt where we leveraged our knowledge of MoveIt to help shape the architecture to enable the necessary functionality needed in industrial automation.

Tesseract Planning Framework is currently broken out into four repositories.

    tesseract - Contain core package shown in the image below
    tesseract_planning - Contains motion and process planning packages (TrajOpt, OMPL, Descartes, etc.)
    tesseract_python - Python wrappers for tesseract and tesseract_planning repositories
    tesseract_ros - ROS wrappers exposing the capabilities of tessseract and tesseract_planning repositories


Tesseract Environment
#####################

The Tesseract Environment is a manager which interface with the State Solver, Scene Graph, Contact Managers, Manipulator Manager and Command History. It provides really no additional capability other than interfacing with these components in a thread safe way and providing additional restriction. For example, the scene graph support adding just links creating disconnected graphs, though the environment restricts things such that everything must be one connected graph. Where if you add just a link it will create a joint attaching it to the base link of the scene graph. In addition if you remove a link or joint it removes all children.

Most if not all components can be cloned (performing a deep copy). This was a critical performance decision which is different from MoveIt. MoveIt leverages the statement that all const function must be thread safe. This imposes a performance issue when it comes to motion planning  requiring function to copy data. Instead Tesseract leverages the ability to clone to achieve optimal performance during motion planning and real-time execution and obstacle avoidance. It leverages cloning so process planners may allocate a cloned object per planner and thread along with allowing the object to be non const.

The Command History is another differentiating functionality. Almost every aspect of the environment can be changed live which adds additional complexity to the framework. In order to manage this capability the command pattern was leveraged to track the changes to the environment. This way all changes to the environment are tracked which is valuable in a production environment and facilitate the ability to leverage this Command History to create an exact replica. This also eliminates the need for having access to the URDF and SRDF all the time. Once the URDF and SRDF has been processed all information is stored in internal data structures (Scene Graph and Manipulator Manager) which are then stored in the Command History. This eliminates the URDF and SRDF as a critical component and are not required to leverage Tesseract.

.. figure:: /_static/tesseract_environment_diagram.png

Tesseract Scene Graph
######################

The scene graph is used to manage connectivity of the environment leveraging boost graph and manages links and joints. It inherits from boost graph so you can leverage any of the boost graph capabilities for searching the scene graph. This should be used to gain access to link and joint information. The image below highlights some of the features provided but is not limited to what is shown.

.. figure:: /_static/tesseract_scene_graph_diagram.png

Tesseract Collision
###################

The tessseract collision package provides support for performing both discrete and continuous collision checking. It understand nothing about connectivity of the object it is managing.

.. figure:: /_static/continuous_first.gif

Features
********

.. include:: /_source/core/packages/tesseract_collision_features_doc.rst

Contact Test Types
******************

.. include:: /_source/core/packages/tesseract_collision_contact_test_types_doc.rst

Available Contact Checkers
**************************

.. include:: /_source/core/packages/tesseract_collision_available_contact_checkers_doc.rst

Benchmark Data
**************

.. include:: /_source/core/packages/tesseract_collision_benchmark_doc.rst

Tesseract Kinematics
####################

This contains an interface for both forward and inverse kinematics. One design decision made is that the kinematics are not environment aware to simplify the process of implementing both forward and inverse kinematics. It is setup to support kinematic chains, trees and graphs but currently only has implementations for chains and trees.

It also includes specialized support for the following:
- IKFast generated inverse kinematics
- OPW Kinematics
- Robot on a positioner
- Robot with an external positioner
- Universal Robot

The kinematics are all loaded as plugins which can be defined in a yaml file and added to the SRDF.



Building
########

Build and Run All
*****************

Run the following command: ::

    catkin build --force-cmake -DTESSERACT_ENABLE_TESTS=ON

Build and Run One Package
*************************

Run the following command: ::

    catkin build pkg_name --force-cmake -DTESSERACT_ENABLE_TESTS=ON

Make Output Verbose
*******************

Run the following command: ::

    catkin build pkg_name --v --force-cmake -DTESSERACT_ENABLE_TESTS=ON

Building with Clang-Tidy
************************

.. note:: Clang-tidy is automatically enabled if cmake argument -DTESSERACT_ENABLE_TESTING_ALL=ON is passed.

To build with clang-tidy, you must pass the -DTESSERACT_ENABLE_CLANG_TIDY=ON to cmake when building: ::

    catkin build --force-cmake -DTESSERACT_ENABLE_CLANG_TIDY=ON
