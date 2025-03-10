<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Physics with Matter.js - Click to Spawn Balls</title>
  <script src="https://cdnjs.cloudflare.com/ajax/libs/matter-js/0.14.2/matter.min.js"></script>
  <style>
    body { margin: 0; overflow: hidden; }
    canvas { display: block; }
  </style>
</head>
<body>

<script>
  // Create an engine
  const engine = Matter.Engine.create();
  const world = engine.world;

  // Create a renderer
  const render = Matter.Render.create({
    element: document.body,
    engine: engine,
    options: {
      width: window.innerWidth,
      height: window.innerHeight,
      wireframes: false
    }
  });

  // Create a ground
  const ground = Matter.Bodies.rectangle(window.innerWidth / 2, window.innerHeight - 25, window.innerWidth, 50, { isStatic: true });
  Matter.World.add(world, [ground]);

  // Run the engine and renderer
  Matter.Engine.run(engine);
  Matter.Render.run(render);

  // Mouse control
  const mouse = Matter.Mouse.create(render.canvas);
  const mouseConstraint = Matter.MouseConstraint.create(engine, {
    mouse: mouse,
    constraint: {
      stiffness: 0.2,
      render: { visible: false }
    }
  });

  Matter.World.add(world, mouseConstraint);

  // Function to create a new ball at the mouse position
  function createBall(x, y) {
    const ball = Matter.Bodies.circle(x, y, 40, { restitution: 0.8 });
    Matter.World.add(world, ball);
  }

  // Add event listener for mouse click to spawn a ball at the mouse position
  render.canvas.addEventListener('click', function(event) {
    const mousePosition = mouse.position; // Use the mouse object's position
    createBall(mousePosition.x, mousePosition.y);
  });
</script>

</body>
</html>

