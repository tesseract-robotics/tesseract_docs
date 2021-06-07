*********************
Tesseract SRDF Format
*********************

Background
==========
Tesseract has its own SRDF format which is similar to the one used through ROS, but includes features specific to Tesseract.


Features
========

#. Groups - Groups define collections of links and joints that are used for planning.
#. Group States - Define a state for a group by name for easy access during operation like planning.
#. Group Tool Center Points - Define a TCP for a group by name for easy access during operation like planning.
#. Optimized Inverse Kinematics - Define custom inverse kinematics to be used for a group.
  * OPW - Ortho-Parallel Basis with Spherical Wrist Inverse Kinematic Solver
  * ROP - Robot On Positioner (ROP) inverse kinematics solver
  * REP - Robot with External Positioner (REP) inverse kinematics solver
#. Collision Margins - Define default collision margin along with link pair collision margin data
#. Allowed Collisions - Define link pair allowed collisions


Example File
============

.. code-block:: xml

   <robot name="abb_irb2400" version="1.0.0">
       <group name="manipulator_chain">
           <chain base_link="base_link" tip_link="tool0"/>
       </group>

       <group name="positioner">
         <chain base_link="world" tip_link="base_link" />
       </group>

       <group name="gantry">
         <chain base_link="world" tip_link="tool0" />
       </group>

       <group name="manipulator_joints">
           <joint name="joint_1"/>
           <joint name="joint_2"/>
           <joint name="joint_3"/>
           <joint name="joint_4"/>
           <joint name="joint_5"/>
           <joint name="joint_6"/>
       </group>

       <group_state name="zeros" group="manipulator_joints">
           <joint name="joint_6" value="0"/>
           <joint name="joint_4" value="0"/>
           <joint name="joint_5" value="0"/>
           <joint name="joint_3" value="0"/>
           <joint name="joint_1" value="0"/>
           <joint name="joint_2" value="0"/>
       </group_state>

       <group_state name="zeros" group="manipulator_chain">
           <joint name="joint_6" value="0"/>
           <joint name="joint_4" value="0"/>
           <joint name="joint_5" value="0"/>
           <joint name="joint_3" value="0"/>
           <joint name="joint_1" value="0"/>
           <joint name="joint_2" value="0"/>
       </group_state>

       <group_tcps group="manipulator_chain">
           <tcp name="scanner" xyz="  0   0 0.2" wxyz="1 0 0 0"/>
       </group_tcps>

       <group_tcps group="manipulator_joints">
           <tcp name="scanner" xyz="  0   0 0.2" wxyz="1 0 0 0"/>
       </group_tcps>

       <group_opw group="manipulator_chain" a1="0.10000000000000001" a2="-0.13500000000000001" b="0" c1="0.61499999999999999" c2="0.70499999999999996" c3="0.755" c4="0.085000000000000006" offsets="0.000000 0.000000 -1.570796 0.000000 0.000000 0.000000" sign_corrections="1 1 1 1 1 1"/>

       <group_rop group="gantry" solver_name="ROPSolver1">
         <manipulator group="manipulator" ik_solver="OPWInvKin" reach="2.3"/>
         <positioner group="positioner" fk_solver="KDLFwdKin">
           <joint name="joint_axis_1" resolution="0.1"/>
         </positioner>
       </group_rop>

       <group_rep group="gantry" solver_name="REPSolver1">
         <manipulator group="manipulator" ik_solver="OPWInvKin" reach="2.3"/>
         <positioner group="positioner" fk_solver="KDLFwdKin">
           <joint name="joint_axis_1" resolution="0.1"/>
         </positioner>
       </group_rep>

       <collision_margins default_margin="0.025">
         <pair_margin link1="link_6" link2="link_5" margin="0.01"/>
         <pair_margin link1="link_5" link2="link_4" margin="0.015"/>
       </collision_margins>

       <disable_collisions link1="link_3" link2="link_5" reason="Never"/>
       <disable_collisions link1="link_3" link2="link_6" reason="Never"/>
       <disable_collisions link1="link_2" link2="link_5" reason="Never"/>
       <disable_collisions link1="link_2" link2="link_4" reason="Never"/>
       <disable_collisions link1="link_4" link2="link_6" reason="Allways"/>
       <disable_collisions link1="link_1" link2="link_5" reason="Never"/>
       <disable_collisions link1="link_3" link2="link_4" reason="Adjacent"/>
       <disable_collisions link1="link_2" link2="link_3" reason="Adjacent"/>
       <disable_collisions link1="base_link" link2="link_1" reason="Adjacent"/>
       <disable_collisions link1="link_1" link2="link_2" reason="Adjacent"/>
       <disable_collisions link1="link_1" link2="link_4" reason="Never"/>
       <disable_collisions link1="base_link" link2="link_4" reason="Never"/>
       <disable_collisions link1="link_1" link2="link_6" reason="Never"/>
       <disable_collisions link1="link_5" link2="link_6" reason="Adjacent"/>
       <disable_collisions link1="base_link" link2="link_5" reason="Never"/>
       <disable_collisions link1="link_1" link2="link_3" reason="Never"/>
       <disable_collisions link1="base_link" link2="link_2" reason="Never"/>
       <disable_collisions link1="link_2" link2="link_6" reason="Never"/>
       <disable_collisions link1="link_4" link2="link_5" reason="Adjacent"/>
       <disable_collisions link1="base_link" link2="link_6" reason="Never"/>
       <disable_collisions link1="base_link" link2="link_3" reason="Never"/>
   </robot>

Example Explanation
-------------------

Create Chain Groups
^^^^^^^^^^^^^^^^^^^

A serial chain is specified using the base link and the tip link. The tip link in a chain is the child link of the last joint in the chain. The base link in a chain is the parent link for the first joint in the chain.

.. code-block:: xml

   <group name="manipulator_chain">
       <chain base_link="base_link" tip_link="tool0"/>
   </group>

   <group name="positioner">
     <chain base_link="world" tip_link="base_link" />
   </group>

   <group name="gantry">
     <chain base_link="world" tip_link="tool0" />
   </group>


Create Joint Groups
^^^^^^^^^^^^^^^^^^^

A group can be specified as a collection of joints. All the child links of each joint are automatically included in the group.

.. code-block:: xml

   <group name="manipulator_joints">
       <joint name="joint_1"/>
       <joint name="joint_2"/>
       <joint name="joint_3"/>
       <joint name="joint_4"/>
       <joint name="joint_5"/>
       <joint name="joint_6"/>
   </group>


Create Group States
^^^^^^^^^^^^^^^^^^^

Store fixed configurations of the robot. A typical use case is in defining a HOME position for a manipulator. The configuration is stored with a string id, which can be used to recover the configuration later.

.. code-block:: xml

   <group_state name="zeros" group="manipulator_joints">
       <joint name="joint_6" value="0"/>
       <joint name="joint_4" value="0"/>
       <joint name="joint_5" value="0"/>
       <joint name="joint_3" value="0"/>
       <joint name="joint_1" value="0"/>
       <joint name="joint_2" value="0"/>
   </group_state>

   <group_state name="zeros" group="manipulator_chain">
       <joint name="joint_6" value="0"/>
       <joint name="joint_4" value="0"/>
       <joint name="joint_5" value="0"/>
       <joint name="joint_3" value="0"/>
       <joint name="joint_1" value="0"/>
       <joint name="joint_2" value="0"/>
   </group_state>


Create Group Tool Center Points
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

Store fixed tool center point definitions by string id, which can be used to recover the tool center point during operation like planning.

.. code-block:: xml

   <group_tcps group="manipulator_chain">
       <tcp name="scanner" xyz="  0   0 0.2" wxyz="1 0 0 0"/>
   </group_tcps>

   <group_tcps group="manipulator_joints">
       <tcp name="scanner" xyz="  0   0 0.2" wxyz="1 0 0 0"/>
   </group_tcps>


Add OPW Inverse Kinematics Solver
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

Add and assign a OPW inverse kinematics solver to an already defined group.

.. image:: /_static/tesseract_setup_wizard_opw_diagram.png

.. code-block:: xml

   <group_opw group="manipulator_chain" a1="0.10000000000000001" a2="-0.13500000000000001" b="0" c1="0.61499999999999999" c2="0.70499999999999996" c3="0.755" c4="0.085000000000000006" offsets="0.000000 0.000000 -1.570796 0.000000 0.000000 0.000000" sign_corrections="1 1 1 1 1 1"/>


Add ROP Inverse Kinematics Solver
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

Add and assign a Robot on Positioner (ROP) inverse kinematics solver to an already defined group. This assumes a custom invserse kinematics solver already exists for the robot and the positioner is to be sample at some resolution to find a larger set of inverse kinematic solutions.

Optional Attributes: solver_name and fk_solver

.. code-block:: xml

   <group_rop group="gantry" solver_name="REPSolver1">
     <manipulator group="manipulator" ik_solver="OPWInvKin" reach="2.3"/>
     <positioner group="positioner" fk_solver="KDLFwdKin">
       <joint name="joint_axis_1" resolution="0.1"/>
     </positioner>
   </group_rop>


Add REP Inverse Kinematics Solver
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

Add and assign a Robot with External Positioner (REP) inverse kinematics solver to an already defined group. This assumes a custom invserse kinematics solver already exists for the robot and the positioner is to be sample at some resolution to find a larger set of inverse kinematic solutions.

Optional Attributes: solver_name and fk_solver

.. code-block:: xml

   <group_rep group="gantry" solver_name="REPSolver1">
     <manipulator group="manipulator" ik_solver="OPWInvKin" reach="2.3"/>
     <positioner group="positioner" fk_solver="KDLFwdKin">
       <joint name="joint_axis_1" resolution="0.1"/>
     </positioner>
   </group_rep>


Define Collision Margin Data
^^^^^^^^^^^^^^^^^^^^^^^^^^^^

In most industrial applications a single contact margin distance is not sutable because there are objects that constantly work within close proximity. This would limit the contact distance to be smaller than desired for other links allowing all objects to operate close to one another. The Tesseract contact checkers allow for a default margin to be defined along with link pair collision margins elimating this issue. This is configurable from within the SRDF file shown below.

.. code-block:: xml

   <collision_margins default_margin="0.025">
     <pair_margin link1="link_6" link2="link_5" margin="0.01"/>
     <pair_margin link1="link_5" link2="link_4" margin="0.015"/>
   </collision_margins>


Define Allowed Collision
^^^^^^^^^^^^^^^^^^^^^^^^

Define link pairs that are allowed to be in collision with each other. This is used during contact checking to avoid checking links that are allowed to be in collision and contact data should not be calculated.

.. code-block:: xml

   <disable_collisions link1="link_3" link2="link_5" reason="Never"/>
   <disable_collisions link1="link_3" link2="link_6" reason="Never"/>
   <disable_collisions link1="link_2" link2="link_5" reason="Never"/>
   <disable_collisions link1="link_2" link2="link_4" reason="Never"/>
   <disable_collisions link1="link_4" link2="link_6" reason="Allways"/>
   <disable_collisions link1="link_1" link2="link_5" reason="Never"/>
   <disable_collisions link1="link_3" link2="link_4" reason="Adjacent"/>
   <disable_collisions link1="link_2" link2="link_3" reason="Adjacent"/>
   <disable_collisions link1="base_link" link2="link_1" reason="Adjacent"/>
   <disable_collisions link1="link_1" link2="link_2" reason="Adjacent"/>
   <disable_collisions link1="link_1" link2="link_4" reason="Never"/>
   <disable_collisions link1="base_link" link2="link_4" reason="Never"/>
   <disable_collisions link1="link_1" link2="link_6" reason="Never"/>
   <disable_collisions link1="link_5" link2="link_6" reason="Adjacent"/>
   <disable_collisions link1="base_link" link2="link_5" reason="Never"/>
   <disable_collisions link1="link_1" link2="link_3" reason="Never"/>
   <disable_collisions link1="base_link" link2="link_2" reason="Never"/>
   <disable_collisions link1="link_2" link2="link_6" reason="Never"/>
   <disable_collisions link1="link_4" link2="link_5" reason="Adjacent"/>
   <disable_collisions link1="base_link" link2="link_6" reason="Never"/>
   <disable_collisions link1="base_link" link2="link_3" reason="Never"/>
