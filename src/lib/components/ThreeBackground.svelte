<script lang="ts">
  import { onMount, onDestroy } from 'svelte';
  import * as THREE from 'three';

  interface Props {
    isLight?: boolean;
  }

  let { isLight = false }: Props = $props();

  let container: HTMLDivElement;
  let renderer: THREE.WebGLRenderer;
  let scene: THREE.Scene;
  let camera: THREE.PerspectiveCamera;
  let animationId: number;

  $effect(() => {
    if (!renderer || !scene) return;
    const bgColor = isLight ? 0xf8fafc : 0x030303;
    renderer.setClearColor(bgColor, 1);
    if (scene.fog && 'color' in scene.fog) {
      scene.fog.color.setHex(bgColor);
    }
  });

  let mouseX = 0;
  let mouseY = 0;
  let targetMouseX = 0;
  let targetMouseY = 0;

  const PARTICLE_COUNT = 45;
  const MAX_DISTANCE = 100;

  // Arrays to hold particle data
  let particlePositions: Float32Array;
  let particleSpeeds: Float32Array;
  let pointsGeometry: THREE.BufferGeometry;
  let linesGeometry: THREE.BufferGeometry;
  let points: THREE.Points;
  let lineSegments: THREE.LineSegments;

  onMount(() => {
    if (!container) return;

    const width = window.innerWidth;
    const height = window.innerHeight;

    // 1. Scene setup
    scene = new THREE.Scene();
    const initialBgColor = isLight ? 0xf8fafc : 0x030303;
    scene.fog = new THREE.FogExp2(initialBgColor, 0.0015);

    // 2. Camera setup
    camera = new THREE.PerspectiveCamera(60, width / height, 1, 1000);
    camera.position.z = 400;

    // 3. Renderer setup
    renderer = new THREE.WebGLRenderer({ antialias: true, alpha: true, powerPreference: 'high-performance' });
    renderer.setPixelRatio(Math.min(window.devicePixelRatio, 2));
    renderer.setSize(width, height);
    renderer.setClearColor(initialBgColor, 1);
    container.appendChild(renderer.domElement);

    // 4. Initialize Particle Positions & Speeds
    particlePositions = new Float32Array(PARTICLE_COUNT * 3);
    particleSpeeds = new Float32Array(PARTICLE_COUNT * 3);

    // Spread particles in a 3D box
    const range = 600;
    for (let i = 0; i < PARTICLE_COUNT; i++) {
      particlePositions[i * 3] = (Math.random() - 0.5) * range; // x
      particlePositions[i * 3 + 1] = (Math.random() - 0.5) * range; // y
      particlePositions[i * 3 + 2] = (Math.random() - 0.5) * range; // z

      // Speeds (tuned to be extremely slow and calming)
      particleSpeeds[i * 3] = (Math.random() - 0.5) * 0.3; // vx
      particleSpeeds[i * 3 + 1] = (Math.random() - 0.5) * 0.3; // vy
      particleSpeeds[i * 3 + 2] = (Math.random() - 0.5) * 0.3; // vz
    }

    // 5. Points (Particles) Geometry & Material
    pointsGeometry = new THREE.BufferGeometry();
    pointsGeometry.setAttribute('position', new THREE.BufferAttribute(particlePositions, 3));

    // Custom Canvas Texture for nice circular glow particles
    const createCircleTexture = () => {
      const canvas = document.createElement('canvas');
      canvas.width = 16;
      canvas.height = 16;
      const ctx = canvas.getContext('2d');
      if (ctx) {
        const gradient = ctx.createRadialGradient(8, 8, 0, 8, 8, 8);
        gradient.addColorStop(0, 'rgba(6, 182, 212, 1)'); // Brand Cyan
        gradient.addColorStop(0.3, 'rgba(139, 92, 246, 0.6)'); // Purple
        gradient.addColorStop(1, 'rgba(0, 0, 0, 0)');
        ctx.fillStyle = gradient;
        ctx.fillRect(0, 0, 16, 16);
      }
      return new THREE.CanvasTexture(canvas);
    };

    const pointsMaterial = new THREE.PointsMaterial({
      size: 5,
      map: createCircleTexture(),
      blending: THREE.AdditiveBlending,
      transparent: true,
      depthWrite: false,
    });

    points = new THREE.Points(pointsGeometry, pointsMaterial);
    scene.add(points);

    // 6. Lines Geometry & Material
    linesGeometry = new THREE.BufferGeometry();
    const linePositions = new Float32Array(PARTICLE_COUNT * PARTICLE_COUNT * 6);
    const lineColors = new Float32Array(PARTICLE_COUNT * PARTICLE_COUNT * 6);

    linesGeometry.setAttribute('position', new THREE.BufferAttribute(linePositions, 3));
    linesGeometry.setAttribute('color', new THREE.BufferAttribute(lineColors, 3));

    const linesMaterial = new THREE.LineBasicMaterial({
      vertexColors: true,
      blending: THREE.AdditiveBlending,
      transparent: true,
      opacity: 0.15,
      depthWrite: false,
    });

    lineSegments = new THREE.LineSegments(linesGeometry, linesMaterial);
    scene.add(lineSegments);

    // Mouse Listeners
    const onMouseMove = (event: MouseEvent) => {
      targetMouseX = (event.clientX - width / 2) * 0.35;
      targetMouseY = (event.clientY - height / 2) * 0.35;
    };

    const onTouchMove = (event: TouchEvent) => {
      if (event.touches.length > 0) {
        targetMouseX = (event.touches[0].clientX - width / 2) * 0.35;
        targetMouseY = (event.touches[0].clientY - height / 2) * 0.35;
      }
    };

    const onResize = () => {
      const w = window.innerWidth;
      const h = window.innerHeight;
      camera.aspect = w / h;
      camera.updateProjectionMatrix();
      renderer.setSize(w, h);
    };

    window.addEventListener('mousemove', onMouseMove);
    window.addEventListener('touchmove', onTouchMove);
    window.addEventListener('resize', onResize);

    // 7. Animation Loop
    const animate = () => {
      animationId = requestAnimationFrame(animate);

      // Smooth mouse tracking
      mouseX += (targetMouseX - mouseX) * 0.05;
      mouseY += (targetMouseY - mouseY) * 0.05;

      // Rotate camera slightly based on mouse
      camera.position.x += (mouseX - camera.position.x) * 0.03;
      camera.position.y += (-mouseY - camera.position.y) * 0.03;
      camera.lookAt(scene.position);

      const positions = pointsGeometry.attributes.position.array as Float32Array;
      const linePositionsArray = linesGeometry.attributes.position.array as Float32Array;
      const lineColorsArray = linesGeometry.attributes.color.array as Float32Array;

      let lineCount = 0;
      const range = 600;

      // Update particle positions
      for (let i = 0; i < PARTICLE_COUNT; i++) {
        const i3 = i * 3;

        // Apply velocities
        positions[i3] += particleSpeeds[i3]; // x
        positions[i3 + 1] += particleSpeeds[i3 + 1]; // y
        positions[i3 + 2] += particleSpeeds[i3 + 2]; // z

        // Bounce off walls
        const boundary = range / 2;
        if (positions[i3] < -boundary || positions[i3] > boundary) particleSpeeds[i3] *= -1;
        if (positions[i3 + 1] < -boundary || positions[i3 + 1] > boundary) particleSpeeds[i3 + 1] *= -1;
        if (positions[i3 + 2] < -boundary || positions[i3 + 2] > boundary) particleSpeeds[i3 + 2] *= -1;

        // Pull toward mouse slightly (gravitational feel)
        const dx = mouseX - positions[i3];
        const dy = -mouseY - positions[i3 + 1];
        const distToMouse = Math.sqrt(dx * dx + dy * dy);
        if (distToMouse < 250) {
          positions[i3] += dx * 0.002;
          positions[i3 + 1] += dy * 0.002;
        }

        // Draw connections
        for (let j = i + 1; j < PARTICLE_COUNT; j++) {
          const j3 = j * 3;
          const dx = positions[i3] - positions[j3];
          const dy = positions[i3 + 1] - positions[j3 + 1];
          const dz = positions[i3 + 2] - positions[j3 + 2];
          const dist = Math.sqrt(dx * dx + dy * dy + dz * dz);

          if (dist < MAX_DISTANCE) {
            const alpha = 1.0 - dist / MAX_DISTANCE;

            // Add line points (2 vertices per segment)
            const lIdx = lineCount * 6;

            linePositionsArray[lIdx] = positions[i3];
            linePositionsArray[lIdx + 1] = positions[i3 + 1];
            linePositionsArray[lIdx + 2] = positions[i3 + 2];

            linePositionsArray[lIdx + 3] = positions[j3];
            linePositionsArray[lIdx + 4] = positions[j3 + 1];
            linePositionsArray[lIdx + 5] = positions[j3 + 2];

            // Cybernetic color gradient between Cyan and Violet
            // Node i color
            lineColorsArray[lIdx] = 0.024; // r (Cyanish)
            lineColorsArray[lIdx + 1] = 0.71; // g
            lineColorsArray[lIdx + 2] = 0.84 * alpha; // b
            
            // Node j color
            lineColorsArray[lIdx + 3] = 0.54; // r (Violetish)
            lineColorsArray[lIdx + 4] = 0.36; // g
            lineColorsArray[lIdx + 5] = 0.96 * alpha; // b

            lineCount++;
          }
        }
      }

      pointsGeometry.attributes.position.needsUpdate = true;
      linesGeometry.attributes.position.needsUpdate = true;
      linesGeometry.attributes.color.needsUpdate = true;

      // Draw only the connected lines
      linesGeometry.setDrawRange(0, lineCount * 2);

      // Rotate scene slowly
      scene.rotation.y += 0.0002;

      renderer.render(scene, camera);
    };

    animate();

    return () => {
      // Clean up event listeners
      window.removeEventListener('mousemove', onMouseMove);
      window.removeEventListener('touchmove', onTouchMove);
      window.removeEventListener('resize', onResize);

      // Cancel animation frame
      if (animationId) cancelAnimationFrame(animationId);

      // Dispose Three.js objects
      if (renderer) {
        renderer.dispose();
        if (container && renderer.domElement) {
          container.removeChild(renderer.domElement);
        }
      }
      pointsGeometry.dispose();
      pointsMaterial.dispose();
      linesGeometry.dispose();
      linesMaterial.dispose();
    };
  });
</script>

<div bind:this={container} class="fixed inset-0 -z-20 overflow-hidden pointer-events-none"></div>
