SRDF
====

The SRDF or Semantic Robot Description Format complement the URDF and specifies joint groups, default robot configurations, additional collision checking information, and additional transforms that may be needed to completely specify the robot's pose. The recommended way to generate a SRDF is using the Tesseract Setup Assistant.

Groups
^^^^^^
Groups (also referred to as kinematics groups) define collections of links and joints that are used for planning. Groups can be specified in several ways:

Collection of Joints
""""""""""""""""""""
A group can be specified as a collection of joints. All the child links of each joint are automatically included in the group.

Serial Chain
""""""""""""
A serial chain is specified using the base link and the tip link. The tip link in a chain is the child link of the last joint in the chain. The base link in a chain is the parent link for the first joint in the chain.

Group States
^^^^^^^^^^^^
The SRDF can also store fixed configurations of the robot. A typical example of the SRDF in this case is in defining a HOME position for a manipulator. The configuration is stored with a string id, which can be used to recover the configuration later.

Group Tool Center Points
^^^^^^^^^^^^^^^^^^^^^^^^
Store fixed tool center point definitions by string id, which can be used to recover the tool center point during operation like planning.

Add OPW Inverse Kinematics Solver
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
Add and assign a OPW inverse kinematics solver to an already defined group.

Add ROP Inverse Kinematics Solver
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
Add and assign a Robot on Positioner (ROP) inverse kinematics solver to an already defined group. This assumes a custom invserse kinematics solver already exists for the robot and the positioner is to be sample at some resolution to find a larger set of inverse kinematic solutions.

Add REP Inverse Kinematics Solver
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
Add and assign a Robot with External Positioner (REP) inverse kinematics solver to an already defined group. This assumes a custom invserse kinematics solver already exists for the robot and the positioner is to be sample at some resolution to find a larger set of inverse kinematic solutions.

Define Collision Margin Data
^^^^^^^^^^^^^^^^^^^^^^^^^^^^
In most industrial applications a single contact margin distance is not sutable because there are objects that constantly work within close proximity. This would limit the contact distance to be smaller than desired for other links allowing all objects to operate close to one another. The Tesseract contact checkers allow for a default margin to be defined along with link pair collision margins elimating this issue. This is configurable from within the SRDF file shown below.

Allowed Collision Matrix
^^^^^^^^^^^^^^^^^^^^^^^^
Define link pairs that are allowed to be in collision with each other. This is used during contact checking to avoid checking links that are allowed to be in collision and contact data should not be calculated.

SRDF Documentation
^^^^^^^^^^^^^^^^^^
For information about the syntax for the Tesseract SRDF, read more details on the `Tesseract SRDF Package page <../core/packages/tesseract_srdf_doc.html>`_.
