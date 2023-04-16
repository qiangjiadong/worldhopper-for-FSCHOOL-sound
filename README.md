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
  <!-- Here's a brief summary of how it works:

  We create a canvas element and set its dimensions to fill the entire page.
  We use the Three.js library to create a 3D spiral shape with trapezoid boxes. The numBoxes variable controls the number of boxes in the spiral, while spiralRadius and spiralHeight control the size and shape of the spiral.
  We add each box to the spiral object and add the spiral to the scene.
  We create a set of links, where each link represents a box in the spiral. The urls array contains the URLs that we want to open when a link is clicked.
  We add each link to the page and add a click event listener to each link that opens the corresponding URL in a new window.
  We create an animate() function that updates the rotation of the spiral and renders the scene every frame using Three.js.
  We add a window resize event listener that updates the camera aspect ratio and renderer size when the window is resized. -->


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
      }
      .link img {
        max-width: 100%;
        max-height: 100%;
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
      
      // Load URLs into each box and add click event listeners
      const urls = [
        "https://www.example.com/page1.html",
        "https://www.example.com/page2.html",
        "https://www.example.com/page3.html",
        // ...and so on
      ];
      const links = document.createElement("div");
      links.classList.add("links");
      for (let i = 0; i < numBoxes; i++) {
        const link = document.createElement("div");
        link.classList.add("link");
        link.innerHTML = `<img src="https://www.example.com/image${i+1}.jpg" alt="Image ${i+1}">`;
        link.addEventListener("click", () => window.open(urls[i], "_blank"));
        links.appendChild(link);
      }
      document.body.appendChild(links);
      
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
