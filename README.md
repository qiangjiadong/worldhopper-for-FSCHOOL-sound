# worldhopper
Example of CSV you need to put in the root directory

```csv
1.jpg,https://www.example.com/page1.html
2.jpg,https://www.example.com/page2.html
3.jpg,https://www.example.com/page3.html
4.jpg,https://www.example.com/page4.html
5.jpg,https://www.example.com/page5.html
```

```html
<!DOCTYPE html>
<html>
  <head>
    <meta charset="utf-8">
    <title>Worldhopper</title>
    <style>
      #canvas {
        width: 100%;
        height: 100%;
        margin: 0;
        padding: 0;
      }
      .link {
        position: absolute;
        display: flex;
        align-items: center;
        justify-content: center;
        border-radius: 50%;
        background-color: white;
        cursor: pointer;
        transform: scale(0.8);
        z-index: 1;
      }
      .link img {
        max-width: 50%;
        max-height: 50%;
      }

    </style>
  </head>
  <body>
    <canvas id="canvas"></canvas>

    <script src="https://cdnjs.cloudflare.com/ajax/libs/three.js/r128/three.min.js"></script>
    <script>
      // Create a scene, camera, and renderer
      const scene = new THREE.Scene();
      const camera = new THREE.PerspectiveCamera(75, window.innerWidth / window.innerHeight, 0.1, 1000);
      const renderer = new THREE.WebGLRenderer({canvas: document.getElementById("canvas")});
      renderer.setSize(window.innerWidth, window.innerHeight);
      camera.position.z = 20;
      
      // Create a spiral shape with trapezoid boxes
      const numBoxes = 20;
      const boxSize = 2;
      const boxHeight = 0.5;
      const spiralRadius = 5;
      const spiralHeight = 5;
      const geometry = new THREE.CylinderGeometry(boxSize, boxSize, boxHeight, 4, 1);
      const material = new THREE.MeshBasicMaterial({color: 0xffffff});
      const spiral = new THREE.Object3D();
      for (let i = 0; i < numBoxes; i++) {
        const t = i / numBoxes * Math.PI * 2;
        const x = Math.cos(t) * i / numBoxes * spiralRadius;
        const y = i / numBoxes * spiralHeight;
        const z = Math.sin(t) * i / numBoxes * spiralRadius;
        const box = new THREE.Mesh(geometry, material);
        box.position.set(x, y, z);
        box.lookAt(new THREE.Vector3());
        box.name = `Box ${i+1}`;
        spiral.add(box);
      }
      scene.add(spiral);
      
      // Load URLs and images from CSV file
      fetch("links.csv")
        .then(response => response.text())
        .then(data => {
          const links = document.createElement("div");
          links.classList.add("links");
          const rows = data.split("\n");
          for (let i = 0; i < rows.length; i++) {
            const cols = rows[i].split(",");
            if (cols.length >= 2) {
              const img = new Image();
              img.src = `images/${cols[0].trim()}`;
              const link = document.createElement("div");
              link.classList.add("link");
              link.appendChild(img);
              link.addEventListener("click", () => window.open(cols[1].trim(), "_blank"));
              links.appendChild(link);
            }
          }
          document.body.appendChild(links);
        })
        .catch(error => console.log(error));
      
      // Animate the scene
      const animate = function () {
        requestAnimationFrame(animate);
        spiral.rotation.y += 0.005;
        renderer.render(scene, camera);
      };
      animate();
      
      // Resize the canvas when the window size changes
      window.addEventListener("resize", () => {
        camera.aspect = window.innerWidth / window.innerHeight;
        camera.updateProjectionMatrix();
        renderer.setSize(window.innerWidth, window.innerHeight);
      });
    </script>
  </body>
</html>
```
