# CSCI-4471-6671

When the program is debugged a console appears then it renders. 
Then it saves the bmp file in Yun_A01 > rayTrace > rayTrace.
 
In my program, I have implemented an image with size of 640 by 480 pixels.
•	I have implemented a savebmp function that passes colour data and saves the image that I have rendered into a bmp file. I have utilised the code segments explains in the following website: https://solarianprogrammer.com/2018/11/19/cpp-reading-writing-bmp-images/’


•	Then I created a vector header file vector.h. and the vectors are created, they consist of 3 double values, and they return the x, y, z values.
Normalize function has been implemented here, not realizing that normalize function already exists in the library so it’s redundant. But I didn’t bother removing it, since it was written up already.
Furthermore, magnitude function has been implemented, which is scalar, like the one we used in the raytracing lab work we have done.

•	Then Ray header file was created.  
Ray::Ray () {
		Origin = Vect(0,0,0);
		Direction = Vect (1,0,0);
}
This part pretty much means when there is when there is new light ray created, we don’t tell it what its origin or the direction, it specifies that the default origin of the vector has the same integer as the origin of the scene and the direction is always x direction.
•	Then Camera header file was created. It has the vectors just like what ray does, but there are 4 of them: position of the camera, direction of it, camera for right direction, and camera for downward direction.
•	Then colour header file was created. It is implemented very similarly to the vector header file. But instead of having multiple method functions this time, I implemented multiple setter functions, such as setColorRed, setColorGreen, and etc.
•	Then light header file was implemented. It is also very similar to vector. Light object requires a vector and color in its parameters. And get functions return the positions and color of the light.
The default position of the light is origin, Vect(0,0,0), and the default color has been set to white Color(1,1,1 ).
•	Then sphere header file was implemented. Sphere has 3 parameters: vector, double value for its numerical values, and colour. The default position(center) is set to origin, and the default radius is set to 1.0, which is one. When setting the colour, I have decided to use the colour that goes from 0 to 1, instead of the one that goes from 0 to 255. 
•	Then the object header file was created. I have set the object constructor to be default, without taking any parameters. However, if an object happens to be a sphere, then it gets the color of it.
findIntersection function is created to find the intersection between the ray and the sphere.  Since there are always a distance between the camera, where the ray originates, and the point of intersection. So if a ray has a direction, and it goes out on our scene, it goes infinitely in that direction, but if there is an intersection, then it stops there and terminates.
for (int aax = 0; aax < aadepth; aax++) {
			for (int aay = 0; aay < aadepth; aay++) {
			
				aa_index = aay*aadepth + aax;
					
				srand(time(0));
				
				if (aadepth == 1) {
					if (width > height) {
xamnt = ((x+0.5)/width)*aspectratio - (((width-height)/(double)height)/2);
						yamnt = ((height - y) + 0.5)/height;
					}
					else if (height > width) {
						xamnt = (x + 0.5)/ width;
						yamnt = (((height - y) + 0.5)/height)/aspectratio - (((height - width)/(double)width)/2);
					}
					else {
						xamnt = (x + 0.5)/width;
						yamnt = ((height - y) + 0.5)/height;

what it does in this part is that it all sets from the position of r direction that our camera is pointed, and create rays that go to the left of that direction and also the right, up and down, in order to cover that image plain. 

Then, the rays are created. Origin of the rays are the same as the origin of the camera. 

for (int index = 0; index < scene_objects.size(); index++) {
	intersections.push_back(scene_objects.at(index)->findIntersection(cam_ray));
}

Loop through the each of the objects in the scene that I have created, and determine if the ray that has been just created intersects with any of the objects in the scene. 

int index_of_winning_object = winningObjectIndex(intersections);

In this code segment, the value of the index of winning object has been created. This variable at each pixel, should be giving the value of -1, 0 or 1. Which would be the case where the ray of this pixel either misses everything in our scene or intersects with the first object in our scene or intersect with the second object in our scene. Therefore our of those three possibilities, this variable gives the value depending on the pixel we got. 
for (int x = 0; x < width; x++) {
		for (int y = 0; y < height; y++) {
pixels[thisone].r = avgRed;
pixels[thisone].g = avgGreen;
pixels[thisone].b = avgBlue;
}
}
what’s being done here is that it goes through every single pixels on the image then assigns colours on them.

This is how we see what our ray intersects with first, in order to determine which direction a ray goes off in, in order to have reflection. In this part,  a ray comes our of the camera, it intersects with the object in our scene, it reflects off of that object, and then it intersects with another object in our scene. If it intersects with the other object, we have a colour of our secondary object, and then that’s going to get added the colour of our first object. So it’s going to be modifying the winningObjectColor, by adding the colour by the object is being reflected off of our secondary ray, and that is going to be the what we returning to our pixel. 
if (winning_object_color.getColorSpecial() > 0 && winning_object_color.getColorSpecial() <= 1) {
		// reflection from objects with specular intensity
		double dot1 = winning_object_normal.dotProduct(intersecting_ray_direction.negative());
		Vect scalar1 = winning_object_normal.vectMult(dot1);
		Vect add1 = scalar1.vectAdd(intersecting_ray_direction);
		Vect scalar2 = add1.vectMult(2);
		Vect add2 = intersecting_ray_direction.negative().vectAdd(scalar2);
		Vect reflection_direction = add2.normalize();
		
		Ray reflection_ray (intersection_position, reflection_direction);
		
		// determine what the ray intersects with first
		vector<double> reflection_intersections;
if the ray intersects off the sphere and it is reflected, then it intersects with our plain, which is our white tile, then we are adding the white tile color to the colour of the sphere. And that’s going to give the illusion of the reflection.

Color reflection_intersection_color = getColorAt(reflection_intersection_position, reflection_intersection_ray_direction, scene_objects, index_of_winning_object_with_reflection, light_sources, accuracy, ambientlight);
				
 Using the getColorAt function, we are making another call to another getColorAt.  That’s because reflections are inherently recursive process. But then each reflections, recursive getColorAt routine has been created and thus, each reflection can go up into multiple directions and then it intersects with other objects which the ray bounces off, and heads towards other directions. Each time this is done, then I get recursive getColorAt in order to perform the reflection.

