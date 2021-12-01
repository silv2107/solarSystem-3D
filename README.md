# Solar System - THREE JS

<strong>Target objective</strong>: to make a solar system using threejs, animated and authentic. 

<strong>Visual</strong>: the page will contain the sun, earth and moon. The sun will be rotating by it's own axis, while the earth and moon will be rotating by their own axis and the sun's axis. Furthermore, the moon will also be rotating around the earth.

## Steps

### 1 Boilerplate code
-   git clone https://github.com/designcourse/threejs-webpack-starter.git
-   npm install

### 2 add folders
-   in src folder create an img folder, which contains an image of the UNIVERS, downloaded from the following site www.solarsystemscope.com 
-   in the static folder create a texture folder, which contains the following images, also downloaded from the site above: 
            
            1. earth.jpg
            2. moon.jpg
            3. sun.jpg
        
### 3 HTML
body has a div, with a title inside, and a canvas tag below it, with corresponding classes:

            <body>
                <div class="container">
                    <h1>Solar system</h1>
                </div>
                <canvas class="webgl"></canvas>
            </body>

### 4 CSS
           *
            {
                margin: 0;
                padding: 0;
            }

            html,
            body
            {
                height: 100vh;
                font-family: 'Poppins';
                background-image: url("./img/univers.jpg");
                background-size: cover;
                background-position: left;
                
            }

            .webgl
            {
                position: absolute;
                top: 0;
                left: 0;
                outline: none;
            }

            .container {
                z-index: 1;
                height: 100vh;
                text-align: center;
            }

            h1 {
                font-size: 6rem;
                text-transform: uppercase;
                color: white;
### 5 Javascript

            import './style.css'
            import * as THREE from 'three'
            import { OrbitControls } from 'three/examples/jsm/controls/OrbitControls.js'
            import * as dat from 'dat.gui'

            function main() {
                // Debug
                //const gui = new dat.GUI()


                //loading
                const textureLoader = new THREE.TextureLoader
                const sunTexture = textureLoader.load('/texture/sun.jpg')
                const earthTexture = textureLoader.load('/texture/earth.jpg')
                const moonTexture = textureLoader.load('/texture/moon1.jpg')


                const canvas = document.querySelector('.webgl');
                const renderer = new THREE.WebGLRenderer({
                canvas: canvas,
                alpha: true
                });
            
                const fov = 40;
                const aspect = 2;  // the canvas default
                const near = 0.1;
                const far = 1000;
                const camera = new THREE.PerspectiveCamera(fov, aspect, near, far);
                camera.position.set(0, 0, 48)
                camera.rotation.z = .3;
                
            
                const scene = new THREE.Scene();
            
                {
                const color = 0xff8000;
                const intensity = 1;
                const light = new THREE.PointLight(color, intensity);
                scene.add(light);
                }
            
                // an array of objects who's rotation to update
                const objects = [];
                //const objectMoon = [];
            
                const radius = 1;
                const widthSegments = 156;
                const heightSegments = 156;
                const sphereGeometry = new THREE.SphereGeometry(
                    radius, widthSegments, heightSegments);

                const solarSystem = new THREE.Object3D();
                scene.add(solarSystem);
                objects.push(solarSystem);

                const moonOrbit = new THREE.Object3D();
                moonOrbit.position.x = 20
                solarSystem.add(moonOrbit)
                objects.push(moonOrbit);
            
                //sunMesh
                const sunMaterial = new THREE.MeshPhongMaterial();
                sunMaterial.map = sunTexture
                const sunMesh = new THREE.Mesh(sphereGeometry, sunMaterial);
                sunMesh.scale.set(9, 9, 9);
                solarSystem.add(sunMesh);
                objects.push(sunMesh);

                //earthMesh
                const earthMaterial = new THREE.MeshPhongMaterial();
                earthMaterial.map = earthTexture
                const earthMesh = new THREE.Mesh(sphereGeometry, earthMaterial);
                earthMesh.scale.set(3, 3, 3);
                earthMesh.position.x = 20
                solarSystem.add(earthMesh);
                objects.push(earthMesh);
                
                //moonMesh
                const moonMaterial = new THREE.MeshPhongMaterial();
                moonMaterial.map = moonTexture
                const moonMesh = new THREE.Mesh(sphereGeometry, moonMaterial);
                moonMesh.scale.set(1, 1, 1);
                moonMesh.position.x = 5
                moonOrbit.add(moonMesh);
                objects.push(moonMesh);


                //light
                const ambient = new THREE.AmbientLight(0xffffff, 1);
                scene.add(ambient);
            
                //Sizes
                const sizes = {
                    width: window.innerWidth ,
                    height: window.innerHeight 
                }
                
                window.addEventListener('resize', () =>
                {
                    // Update sizes
                    sizes.width = window.innerWidth
                    sizes.height = window.innerHeight
                
                    // Update camera
                    camera.aspect = sizes.width / sizes.height
                    camera.updateProjectionMatrix()

                    renderer.setSize(sizes.width, sizes.height)
                    renderer.setPixelRatio(Math.min(window.devicePixelRatio, 2))
                })

                
                    // Update renderer
                renderer.setSize(sizes.width, sizes.height)
                renderer.setPixelRatio(Math.min(window.devicePixelRatio, 2))
            
                function render(time) {
                time *= 0.0005;
            
                objects.forEach((obj) => {
                    obj.rotation.y = time;
                });
                
                moonOrbit.rotation.y = time * 4;
                renderer.render(scene, camera);
            
                requestAnimationFrame(render);
                }
            
                requestAnimationFrame(render);
            }
            
            main();

### 6 Structure of the solarsystem
![Structure](/structure.jpg)  

### 7 Sphere Geometry
All three planets are defined by the following sphere threejs code:

        const radius = 1;
            const widthSegments = 156;
            const heightSegments = 156;
            const sphereGeometry = new THREE.SphereGeometry(
                radius, widthSegments, heightSegments);

### 8 Camera settings
        const fov = 40;
            const aspect = 2;  // the canvas default
            const near = 0.1;
            const far = 1000;
            const camera = new THREE.PerspectiveCamera(fov, aspect, near, far);
            camera.position.set(0, 0, 48)
            camera.rotation.z = .3;

### 9 Background
The background image is the univers image found in the img folder

### 10 Object array
All planets and solarsystem are added to an empty array variable, named objects:
const object =  [];

### 11 Planets, solarsystem and moonorbit
        //solarsystem variable is defined and added to the scene and pushed to the objects:
        const solarSystem = new THREE.Object3D();
                        scene.add(solarSystem);
                        objects.push(solarSystem);

        //moonorbit variable is defined and added to the solarsystem and pushed to the objects:
        const moonOrbit = new THREE.Object3D();
        moonOrbit.position.x = 20
        solarSystem.add(moonOrbit)
        objects.push(moonOrbit);

        //sunMesh
        const sunMaterial = new THREE.MeshPhongMaterial();
        sunMaterial.map = sunTexture
        const sunMesh = new THREE.Mesh(sphereGeometry, sunMaterial);
        sunMesh.scale.set(9, 9, 9);
        solarSystem.add(sunMesh);
        objects.push(sunMesh);

        //earthMesh
        const earthMaterial = new THREE.MeshPhongMaterial();
        earthMaterial.map = earthTexture
        const earthMesh = new THREE.Mesh(sphereGeometry, earthMaterial);
        earthMesh.scale.set(3, 3, 3);
        earthMesh.position.x = 20
        solarSystem.add(earthMesh);
        objects.push(earthMesh);

        //moonMesh 
        const moonMaterial = new THREE.MeshPhongMaterial();
        moonMaterial.map = moonTexture
        const moonMesh = new THREE.Mesh(sphereGeometry, moonMaterial);
        moonMesh.scale.set(1, 1, 1);
        moonMesh.position.x = 5
        moonOrbit.add(moonMesh);
        objects.push(moonMesh);

### 12 Lighting
        //Ambient Light is added to the scene
        const ambient = new THREE.AmbientLight(0xffffff, 1);
            scene.add(ambient);

### 13 Rotation
        // Rotation is set in two places:
        //-   objects variable to rotate at *time
        //-   moonorbit variable to rotate at 2*time
        
        function render(time) {
            time *= 0.0005;
        
            objects.forEach((obj) => {
                obj.rotation.y = time;
            });
            
            moonOrbit.rotation.y = time * 4;
            renderer.render(scene, camera);
        
            requestAnimationFrame(render);
            }



            




