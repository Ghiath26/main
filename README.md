# main
earth OM
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Dubai City 3D</title>
    <script src="https://cdnjs.cloudflare.com/ajax/libs/three.js/r128/three.min.js"></script>
    <script src="https://cdn.jsdelivr.net/npm/three@0.128/examples/js/loaders/GLTFLoader.js"></script>
    <script src="https://cdn.jsdelivr.net/npm/three@0.128/examples/js/controls/OrbitControls.js"></script>
    <style>
        body { margin: 0; overflow: hidden; }
        canvas { display: block; }
    </style>
</head>
<body>
    <script>
        // Scene setup
        const scene = new THREE.Scene();
        const camera = new THREE.PerspectiveCamera(75, window.innerWidth / window.innerHeight, 0.1, 100000);
        const renderer = new THREE.WebGLRenderer({ antialias: true });
        renderer.setSize(window.innerWidth, window.innerHeight);
        renderer.shadowMap.enabled = true;
        document.body.appendChild(renderer.domElement);

        // Camera controls
        const controls = new THREE.OrbitControls(camera, renderer.domElement);
        controls.enableDamping = true;

        // Load high-resolution satellite Earth texture
        const textureLoader = new THREE.TextureLoader();
        const earthTexture = textureLoader.load('https://eoimages.gsfc.nasa.gov/images/imagerecords/74000/74218/world.topo.bathy.200412.3x5400x2700.jpg');

        // Create Earth sphere
        const earthGeometry = new THREE.SphereGeometry(3000, 128, 128);
        const earthMaterial = new THREE.MeshStandardMaterial({ map: earthTexture });
        const earth = new THREE.Mesh(earthGeometry, earthMaterial);
        scene.add(earth);

        // Function to create city buildings
        function createCity(latitude, longitude, numBuildings, cityName) {
            const cityGroup = new THREE.Group();
            for (let i = 0; i < numBuildings; i++) {
                const height = Math.random() * 150 + 50;
                const x = Math.random() * 100 - 50;
                const z = Math.random() * 100 - 50;
                const buildingTexture = textureLoader.load('https://threejsfundamentals.org/threejs/resources/images/buildingTexture.jpg');
                const geometry = new THREE.BoxGeometry(20, height, 20);
                const material = new THREE.MeshStandardMaterial({ map: buildingTexture });
                const building = new THREE.Mesh(geometry, material);
                building.position.set(x, height / 2, z);
                building.castShadow = true;
                cityGroup.add(building);
            }
            
            // Convert lat/long to 3D sphere position
            const phi = (90 - latitude) * (Math.PI / 180);
            const theta = (longitude + 180) * (Math.PI / 180);
            const radius = 3000;
            
            cityGroup.position.x = radius * Math.sin(phi) * Math.cos(theta);
            cityGroup.position.y = radius * Math.cos(phi);
            cityGroup.position.z = radius * Math.sin(phi) * Math.sin(theta);
            
            earth.add(cityGroup);
        }

        // Add major world cities
        createCity(25.276987, 55.296249, 50, 'Dubai');
        createCity(40.712776, -74.005974, 100, 'New York');
        createCity(51.507351, -0.127758, 80, 'London');
        createCity(35.689487, 139.691711, 120, 'Tokyo');
        createCity(48.856613, 2.352222, 90, 'Paris');
        createCity(-33.868820, 151.209290, 70, 'Sydney');

        // Lighting
        const ambientLight = new THREE.AmbientLight(0xffffff, 0.6);
        scene.add(ambientLight);

        const directionalLight = new THREE.DirectionalLight(0xffffff, 1);
        directionalLight.position.set(50, 100, 50);
        directionalLight.castShadow = true;
        scene.add(directionalLight);

        // Camera position
        camera.position.set(0, 3500, 5000);

        // Animation loop
        function animate() {
            requestAnimationFrame(animate);
            earth.rotation.y += 0.001;
            controls.update();
            renderer.render(scene, camera);
        }
        animate();
    </script>
</body>
</html>



