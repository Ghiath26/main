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
    <style>
        body { margin: 0; overflow: hidden; font-family: Arial, sans-serif; }
        canvas { display: block; }
        #job-menu { position: absolute; top: 10px; left: 10px; background: rgba(0, 0, 0, 0.7); color: white; padding: 10px; border-radius: 5px; }
        #home-designer { position: absolute; top: 50px; left: 10px; background: blue; color: white; padding: 10px; border-radius: 5px; cursor: pointer; }
    </style>
</head>
<body>
    <div id="job-menu">Select a Job:<br>
        <button onclick="selectJob('Pilot')">Pilot</button>
        <button onclick="selectJob('Material Collection')">Material Collection</button>
    </div>
    <div id="home-designer" onclick="openHomeDesigner()">Home Designer</div>
    <script>
        // Scene setup
        const scene = new THREE.Scene();
        const camera = new THREE.PerspectiveCamera(75, window.innerWidth / window.innerHeight, 0.1, 5000);
        const renderer = new THREE.WebGLRenderer({ antialias: true });
        renderer.setSize(window.innerWidth, window.innerHeight);
        renderer.shadowMap.enabled = true;
        document.body.appendChild(renderer.domElement);
        
        // Loaders
        const textureLoader = new THREE.TextureLoader();
        const gltfLoader = new THREE.GLTFLoader();
        
        // Sky texture
        const skyTexture = textureLoader.load('https://threejsfundamentals.org/threejs/resources/images/sky.jpg');
        scene.background = skyTexture;
        
        // Ground (desert texture)
        const desertTexture = textureLoader.load('https://threejsfundamentals.org/threejs/resources/images/sand.jpg');
        const groundGeometry = new THREE.PlaneGeometry(5000, 5000);
        const groundMaterial = new THREE.MeshStandardMaterial({ map: desertTexture });
        const ground = new THREE.Mesh(groundGeometry, groundMaterial);
        ground.rotation.x = -Math.PI / 2;
        scene.add(ground);
        
        // AI Air traffic - Boeing 747-8 example
        function createAircraft(modelUrl, x, z) {
            gltfLoader.load(modelUrl, (gltf) => {
                const aircraft = gltf.scene;
                aircraft.position.set(x, 200, z);
                aircraft.scale.set(10, 10, 10);
                scene.add(aircraft);
            });
        }
        
        // Example Aircraft - Replace with actual URLs
        createAircraft('https://example.com/b7478.glb', 0, 0);
        
        // Load AboFlah character model
        let aboFlahCharacter;
        function createAboFlahCharacter(x, z) {
            const characterModel = 'https://example.com/aboflah.glb'; // Replace with actual model URL
            gltfLoader.load(characterModel, (gltf) => {
                aboFlahCharacter = gltf.scene;
                aboFlahCharacter.position.set(x, 0, z);
                aboFlahCharacter.scale.set(5, 5, 5);
                scene.add(aboFlahCharacter);
                camera.position.set(0, 5, 0);
                camera.lookAt(aboFlahCharacter.position);
            });
        }
        
        createAboFlahCharacter(0, 0);
        
        // Lighting
        const ambientLight = new THREE.AmbientLight(0xffffff, 0.8);
        scene.add(ambientLight);
        
        const directionalLight = new THREE.DirectionalLight(0xffffff, 1.2);
        directionalLight.position.set(200, 400, 200);
        directionalLight.castShadow = true;
        scene.add(directionalLight);
        
        // Character movement controls (First-Person View)
        const movementSpeed = 2;
        const keys = { ArrowUp: false, ArrowDown: false, ArrowLeft: false, ArrowRight: false };
        
        document.addEventListener('keydown', (event) => {
            if (keys.hasOwnProperty(event.key)) {
                keys[event.key] = true;
            }
        });
        
        document.addEventListener('keyup', (event) => {
            if (keys.hasOwnProperty(event.key)) {
                keys[event.key] = false;
            }
        });
        
        function moveCharacter() {
            if (aboFlahCharacter) {
                if (keys.ArrowUp) aboFlahCharacter.position.z -= movementSpeed;
                if (keys.ArrowDown) aboFlahCharacter.position.z += movementSpeed;
                if (keys.ArrowLeft) aboFlahCharacter.position.x -= movementSpeed;
                if (keys.ArrowRight) aboFlahCharacter.position.x += movementSpeed;
                
                camera.position.set(
                    aboFlahCharacter.position.x,
                    aboFlahCharacter.position.y + 5,
                    aboFlahCharacter.position.z
                );
                camera.lookAt(aboFlahCharacter.position);
            }
        }
        
        function selectJob(job) {
            alert('You selected: ' + job);
        }
        
        function openHomeDesigner() {
            alert('Opening Home Designer...');
        }
        
        // Animation loop
        function animate() {
            requestAnimationFrame(animate);
            moveCharacter();
            renderer.render(scene, camera);
        }
        animate();
    </script>
</body>
</html>




