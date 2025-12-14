<!DOCTYPE html>
<html lang="fa">
<head>
<meta charset="UTF-8">
<title>⚡ Blue Ion Engine</title>
<style>
  html,body{
    margin:0;
    overflow:hidden;
    background:#000814;
    font-family:sans-serif;
  }
  #info{
    position:absolute;
    top:12px;
    left:12px;
    padding:8px 14px;
    border-radius:10px;
    background:rgba(255,255,255,0.06);
    color:#aee8ff;
    backdrop-filter: blur(8px);
  }
</style>
</head>
<body>
<canvas id="c"></canvas>
<div id="info">⚡ Blue Ion Engine — Hold Mouse</div>

<script>
const canvas = document.getElementById("c");
const ctx = canvas.getContext("2d");
let w,h;
function resize(){
  w = canvas.width = innerWidth;
  h = canvas.height = innerHeight;
}
resize();
addEventListener("resize", resize);

let ions = [];
let active = false;
let mouse = {x:w/2, y:h/2};

addEventListener("mousemove", e=>{
  mouse.x = e.clientX;
  mouse.y = e.clientY;
});
addEventListener("mousedown", ()=> active = true);
addEventListener("mouseup", ()=> active = false);

function emitIons(){
  if(!active) return;

  for(let i=0;i<8;i++){
    ions.push({
      x: mouse.x,
      y: mouse.y,
      vx: -Math.random()*1.5 - 2.5,
      vy: (Math.random()-0.5)*0.8,
      life: 160,
      size: Math.random()*2+1,
      charge: Math.random()*0.5+0.5
    });
  }
}

function draw(){
  // cold space fade
  ctx.fillStyle = "rgba(0,5,20,0.25)";
  ctx.fillRect(0,0,w,h);

  emitIons();

  ions.forEach(p=>{
    p.x += p.vx;
    p.y += p.vy;
    p.life--;
    p.vx *= 1.01; // acceleration by electric field

    let glow = p.charge * 30;
    ctx.shadowBlur = glow;
    ctx.shadowColor = "rgba(80,200,255,0.8)";
    ctx.fillStyle = `rgba(120,220,255,${p.life/160})`;

    ctx.beginPath();
    ctx.arc(p.x, p.y, p.size, 0, Math.PI*2);
    ctx.fill();
  });

  ions = ions.filter(p=>p.life>0);

  // engine ring
  if(active){
    ctx.strokeStyle = "rgba(120,220,255,0.4)";
    ctx.lineWidth = 2;
    ctx.beginPath();
    ctx.arc(mouse.x, mouse.y, 18, 0, Math.PI*2);
    ctx.stroke();
  }

  requestAnimationFrame(draw);
}
draw();
</script>
</body>
</html>
