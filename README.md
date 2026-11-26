basket pogibshego-game
function resize() {
    W = c.width = window.innerWidth;
    H = c.height = window.innerHeight;
  }
  resize();
  window.addEventListener('resize', resize);

  let basketX = W / 2;
  let score = 0;
  let best = 0;
  let balls = ;

  function spawn() {
    balls.push({
      x: 30 + Math.random() * (W - 60),
      y: -20,
      vy: 2 + Math.random() * 2 + score * 0.1
    });
  }
  setInterval(spawn, 1200);

  function move(e) {
    const t = e.touches ? e.touches : e;
    basketX = t.clientX;
  }
  window.addEventListener('mousemove', move);
  window.addEventListener('touchmove', move, { passive: false });

  function loop() {
    x.fillStyle = '#1a3a5c';
    x.fillRect(0, 0, W, H);

    // Корзина
    x.fillStyle = '#f80';
    x.fillRect(basketX - 45, H - 60, 90, 20);
    x.fillStyle = '#ff5';
    x.fillRect(basketX - 45, H - 64, 90, 6);

    for (let b of balls) {
      b.y += b.vy;
      if (b.y > H - 70 && b.y < H - 40 && Math.abs(b.x - basketX) < 50 && !b.caught) {
        b.caught = true;
        score++;
        if (score > best) best = score;
      }
      if (b.y < H + 30 && !b.caught) {
        x.beginPath();
        x.arc(b.x, b.y, 14, 0, Math.PI * 2);
        x.fillStyle = '#fff';
        x.fill();
        x.strokeStyle = '#000';
        x.stroke();
      }
    }

    balls = balls.filter(b => !b.caught && b.y < H + 30);

    x.fillStyle = '#fff';
    x.font = '28px sans-serif';
    x.fillText('Счёт: ' + score, 10, 34);
    x.fillText('Рекорд: ' + best, 10, 68);

    requestAnimationFrame(loop);
  }
  loop();
</script>