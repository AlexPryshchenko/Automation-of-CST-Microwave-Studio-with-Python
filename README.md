# __Automation of CST Microwave Studio with Python__
In this project i'll show you how to control CST Microwave Studio with Python using CST API 

## __Requirments:__
---

* Python 3.7  
* Pywin32 227
* Numpy 
* Matplotlib
* CST Microwave Studio 2021 

## __Files:__
---

 __CST_automation.ipynb__ - Jupyter Notebook file which demonstrates how to control CST. 

__test_inhomogeneities.cst__ - testing CST project for automation.


## __Objective:__
---

We will control a CST on the example of creating a randomly inhomogeneous volumes. Such problem can be interesting for the researchers who deal with the simulation of any kind of randomly inhomogeneous materials, structures and substances like soil, concrete, water, rocks, glaciers, and so on.

But also using this approach you can automatically create strongly ordered models with a big number of elements too. It can be antenna arrays, brick walls, stairways, paving stone ect. Actually with this approach you can automate any kind of problem. 

My task in this project is to create a model of soil, which contains an inhomogeneities of different sizes and permittivity in a random positions in investigated volume. Such model of ground will bring my UWB GPR (Ultrawideband Grond Penetrating Radar) simulations closer to real conditions of subsurface survey. 

## __Overview:__
---

My simulation problem looks like this:

![investigated volume](https://github.com/AlexPryshchenko/Automation-of-CST-Microwave-Studio-with-Python/blob/main/readme_files/investigated_volume.png)

We have air (epsilon = 1) and grond (epsilon = 9) volumes. The PMN-4 mine (1 - metal detonation mechanism, 2 - explosive material, 3 - bakelite body) is hidden on some depth in the ground. Such statement of the problem corresponds to the conditions of humanitarian demining. In this figure we have ideal homogeneous volume, which does not correspond to real ground substance. 

To bring it closer to reality, we randomly fill it with inhomogeneities of different sizes and permittivity:

![gif1](https://github.com/AlexPryshchenko/Automation-of-CST-Microwave-Studio-with-Python/blob/main/readme_files/filling.gif)

Colors of spheres corresponds to a values of permittivity:

![viridis](https://github.com/AlexPryshchenko/Automation-of-CST-Microwave-Studio-with-Python/blob/main/readme_files/colorbar.png)

>In __CST_automation.ipynb__ you can see how to fill volume with any object, not just sphere.

Also you can define a volume boundaries for a random filling, number of elements, a range of its sizes and permittivity. You can fit it to your project.

The next step is to resolve the problem of overlapping objects. If you create a big number of inclusions you will definitely obtain an intersections between generated objects, which looks like this:

![overlapping](https://github.com/AlexPryshchenko/Automation-of-CST-Microwave-Studio-with-Python/blob/main/readme_files/overlapped_fig.png)

>Unresolved overlapping will give you an inaccurate results after electromagnetic simulation.

Of course you can solve this problem manually, but if you have a big number of intersections, a build-in "Intersection Check" will solve it too long, and not always in a right way. 

So I've decided to make my own algorithm. The idea is to find a distande between centers of objects in a loop, and if this distance will be smaller than the double maximum radius (or a double radius of maximun circumscribed sphere for any object, like mine) there is an overlapping and the CST "boolean insert" will be done:  

![resolve overlapping](https://github.com/AlexPryshchenko/Automation-of-CST-Microwave-Studio-with-Python/blob/main/readme_files/resolved_overlapping.png)

>Distance is calculating by well-known expression from geometry: 
>>![distance](readme_files/https://github.com/AlexPryshchenko/Automation-of-CST-Microwave-Studio-with-Python/blob/main/readme_files/distance.png)

Of course this method is not perfect and compels to insert smaller non-intersected spheres, which are located closer than double maximun radius, BUT:
+ Insert of non-overlapped objects does not delete them.
+ This method is much faster than total self-insert of all generated objects.
+ It is faster than built-in "Intersection Check"
+ All overlapped conflicts will be resolved

Also, using the same approach we can "clear" an important object, which is located in investigated volume. See how it looks like on the example of mine:

![resolve overlapping with mine](https://github.com/AlexPryshchenko/Automation-of-CST-Microwave-Studio-with-Python/blob/main/readme_files/pmn4_overlapping_resolve.png)

On the left figure we see a mine, covered with a randomly generated spheres. Some of them were generated in mine's volume. On the right figure you can see a result of described algorythm. Mine is "inserted" in the inhomogeneities as a result.

By the way, on the figure below you can see the elemtromagnetic influence of inhomogeneities in the ground model in comparison with a homogeneous one.

Here are time dependences received by Ultrawideband Grond Penetrating Radar:

![received signals difference](https://github.com/AlexPryshchenko/Automation-of-CST-Microwave-Studio-with-Python/blob/main/readme_files/signal_difference.png)

We see that signals are quite differents. This fact can be explained by the presence of inhomogeneities in the investigated ground volume. Thus blue curve reflects more precise approximation of real ground volume.

Download and try it :)
