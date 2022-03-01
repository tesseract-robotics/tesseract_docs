# Glass Upright example

## Code

```cpp
tesseract_environment::Command::Ptr GlassUprightExample::addSphere()
```

This method adds sphere to the environment

```cpp
bool GlassUprightExample::run()
```

The run method is invoked inside the main node, we will explain what this method is doing in detail.

```cpp
// Set the robot initial state
std::vector<std::string> joint_names;
joint_names.push_back("joint_a1");
joint_names.push_back("joint_a2");
joint_names.push_back("joint_a3");
joint_names.push_back("joint_a4");
joint_names.push_back("joint_a5");
joint_names.push_back("joint_a6");
joint_names.push_back("joint_a7");

Eigen::VectorXd joint_start_pos(7);
joint_start_pos(0) = -0.4;
joint_start_pos(1) = 0.2762;
joint_start_pos(2) = 0.0;
joint_start_pos(3) = -1.3348;
joint_start_pos(4) = 0.0;
joint_start_pos(5) = 1.4959;
joint_start_pos(6) = 0.0;

Eigen::VectorXd joint_end_pos(7);
joint_end_pos(0) = 0.4;
joint_end_pos(1) = 0.2762;
joint_end_pos(2) = 0.0;
joint_end_pos(3) = -1.3348;
joint_end_pos(4) = 0.0;
joint_end_pos(5) = 1.4959;
joint_end_pos(6) = 0.0;

env_->setState(joint_names, joint_start_pos);
```

The robot initial state is set by defining the `joint_names` which is a vector carry the joint names as defined in the robot description.
`joint_start_pos` and `joint_end_pos` are vectors carry the values for the joints defined in the `Joint_names`.

The values of the `joint_start_pos` is set as the start state of the robot using the `setState` method.

```cpp
// Create Program
CompositeInstruction program("FREESPACE", CompositeInstructionOrder::ORDERED, ManipulatorInfo("manipulator"));
```

Here we created the program which is a group of instruction that will be added in the next sections.
The program takes three parameters. First is a string which is a key or a label for the program.
Second is the `CompositeInstructionOrder` it can be one of the following three values:

* ORDERED: Must go in forward
* UNORDERED: Any order is allowed
* ORDERED_AND_REVERABLE: An go forward or reverse the order

The third parameter is the `ManipulatorInfo` which here is initialized by the `manipulator` which should match the name of the group in srdf file.

Next we will add the instructions to our program. To create an instruction we need to have an a waypoint which is the point we would like to go to by this instruction.

```cpp
  // Start and End Joint Position for the program
  Waypoint wp0 = StateWaypoint(joint_names, joint_start_pos);
  Waypoint wp1 = StateWaypoint(joint_names, joint_end_pos);
```

Two waypoints were defined using the `StateWaypoint` method, which takes the names and the desired values of the joints.

```cpp
PlanInstruction start_instruction(wp0, PlanInstructionType::START);
program.setStartInstruction(start_instruction);
```

A plan instruction is created which takes the first waypoint `wp0`, set the type of the instruction as `START`.

The PlanInstructionType can be one of the following:

* LINEAR: Straight line motion to the waypoint
* FREESPACE: Free motion, similar to the point to point
* CIRCULAR: Circular motion
* START: Set the point as the start point

Then we added this point to the program as the start instruction using `setStartInstruction` method.

```cpp
// Assign a linear motion so cartesian is defined as the target
PlanInstruction plan_f0(wp1, PlanInstructionType::LINEAR, "UPRIGHT");
plan_f0.setDescription("freespace_plan");
// Add Instructions to program
program.push_back(plan_f0);
```

The second plan instruction is a linear motion to the second waypoint, Then we push back the point to the program.

```cpp
// Create Process Planning Server
ProcessPlanningServer planning_server(std::make_shared<ROSProcessEnvironmentCache>(monitor_), 5);
planning_server.loadDefaultProcessPlanners();
```

Then we create the ProcessPlanningServer which will take the program as a request and return the output trajectory as a response.

`loadDefaultProcessPlanners`: loads the default planners.

We can add some planning profiles to our planning server, each profile add some costs and constrains to our problem

```cpp
// Create TrajOpt Profile
auto trajopt_plan_profile = std::make_shared<tesseract_planning::TrajOptDefaultPlanProfile>();
trajopt_plan_profile->cartesian_coeff = Eigen::VectorXd::Constant(6, 1, 5);
trajopt_plan_profile->cartesian_coeff(0) = 0;
trajopt_plan_profile->cartesian_coeff(1) = 0;
trajopt_plan_profile->cartesian_coeff(2) = 0;
```

One of the parameters we can control using the `TrajOptDefaultPlanProfile` is the `cartesian_coeff`.
The size of cartesian_coeff should be six. The first three correspond to translation and the last three to rotation.
`The trajopt_plan_profile->cartesian_coeff(0) = 0;` indicates it is free to translate along the x-axis.
If the value is greater than zero it is considered constrained and the coefficient represents a weight/scale applied to the error and gradient.

```cpp
// Add profile to Dictionary
planning_server.getProfiles()->addProfile<tesseract_planning::TrajOptPlanProfile>("UPRIGHT", trajopt_plan_profile);
```

We add the profile to the planning server

```
// Create Process Planning Request
ProcessPlanningRequest request;
request.name = tesseract_planning::process_planner_names::TRAJOPT_PLANNER_NAME;
request.instructions = Instruction(program);

request.instructions.print("Program: ");

// Solve process plan
ProcessPlanningFuture response = planning_server.run(request);
planning_server.waitForAll();
```

We create the request and pass it to the run method to get the response.