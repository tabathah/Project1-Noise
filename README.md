# [Project 1: Noise](https://github.com/CIS700-Procedural-Graphics/Project1-Noise)

## Tabatha Hickman

I created a pseudo-noise based animated cloud.

![](./proj1 progress shots/final product.PNG)

## Basic Geometry

The cloud started off as an icosahedron geometry with high detail so that I had a lrage amount of vertices to work with. Here it appears with each vertex's RGB value set as the surface normal.

![](./proj1 progress shots/01- icosahedron, normal colors.PNG)

## Generating 3D Noise

I then worked on generating 3D noise. I started with a simple, highly patterned function that took the sine of the dot product of a vector of a given x, y, z position and another vector with random numbers. I wrote an interpolation function that, given the position of a vertex, found the eight surrounding lattice points in 3D space around the given position, found noise values for those eight points, and trilinearly interpolated them (using cosine interpolation) to get a noise value for that vertex. Fianlly, in order to make the noise attravtive and interesting, I implemented a multi-octave noise generator, which found noise values at four different octaves with different frequencies and amplitudes, giving the noise randommness withouot getting too sporadic. 

After implementing the noise generation, I applied it to my geometry by adding the noise value to each vertex's position along the surface normal. The result from this, was the following.

![](./proj1 progress shots/02- added bad noise function for vertex.PNG)

I also decided to apply the noise values to the coloring of vertices. I created a gradient of colors such that depending on the magnitude of the noise value, the color would range from yellow to white to blue, interpolating smoothly in between.

![](./proj1 progress shots/03- color gradient based on noise.PNG)

This product appeared to have two major problems: There seemed to be stratification in the geometry around the axes, creating sharp cliffs in a voxelized form rather than smooth bumps that flowed into one another. Also, despite my multi-octave noise generation, the peaks were broad and flat rather than varied and multiple. 

The second problem was because I was incorrectly applying frequency to the noise generation. I was implementing it such that higher frequency caused the noise to sample from a narrower range, whereas it should've been a wider range. Fixing the frequency provided the result below:

![](./proj1 progress shots/incorrect interpolation.PNG)

There was still a voxelized stratification occurring and now the noise seemed very jagged and not as smooth and gradual as I had hoped. Both of these problems were caused by bugs in my interpolation code. I was accidentally not providing a t value between 0 and 1, as well as switching the low and high values that were being interpolated between. Once I fixed these problems and improved my noise function to multiply the sine decribed earlier by a large number and take the fractional component of that, I got the final product from above.

## GUI Sliders

I also created a few sliders using dat.GUI that affect the brightness of colors in the cloud and the persitence of the noise, which weakens or strengthens the noise values, basically affecting amplitude. 

These are lower and higher brightnesses than the final product above:

![](./proj1 progress shots/low brightness.PNG)
![](./proj1 progress shots/high brightness.PNG)

And these are lower and higher persitences than the final product above:

![](./proj1 progress shots/low persistence.PNG)
![](./proj1 progress shots/high persistence.PNG)

## Time-based Animation

Another thing I did was animate the noise on the cloud with respect to time. I wanted the cloud to bloom like a flower and then receed on a cycle of about two seconds. I did this by multiplying the offset from the noise by some value between 0 and 1 that was calculated based on the time. I didn't like how when the offset was multiplied by 0 it was a simple sphere, so I changed the value to range from 1/7 to 1. I also multiplied the position by a small fraction of the time before a noise value was taken from it so that the noise values continually changed for each vertex, creating a more lively cloud.    






## Objective

Get comfortable with using three.js and its shader support and generate an interesting 3D, continuous surface using a multi-octave noise algorithm.

## Getting Started

1. [Install Node.js](https://nodejs.org/en/download/). Node.js is a JavaScript runtime. It basically allows you to run JavaScript when not in a browser. For our purposes, this is not necessary. The important part is that with it comes `npm`, the Node Package Manager. This allows us to easily declare and install external dependencies such as [three.js](https://threejs.org/), [dat.GUI](https://workshop.chromeexperiments.com/examples/gui/#1--Basic-Usage), and [glMatrix](http://glmatrix.net/). Some other packages we'll be using make it significantly easier to develop your code and create modules for better code reuse and clarity. These tools make it _signficantly_ easier to write code in multiple `.js` files without globally defining everything.

2. Fork and clone [this repository](https://github.com/CIS700-Procedural-Graphics/Project1-Noise).

3. In the root directory of your project, run `npm install`. This will download all of those dependencies.

4. Do either of the following (but I highly recommend the first one for reasons I will explain later).

    a. Run `npm start` and then go to `localhost:7000` in your web browser

    b. Run `npm run build` and then go open `index.html` in your web browser

    You should hopefully see the framework code with a 3D cube at the center of the screen!


## Developing Your Code
All of the JavaScript code is living inside the `src` directory. The main file that gets executed when you load the page as you may have guessed is `main.js`. Here, you can make any changes you want, import functions from other files, etc. The reason that I highly suggest you build your project with `npm start` is that doing so will start a process that watches for any changes you make to your code. If it detects anything, it'll automagically rebuild your project and then refresh your browser window for you. Wow. That's cool. If you do it the other way, you'll need to run `npm build` and then refresh your page every time you want to test something.

## Publishing Your Code
We highly suggest that you put your code on GitHub. One of the reasons we chose to make this course using JavaScript is that the Web is highly accessible and making your awesome work public and visible can be a huge benefit when you're looking to score a job or internship. To aid you in this process, running `npm run deploy` will automatically build your project and push it to `gh-pages` where it will be visible at `username.github.io/repo-name`.

## What is Actually Happening?
You can skip this part if you really want, but I highly suggest you read it.

### npm install
`npm install` will install all dependencies into a folder called `node_modules`. That's about it.

### package.json

This is the important file that `npm` looks at. In it, you can see the commands it's using for the `start`, `build`, and `deploy` scripts mentioned above. You can also see all of the dependencies the project requires. I will briefly go through what each of these is.
 - dat-gui: Gives us a nice and simple GUI for modifying variables in our program
 
 - gl-matrix: Useful library for linear algebra, much like glm

 - stats-js: Gives us a nice graph for timing things. We use it to report how long it takes to render each frame

 - three: Three.js is the main library we're using to draw stuff

 - three-orbit-controls: Handles mouse / touchscreen camera controls

 - babel-core, babel-loader, babel-preset-es2015: JavaScript is a a really fast moving language. It is constantly, constantly changing. Unfortunately, web browsers don't keep up nearly as quickly. Babel does the job of converting your code to a form that current browsers support. This allows us to use newer JavaScript features such as classes and imports without worrying about compatibility.

 - gh-pages-deploy: This is the library that automates publishing your code to Github

 - webpack: Webpack serves the role of packaging your project into a single file. Browsers don't actually support "importing" from other files, so without Webpack, to access data and functions in other files we would need to globally define EVERYTHING. This is an extremely bad idea. Webpack lets us use imports and develop code in separate files. Running `npm build` or `npm start` is what bundles all of your code together.

- webpack-dev-server: This is an extremely useful tool for development. It essentially creates a file watcher and rebuilds your project whenever you make changes. It also injects code into your page that gets notified when these changes occur so it can automatically refresh your page.

 - webpack-glsl-loader: Webpack does much more than just JavaScript. We can use it to load glsl, css, images, etc. For whatever you want to import, somebody has probably made a webpack loader for it.

### webpack.config.js

This is the configuration file in webpack. The most important part is `entry` and `output`. These define the input and output for webpack. It will start from `entry`, explore all dependencies, and package them all into `output`. Here, the `output` is `bundle.js`. If you look in `index.html`, you can see that the page is loading `bundle.js`, not `main.js`.

The other sections are just configuration settings for `webpack-dev-server` and setup for loading different types of files.

## Setting up a shader

Using the provided framework code, create a new three.js material which references a vertex and fragment shader. Look at the adamMaterial for reference. It should reference at least one uniform variable (you'll need a time variable to animate your mesh later on).

Create [an icosahedron](https://threejs.org/docs/index.html#Reference/Geometries/IcosahedronBufferGeometry), instead of the default cube geometry provided in the scene. Test your shader setup by applying the material to the icosahedron and color the mesh in the fragment shader using the normals' XYZ components as RGB.

Note that three.js automatically injects several uniform and attribute variables into your shaders by default; they are listed in the [documentation](https://threejs.org/docs/api/renderers/webgl/WebGLProgram.html) for three.js's WebGLProgram class.

## Noise Generation

In the shader, write a 3D multi-octave lattice-value noise function that takes three input parameters and generates output in a controlled range, say [0,1] or [-1, 1]. This will require the following steps. 

1. Write several (for however many octaves of noise you want) basic pseudo-random 3D noise functions (the hash-like functions we discussed in class). It's fine to reference one from the slides or elsewhere on the Internet. Again, this should just be a set of math operations, often using large prime numbers to random-looking output from three input parameters.

2. Write an interpolation function. Lerp is fine, but for better results, we suggest cosine interpolation.

3. (Optional) Write a smoothing function that will average the results of the noise value at some (x, y, z) with neighboring values, that is (x+-1, y+-1, z+-1).

4. Write an 'interpolate noise' function that takes some (x, y, z) point as input and produces a noise value for that point by interpolating the surrounding lattice values (for 3D, this means the surrounding eight 'corner' points). Use your interpolation function and pseudo-random noise generator to accomplish this.

5. Write a multi-octave noise generation function that sums multiple noise functions together, with each subsequent noise function increasing in frequency and decreasing in amplitude. You should use the interpolate noise function you wrote previously to accomplish this, as it generates a single octave of noise. The slides contain pseudocode for writing your multi-octave noise function.


## Noise Application

View your noise in action by applying it as a displacement on the surface of your icosahedron, giving your icosahedron a bumpy, cloud-like appearance. Simply take the noise value as a height, and offset the vertices along the icosahedron's surface normals. You are, of course, free to alter the way your noise perturbs your icosahedron's surface as you see fit; we are simply recommending an easy way to visualize your noise. You could even apply a couple of different noise functions to perturb your surface to make it even less spherical.

In order to animate the vertex displacement, use time as the third dimension or as some offset to the (x, y, z) input to the noise function. Pass the current time since start of program as a uniform to the shaders.

For both visual impact and debugging help, also apply color to your geometry using the noise value at each point. There are several ways to do this. For example, you might use the noise value to create UV coordinates to read from a texture (say, a simple gradient image), or just compute the color by hand by lerping between values.

## Interactivity

Using dat.GUI and the examples provided in the reference code, make some aspect of your demo an interactive variable. For example, you could add a slider to adjust the strength or scale of the noise, change the number of noise octaves, etc.

## For the overachievers (extra credit)

- More interactivity (easy): pretty self-explanatory. Make more aspects of your demo interactive by adding more controlable variables in the GUI.

- Custom mesh (easy): Figure out how to import a custom mesh rather than using an icosahedron for a fancy-shaped cloud.

- Mouse interactivity (medium): Find out how to get the current mouse position in your scene and use it to deform your cloud, such that users can deform the cloud with their cursor.

- Music (hard): Figure out a way to use music to drive your noise animation in some way, such that your noise cloud appears to dance.

## Submission

- Update README.md to contain a solid description of your project

- Publish your project to gh-pages. `npm run deploy`. It should now be visible at http://username.github.io/repo-name

- Create a [pull request](https://help.github.com/articles/creating-a-pull-request/) to this repository, and in the comment, include a link to your published project.

- Submit the link to your pull request on Canvas.