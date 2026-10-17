
<!DOCTYPE html>
<html lang="pt-BR">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width,initial-scale=1">
<title>Cobrinha das Letras</title>
<style>
body{margin:0;background:#14251d;color:white;font-family:Arial;text-align:center}
h1{color:#7dff9b}
canvas{background:#203a2b;border:4px solid #6dce82;max-width:95vw}
button{padding:12px 20px;margin:8px;border:0;border-radius:8px;background:#7dff9b;font-weight:bold;font-size:17px}
p{font-size:18px}
</style>
</head>
<body>
<h1>🐍 Cobrinha das Letras</h1>
<p>Pontos: <span id="score">0</span> | Letras coletadas: <span id="word"> </span></p>
<canvas id="game" width="400" height="400"></canvas>
<p>Use as setas do teclado ou WASD para mover.</p>
<button onclick="start()">COMEÇAR / REINICIAR</button>
<script>
const canvas=document.getElementById("game");
const ctx=canvas.getContext("2d");
const cell=20, size=20;
let snake,dir,next,letter,score,word,timer,playing=false;

function randomLetter(){
 return {x:Math.floor(Math.random()*size),
 y:Math.floor(Math.random()*size),
 char:"ABCDEFGHIJKLMNOPQRSTUVWXYZ"[Math.floor(Math.random()*26)]};
}
function start(){
 clearInterval(timer);
 snake=[{x:8,y:10},{x:7,y:10},{x:6,y:10}];
 dir={x:1,y:0};next={x:1,y:0};
 letter=randomLetter();score=0;word="";playing=true;
 update();
 timer=setInterval(tick,130);
}
function update(){
 document.getElementById("score").textContent=score;
 document.getElementById("word").textContent=word;
}
document.addEventListener("keydown",e=>{
 const k=e.key.toLowerCase();
 const moves={
 ArrowUp:{x:0,y:-1},w:{x:0,y:-1},
 ArrowDown:{x:0,y:1},s:{x:0,y:1},
 ArrowLeft:{x:-1,y:0},a:{x:-1,y:0},
 ArrowRight:{x:1,y:0},d:{x:1,y:0}
 };
 if(moves[k]){
  e.preventDefault();
  const d=moves[k];
  if(d.x!==-dir.x||d.y!==-dir.y)next=d;
 }
});
function tick(){
 dir=next;
 const head={x:snake[0].x+dir.x,y:snake[0].y+dir.y};
 if(head.x<0||head.y<0||head.x>=size||head.y>=size||
 snake.some((p,i)=>i>0&&p.x===head.x&&p.y===head.y)){
  clearInterval(timer);playing=false;
  alert("Fim de jogo! Pontuação: "+score);
  return;
 }
 snake.unshift(head);
 if(head.x===letter.x&&head.y===letter.y){
  score+=10;word+=letter.char;letter=randomLetter();
  while(snake.some(p=>p.x===letter.x&&p.y===letter.y))letter=randomLetter();
  update();
 }else snake.pop();
 draw();
}
function draw(){
 ctx.clearRect(0,0,400,400);
 ctx.strokeStyle="#2d4b38";
 for(let i=0;i<=400;i+=cell){
  ctx.beginPath();ctx.moveTo(i,0);ctx.lineTo(i,400);ctx.stroke();
  ctx.beginPath();ctx.moveTo(0,i);ctx.lineTo(400,i);ctx.stroke();
 }
 ctx.fillStyle="#ffd95e";ctx.font="bold 20px Arial";
 ctx.textAlign="center";ctx.textBaseline="middle";
 ctx.fillText(letter.char,letter.x*cell+10,letter.y*cell+10);
 snake.forEach((p,i)=>{
  ctx.fillStyle=i===0?"#a0ff9a":"#48c96b";
  ctx.fillRect(p.x*cell+1,p.y*cell+1,cell-2,cell-2);
 });
 ctx.fillStyle="#10251b";
 ctx.fillRect(snake[0].x*cell+5,snake[0].y*cell+5,3,3);
 ctx.fillRect(snake[0].x*cell+12,snake[0].y*cell+5,3,3);
}
function idle(){
 ctx.fillStyle="#203a2b";ctx.fillRect(0,0,400,400);
 ctx.fillStyle="#fff";ctx.font="20px Arial";ctx.textAlign="center";
 ctx.fillText("Clique em COMEÇAR",200,200);
}
idle();
</script>
</body>
</html>
