// --- 3D фон и движение мыши ---
const canvas = document.getElementById('background');
const ctx = canvas.getContext('2d');
canvas.width = innerWidth;
canvas.height = innerHeight;

let rects = Array.from({length: 60}, () => ({
  x: Math.random() * canvas.width,
  y: Math.random() * canvas.height,
  w: 50 + Math.random() * 100,
  h: 30 + Math.random() * 60,
  dx: (Math.random() - 0.5) * 0.5,
  dy: (Math.random() - 0.5) * 0.5,
  angle: Math.random() * 360
}));

let mouse = {x: 0, y: 0};

window.addEventListener('mousemove', e => {
  mouse.x = e.clientX;
  mouse.y = e.clientY;
});

window.addEventListener('scroll', e => {
  rects.forEach(r => r.angle += 2);
});

function draw() {
  ctx.clearRect(0,0,canvas.width,canvas.height);
  rects.forEach(r => {
    ctx.save();
    ctx.translate(r.x, r.y);
    ctx.rotate((r.angle + mouse.x/1000) * Math.PI/180);
    ctx.strokeStyle = `rgba(0,255,100,0.3)`;
    ctx.lineWidth = 2;
    ctx.strokeRect(-r.w/2, -r.h/2, r.w, r.h);
    ctx.restore();
    r.x += r.dx;
    r.y += r.dy;
    if (r.x < 0 || r.x > canvas.width) r.dx *= -1;
    if (r.y < 0 || r.y > canvas.height) r.dy *= -1;
  });
  requestAnimationFrame(draw);
}
draw();

// --- Эффект появления логотипа ---
const logo = document.getElementById('logo');
logo.style.opacity = 0;
setTimeout(() => {
  logo.style.transition = "opacity 2s ease-in-out";
  logo.style.opacity = 1;
}, 500);
