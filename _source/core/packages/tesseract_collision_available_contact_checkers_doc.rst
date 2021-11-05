===========================  ==========  ===========
Contact Checker              Type        Description
===========================  ==========  ===========
BulletDiscreteBVHManager     Discrete    This leverages the Bullet Physics BVH to perform discrete contact checking
BulletCastBVHManager         Continuous  This leverages the Bullet Physics BVH to perform continuous contact checking by creating a casted convex hull between two link locations.
BulletDiscreteSimpleManager  Discrete    This leverages the Bullet Physics  to perform discrete contact checking. It is a naive implementation where it loops over all contact pairs and checks for contact. In relatively small environments this can be faster than leveraging the BVH implementation.
BulletCastSimpleManager      Continuous  This leverages the Bullet Physics  to perform continuous contact checking by creating a casted convex hull between two link locations.. It is a naive implementation where it loops over all contact pairs and checks for contact. In relatively small environments this can be faster than leveraging the BVH implementation.
FCLDiscreteBVHManager        Discrete    This leverages the Flexible Collision Library to perform discrete contact checking.
GPUDiscreteBVHManager        Discrete    This leverages an library for performing GPU/CPU contact checking
===========================  ==========  ===========
