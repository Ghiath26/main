# main
game
const=zoom in
const=zoomout
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Dubai City 3D</title>
    <script src="https://cdnjs.cloudflare.com/ajax/libs/three.js/r128/three.min.js"></script>
    <script src="https://cdn.jsdelivr.net/npm/three@0.128/examples/js/loaders/GLTFLoader.js"></script>
    <style>
        body { margin: 0; }
        canvas { display: block; }
    </style>
</head>
<body>
    <script>
        // Scene setup
        const scene = new THREE.Scene();
        const camera = new THREE.PerspectiveCamera(75, window.innerWidth / window.innerHeight, 0.1, 1000);
        const renderer = new THREE.WebGLRenderer({ antialias: true });
        renderer.setSize(window.innerWidth, window.innerHeight);
        renderer.shadowMap.enabled = true;
        document.body.appendChild(renderer.domElement);
        
        // Ground (desert texture)
        const textureLoader = new THREE.TextureLoader();
        const desertTexture = textureLoader.load('https://threejsfundamentals.org/threejs/resources/images/sand.jpg');
        const groundGeometry = new THREE.PlaneGeometry(500, 500);
        const groundMaterial = new THREE.MeshStandardMaterial({ map: desertTexture });
        const ground = new THREE.Mesh(groundGeometry, groundMaterial);
        ground.rotation.x = -Math.PI / 2;
        scene.add(ground);
        
        // Function to create textured buildings
        function createBuilding(x, z, height, textureURL) {
            const buildingTexture = textureLoader.load(textureURL);
            const geometry = new THREE.BoxGeometry(10, height, 10);
            const material = new THREE.MeshStandardMaterial({ map: buildingTexture });
            const building = new THREE.Mesh(geometry, material);
            building.position.set(x, height / 2, z);
            building.castShadow = true;
            scene.add(building);
        }
        
        // Adding Dubai-style skyscrapers with textures
        createBuilding(0, 0, 100, 'https://threejsfundamentals.org/threejs/resources/images/buildingTexture.jpg'); // Burj Khalifa style
        createBuilding(-30, 10, 40, 'https://threejsfundamentals.org/threejs/resources/images/buildingTexture.jpg');
        createBuilding(30, -20, 50, 'https://threejsfundamentals.org/threejs/resources/images/buildingTexture.jpg');
        createBuilding(-50, 20, 30, 'https://threejsfundamentals.org/threejs/resources/images/buildingTexture.jpg');
        
        // Creating a simple interior for one building
        function createInterior() {
            const roomGeometry = new THREE.BoxGeometry(20, 10, 20);
            const roomMaterial = new THREE.MeshStandardMaterial({ color: 0xaaaaaa, side: THREE.DoubleSide });
            const room = new THREE.Mesh(roomGeometry, roomMaterial);
            room.position.set(50, 5, 50);
            scene.add(room);
        }
        createInterior();
        
        // Adding Arabian NPCs
        const loader = new THREE.GLTFLoader();
        let npcs = [];
        
        function loadNPC(positionX, positionZ) {
            loader.load('https://cdn.jsdelivr.net/gh/KhronosGroup/glTF-Sample-Models/2.0/AnimatedHuman/glTF/AnimatedHuman.gltf', function(gltf) {
                let npc = gltf.scene;
                npc.scale.set(2, 2, 2);
                npc.position.set(positionX, 0, positionZ);
                scene.add(npc);
                npcs.push(npc);
            });
        }
        
        // Spawn multiple NPCs around the city
        loadNPC(10, 10);
        loadNPC(-20, 15);
        loadNPC(30, -10);
        loadNPC(-40, 20);
        
        // NPC Movement
        function animateNPCs() {
            npcs.forEach(npc => {
                npc.position.x += Math.sin(Date.now() * 0.001) * 0.02;
                npc.position.z += Math.cos(Date.now() * 0.001) * 0.02;
            });
        }
        
        // Airport (Muscat International Airport representation)
        function createAirport(x, z) {
            const airportGeometry = new THREE.BoxGeometry(80, 5, 200);
            const airportMaterial = new THREE.MeshStandardMaterial({ color: 0x555555 });
            const airport = new THREE.Mesh(airportGeometry, airportMaterial);
            airport.position.set(x, 2.5, z);
            scene.add(airport);
        }
        createAirport(100, -100);
        
        // Lighting
        const ambientLight = new THREE.AmbientLight(0xffffff, 0.5);
        scene.add(ambientLight);
        
        const directionalLight = new THREE.DirectionalLight(0xffffff, 1);
        directionalLight.position.set(50, 100, 50);
        directionalLight.castShadow = true;
        scene.add(directionalLight);
        
        // Camera position
        camera.position.set(0, 50, 150);
        
        // Animation loop
        function animate() {
            requestAnimationFrame(animate);
            animateNPCs();
            renderer.render(scene, camera);
        }
        animate();
    </script>
</body>
</html>




