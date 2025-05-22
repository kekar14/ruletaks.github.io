<!DOCTYPE html>
<html lang="es">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0"/>
  <title>Ruleta Kiss o Slap</title>
  <style>
    body {
      margin: 0;
      height: 100vh;
      background: linear-gradient(135deg, #000000, #001f33);
      display: flex;
      flex-direction: column;
      align-items: center;
      justify-content: center;
      color: white;
      font-family: Arial, sans-serif;
    }

    .wheel-container {
      position: relative;
      width: 300px;
      height: 300px;
    }

    .wheel {
      width: 100%;
      height: 100%;
      border-radius: 50%;
      border: 5px solid white;
      transition: transform 4s cubic-bezier(0.33, 1, 0.68, 1);
    }

    .pointer {
  position: absolute;
  bottom: -20px; /* en vez de top */
  left: 50%;
  transform: translateX(-50%) rotate(180deg); /* lo rota hacia arriba */
  width: 0;
  height: 0;
  border-left: 20px solid transparent;
  border-right: 20px solid transparent;
  border-top: 30px solid red; /* cambiamos border-bottom -> border-top */
  z-index: 2;
    }

    #spin {
      margin-top: 30px;
      padding: 12px 30px;
      font-size: 18px;
      background-color: #0055aa;
      border: none;
      border-radius: 5px;
      color: white;
      cursor: pointer;
      transition: background 0.3s;
    }

    #spin:hover {
      background-color: #0077dd;
    }
  </style>
</head>
<body>

<div class="wheel-container">
  <canvas id="wheel" width="300" height="300"></canvas>
  <div class="pointer"></div> <!-- <- ahora va después del canvas -->
</div>

  <button id="spin">Girar</button>

  <script>
    const canvas = document.getElementById("wheel");
    const ctx = canvas.getContext("2d");
    const segments = ["Kiss", "Slap"];
    const colors = ["#c10d30", "#033895"];
    const numSegments = segments.length;
    const anglePerSegment = 2 * Math.PI / numSegments;
    let currentAngle = 0;

    function drawWheel() {
      for (let i = 0; i < numSegments; i++) {
        const startAngle = i * anglePerSegment;
        const endAngle = startAngle + anglePerSegment;

        // Segment color
        ctx.fillStyle = colors[i % colors.length];
        ctx.beginPath();
        ctx.moveTo(150, 150);
        ctx.arc(150, 150, 150, startAngle, endAngle);
        ctx.closePath();
        ctx.fill();

        // Segment text
        ctx.save();
        ctx.translate(150, 150);
        ctx.rotate(startAngle + anglePerSegment / 2);
        ctx.textAlign = "right";
        ctx.fillStyle = "white";
        ctx.font = "20px Arial";
        ctx.fillText(segments[i], 140, 10);
        ctx.restore();
      }
    }

       drawWheel();

    // Girar ruleta
    let currentRotation = 0;

    document.getElementById("spin").addEventListener("click", () => {
      const randomAngle = Math.random() * 360;
      const extraSpins = 5; // vueltas completas
      const targetRotation = currentRotation + 360 * extraSpins + randomAngle;
      const rotationDeg = targetRotation % 360;
      const canvasRotation = targetRotation;

      canvas.style.transition = "transform 4s cubic-bezier(0.33, 1, 0.68, 1)";
      canvas.style.transform = `rotate(${canvasRotation}deg)`;

      currentRotation = canvasRotation;

      // Mostrar resultado después del giro
      setTimeout(() => {
        const selectedIndex = Math.floor(((360 - rotationDeg) % 360) / (360 / numSegments));
        alert("Resultado: " + segments[selectedIndex]);
      }, 4200);
    });
  </script>
</body>
</html>
