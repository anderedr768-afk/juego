# juego
carros
<!DOCTYPE html>
<html>
<head>
    <title>Juego 3D de Carro</title>
    <style>
        body { margin: 0; overflow: hidden; }
    </style>
</head>
<body>

<script src="https://cdn.jsdelivr.net/npm/three@0.158.0/build/three.min.js"></script>

<script>
    const scene = new THREE.Scene();
    const camera = new THREE.PerspectiveCamera(75, window.innerWidth/window.innerHeight, 0.1, 1000);
    const renderer = new THREE.WebGLRenderer();
    renderer.setSize(window.innerWidth, window.innerHeight);
    document.body.appendChild(renderer.domElement);

    // Suelo
    const groundGeometry = new THREE.PlaneGeometry(200, 200);
    const groundMaterial = new THREE.MeshBasicMaterial({ color: 0x228B22, side: THREE.DoubleSide });
    const ground = new THREE.Mesh(groundGeometry, groundMaterial);
    ground.rotation.x = Math.PI / 2;
    scene.add(ground);

    // Carro (cubo)
    const carGeometry = new THREE.BoxGeometry(2, 1, 4);
    const carMaterial = new THREE.MeshBasicMaterial({ color: 0xff0000 });
    const car = new THREE.Mesh(carGeometry, carMaterial);
    car.position.y = 0.5;
    scene.add(car);

    camera.position.set(0, 5, -10);

    let speed = 0;
    const maxSpeed = 0.5;
    const acceleration = 0.01;
    const turnSpeed = 0.03;

    const keys = {};

    document.addEventListener("keydown", (e) => keys[e.key] = true);
    document.addEventListener("keyup", (e) => keys[e.key] = false);

    function animate() {
        requestAnimationFrame(animate);

        if (keys["w"]) speed = Math.min(speed + acceleration, maxSpeed);
        if (keys["s"]) speed = Math.max(speed - acceleration, -maxSpeed);
        if (!keys["w"] && !keys["s"]) speed *= 0.98;

        if (keys["a"]) car.rotation.y += turnSpeed;
        if (keys["d"]) car.rotation.y -= turnSpeed;

        car.position.x -= Math.sin(car.rotation.y) * speed;
        car.position.z -= Math.cos(car.rotation.y) * speed;

        // Cámara sigue al carro
        camera.position.x = car.position.x - Math.sin(car.rotation.y) * 10;
        camera.position.z = car.position.z - Math.cos(car.rotation.y) * 10;
        camera.lookAt(car.position);

        renderer.render(scene, camera);
    }

    animate();
</script>

</body>
</html>
