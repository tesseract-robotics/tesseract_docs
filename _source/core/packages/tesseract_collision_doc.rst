***************************
Tesseract Collision Package
***************************

Background
==========
This package is used for performing both discrete and continuous collision checking. It
understands nothing about connectivity of the object within. It purely allows for the
user to add objects to the checker, set object transforms, enable/disable objects, set
contact distance per object, and perform collision checks.

.. Note::

   The contact managers are load as plugins through a yaml config file which is added to the SRDF file.

.. image:: /_static/continuous_first.gif

Contact Manager Plugin Config
=============================

The contact manager config file has four sections:

==================  ===========
Section             Description
==================  ===========
search_paths        A list of locations to search for libraries. It searches in the order provided.
search_libraries    A list of libraries to search for classes
discrete_plugins    The discrete contact manater plugins plugins.
continuous_plugins  The continuous contact manater plugins plugins.
==================  ===========

Example Config file:

.. code-block:: yaml

   contact_manager_plugins:
     search_paths:
       - /usr/local/lib
     search_libraries:
       - tesseract_collision_bullet_factories
       - tesseract_collision_fcl_factories
     discrete_plugins:
       default: BulletDiscreteBVHManager
       plugins:
         BulletDiscreteBVHManager:
           class: BulletDiscreteBVHManagerFactory
         BulletDiscreteSimpleManager:
           class: BulletDiscreteSimpleManagerFactory
         FCLDiscreteBVHManager:
           class: FCLDiscreteBVHManagerFactory
     continuous_plugins:
       default: BulletCastBVHManager
       plugins:
         BulletCastBVHManager:
           class: BulletCastBVHManagerFactory
         BulletCastSimpleManager:
           class: BulletCastSimpleManagerFactory


Features
========

.. include:: /_source/core/packages/tesseract_collision_features_doc.rst

Contact Test Types
==================

.. include:: /_source/core/packages/tesseract_collision_contact_test_types_doc.rst

Available Contact Checkers
==========================

.. include:: /_source/core/packages/tesseract_collision_available_contact_checkers_doc.rst

Discrete Collision Checker Example
==================================

.. rli:: https://raw.githubusercontent.com/tesseract-robotics/tesseract/master/tesseract_collision/examples/box_box_example.cpp
   :language: c++

You can find this example `here <https://github.com/tesseract-robotics/tesseract/blob/master/tesseract_collision/examples/box_box_example.cpp>`_.

Example Explanation
-------------------

Create Contact Checker
^^^^^^^^^^^^^^^^^^^^^^

.. rli:: https://raw.githubusercontent.com/tesseract-robotics/tesseract/master/tesseract_collision/examples/box_box_example.cpp
   :language: c++
   :start-after: // documentation:start:1:
   :end-before: // documentation:end:1:

Add Collision Objects to Contact Checker
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

Add a collision object in a enabled state
"""""""""""""""""""""""""""""""""""""""""

.. rli:: https://raw.githubusercontent.com/tesseract-robotics/tesseract/master/tesseract_collision/examples/box_box_example.cpp
   :language: c++
   :start-after: // documentation:start:2:
   :end-before: // documentation:end:2:

.. Note::
   A collision object can consist of multiple collision shapes.

Add collision object in a disabled state
""""""""""""""""""""""""""""""""""""""""

.. rli:: https://raw.githubusercontent.com/tesseract-robotics/tesseract/master/tesseract_collision/examples/box_box_example.cpp
   :language: c++
   :start-after: // documentation:start:3:
   :end-before: // documentation:end:3:

Create convex hull from mesh file
"""""""""""""""""""""""""""""""""

.. rli:: https://raw.githubusercontent.com/tesseract-robotics/tesseract/master/tesseract_collision/examples/box_box_example.cpp
   :language: c++
   :start-after: documentation:start:4:
   :end-before: documentation:end:4:

Add convex hull collision object
""""""""""""""""""""""""""""""""
.. rli:: https://raw.githubusercontent.com/tesseract-robotics/tesseract/master/tesseract_collision/examples/box_box_example.cpp
   :language: c++
   :start-after: documentation:start:5:
   :end-before: documentation:end:5:

Set the active collision objects
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

.. rli:: https://raw.githubusercontent.com/tesseract-robotics/tesseract/master/tesseract_collision/examples/box_box_example.cpp
   :language: c++
   :start-after: documentation:start:6:
   :end-before: documentation:end:6:

Set the contact distance threshold
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

.. rli:: https://raw.githubusercontent.com/tesseract-robotics/tesseract/master/tesseract_collision/examples/box_box_example.cpp
   :language: c++
   :start-after: documentation:start:7:
   :end-before: documentation:end:7:

Set the collision object's transform
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

.. rli:: https://raw.githubusercontent.com/tesseract-robotics/tesseract/master/tesseract_collision/examples/box_box_example.cpp
   :language: c++
   :start-after: documentation:start:8:
   :end-before: documentation:end:8:

Perform collision check
^^^^^^^^^^^^^^^^^^^^^^^

.. Note::

   One object is inside another object

.. rli:: https://raw.githubusercontent.com/tesseract-robotics/tesseract/master/tesseract_collision/examples/box_box_example.cpp
   :language: c++
   :start-after: documentation:start:9:
   :end-before: documentation:end:9:

Set the collision object's transform
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

.. rli:: https://raw.githubusercontent.com/tesseract-robotics/tesseract/master/tesseract_collision/examples/box_box_example.cpp
   :language: c++
   :start-after: documentation:start:10:
   :end-before: documentation:end:10:

Perform collision check
^^^^^^^^^^^^^^^^^^^^^^^

.. Note::

   The objects are outside the contact threshold

.. rli:: https://raw.githubusercontent.com/tesseract-robotics/tesseract/master/tesseract_collision/examples/box_box_example.cpp
   :language: c++
   :start-after: documentation:start:11:
   :end-before: documentation:end:11:

Change contact distance threshold
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

.. rli:: https://raw.githubusercontent.com/tesseract-robotics/tesseract/master/tesseract_collision/examples/box_box_example.cpp
   :language: c++
   :start-after: // documentation:start:12:
   :end-before: // documentation:end:12:


Perform collision check
^^^^^^^^^^^^^^^^^^^^^^^

.. Note::

   The objects are inside the contact threshold

.. rli:: https://raw.githubusercontent.com/tesseract-robotics/tesseract/master/tesseract_collision/examples/box_box_example.cpp
   :language: c++
   :start-after: // documentation:start:13:
   :end-before: // documentation:end:13:

Running the Example
-------------------

Build the Tesseract Workspace: ::

  catkin build

Navigate to the build folder containing the executable: ::

  cd build/tesseract_collision/examples

Run the executable: ::

  ./tesseract_collision_box_box_example
