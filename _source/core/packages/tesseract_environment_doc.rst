*****************************
Tesseract Environment Package
*****************************

This package contains the Tesseract Environment which provides functionality to add,
remove, move and modify links and joint. It also manages adding object to the contact
managers and provides the ability.

The Tesseract Environment is a manager which interface with the State Solver, Scene Graph, Contact Managers, Manipulator Manager and Command History. It provides really no additional capability other than interfacing with these components in a thread safe way and providing additional restriction. For example, the scene graph support adding just links creating disconnected graphs, though the environment restricts things such that everything must be one connected graph. Where if you add just a link it will create a joint attaching it to the base link of the scene graph. In addition if you remove a link or joint it removes all children.

Most if not all components can be cloned (performing a deep copy). This was a critical performance decision which is different from MoveIt. MoveIt leverages the statement that all const function must be thread safe. This imposes a performance issue when it comes to motion planning  requiring function to copy data. Instead Tesseract leverages the ability to clone to achieve optimal performance during motion planning and real-time execution and obstacle avoidance. It leverages cloning so process planners may allocate a cloned object per planner and thread along with allowing the object to be non const.

The Command History is another differentiating functionality. Almost every aspect of the environment can be changed live which adds additional complexity to the framework. In order to manage this capability the command pattern was leveraged to track the changes to the environment. This way all changes to the environment are tracked which is valuable in a production environment and facilitate the ability to leverage this Command History to create an exact replica. This also eliminates the need for having access to the URDF and SRDF all the time. Once the URDF and SRDF has been processed all information is stored in internal data structures (Scene Graph and Manipulator Manager) which are then stored in the Command History. This eliminates the URDF and SRDF as a critical component and are not required to leverage Tesseract.

.. figure:: /_static/tesseract_environment_diagram.png
