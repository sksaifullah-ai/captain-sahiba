<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>To The Girl Who Was Born To Touch The Sky</title>

<style>
/* ===== BODY ===== */
body{
  margin:0;
  font-family:"Segoe UI",system-ui,sans-serif;
  background:linear-gradient(180deg,#ffe3ef,#fff0f6);
  overflow:hidden;
}

/* ===== SLIDES ===== */
.slide{
  position:absolute;
  width:100%;
  height:100vh;
  display:flex;
  flex-direction:column;
  justify-content:center;
  align-items:center;
  text-align:center;
  padding:20px;
}

.hidden{display:none}

/* ===== TITLE & MESSAGES ===== */
h1{
  font-size: clamp(24px,5vw,48px); /* responsive */
  color:#333;
  letter-spacing:1px;
  text-align:center;
}

.message{
  width: clamp(80%,65%,90%);
  font-size: clamp(16px,4vw,24px);
  line-height:1.9;
  color:#333;
  font-weight:bold;
}

.line{
  opacity:0;
  transform:translateY(10px);
  animation:fadeUp 1s forwards;
}

@keyframes fadeUp{
  to{opacity:1;transform:translateY(0)}
}

/* ===== PETALS ===== */
.petal{
  position:absolute;
  top:-50px;
  font-size:22px;
  animation:fall linear forwards, sway 4s ease-in-out infinite;
  opacity:.9;
}

@keyframes fall{
  0%{transform:translateY(-50px) rotate(0deg)}
  100%{transform:translateY(110vh) rotate(360deg)}
}

@keyframes sway{
  0%{margin-left:-10px}
  50%{margin-left:10px}
  100%{margin-left:-10px}
}

/* ===== HAMSTER BUTTON ===== */
#hamsterContainer{
  position:absolute;
  bottom:80px;
  display:flex;
  flex-direction:column;
  align-items:center;
  cursor:pointer;
  transition:transform .2s;
}

#hamster{
  font-size: clamp(28px,8vw,54px); /* responsive */
  animation:hamRun 0.6s infinite alternate;
}

@keyframes hamRun{
  from{transform:translateY(0)}
  to{transform:translateY(-6px)}
}

#tapText{
  margin-top:6px;
  background:#ff5fa2;
  color:white;
  padding: clamp(6px,1vw,10px) clamp(12px,3vw,22px);
  border-radius:25px;
  font-size: clamp(12px,3vw,16px);
  box-shadow:0 6px 16px rgba(0,0,0,0.18);
  font-weight:600;
}

/* ===== FINAL SLIDE ===== */
#final h1{
  font-size: clamp(36px,6vw,58px);
  color:black;
  font-weight:900;
  letter-spacing:1px;
}

.glow{display:none;}

@keyframes glowPulse{
  0%{transform:scale(1)}
  50%{transform:scale(1.15)}
  100%{transform:scale(1)}
}

/* ===== FIREWORKS ===== */
.firework{
  position:absolute;
  font-size:30px;
  animation:explode 1.2s ease-out forwards;
}

@keyframes explode{
  0%{transform:scale(.3);opacity:1}
  100%{transform:scale(2);opacity:0}
}

/* ===== MEDIA QUERIES (PHONES) ===== */
@media (max-width:600px){
  h1{ font-size: clamp(20px,7vw,38px); }
  .message{ font-size: clamp(14px,4vw,20px); width:90%; }
  #hamster{ font-size: clamp(24px,10vw,40px); }
  #tapText{ font-size: clamp(12px,3vw,14px); padding:6px 12px; }
}
</style>
</head>

<body>

<!-- SLIDE 1 -->
<div id="slide1" class="slide">
  <h1>To the Girl Who Was Born to Touch the Sky</h1>
  <div id="hamsterContainer" class="hidden">
    <div id="hamster">🐹</div>
    <div id="tapText">Tap Here</div>
  </div>
</div>

<!-- SLIDE 2 MESSAGE -->
<div id="slide2" class="slide hidden">
  <div class="message" id="message"></div>
</div>

<!-- SLIDE 3 FINAL -->
<div id="final" class="slide hidden">
  <h1>Congratulations Captain Sahiba</h1>
</div>

<script>
const hamster=document.getElementById("hamsterContainer")
const slide1=document.getElementById("slide1")
const slide2=document.getElementById("slide2")
const finalSlide=document.getElementById("final")
const messageBox=document.getElementById("message")
let dodges=0
const lines=[
  "Dear Captain Sahiba,",
  "You have stood strong through everything and worked so hard to reach this point.",
  "Even through difficulties, you stayed the same kind and gentle soul.",
  "May the skies ahead always be good to you and may your journey be blessed always.",
  "May Allah always watch over you, protect you, and guide every step of your journey."
]

/* HAMSTER APPEARS */
setTimeout(()=>{
  hamster.classList.remove("hidden")
  randomMove()
},2000)

function randomMove(){
  hamster.style.position="absolute"
  const moveInterval=setInterval(()=>{
    if(dodges>=7){clearInterval(moveInterval);return}
    const x=Math.random()*(window.innerWidth-120)
    const y=Math.random()*(window.innerHeight-160)
    hamster.style.left=x+"px"
    hamster.style.top=y+"px"
  },450)
}

/* DODGE CURSOR */
document.addEventListener("mousemove",e=>{
  const rect=hamster.getBoundingClientRect()
  const distX=Math.abs(e.clientX-(rect.left+rect.width/2))
  const distY=Math.abs(e.clientY-(rect.top+rect.height/2))
  if(distX<130 && distY<130 && dodges<7){
    const x=Math.random()*(window.innerWidth-120)
    const y=Math.random()*(window.innerHeight-160)
    hamster.style.left=x+"px"
    hamster.style.top=y+"px"
    dodges++
  }
})

hamster.addEventListener("click",()=>{
  slide1.classList.add("hidden")
  slide2.classList.remove("hidden")
  showLines()
})

/* MESSAGE SEQUENCE */
function showLines(){
  let i=0
  function next(){
    if(i>=lines.length){
      setTimeout(showFinal,2200)
      return
    }
    const p=document.createElement("div")
    p.className="line"
    p.textContent=lines[i]
    messageBox.appendChild(p)
    i++
    setTimeout(next,1800)
  }
  next()
}

function showFinal(){
  slide2.classList.add("hidden")
  finalSlide.classList.remove("hidden")
  launchFireworks()
}

/* FIREWORKS */
function launchFireworks(){
  setInterval(()=>{
    const f=document.createElement("div")
    f.className="firework"
    f.innerHTML=["🎆","✨","🎉","🌸","💫"][Math.floor(Math.random()*5)]
    f.style.left=Math.random()*window.innerWidth+"px"
    f.style.top=Math.random()*window.innerHeight+"px"
    document.body.appendChild(f)
    setTimeout(()=>f.remove(),1200)
  },200)
}

/* PETALS */
function createPetal(){
  const p=document.createElement("div")
  p.className="petal"
  p.innerHTML="🌸"
  p.style.left=Math.random()*window.innerWidth+"px"
  p.style.fontSize=(window.innerWidth/25 + Math.random()*5)+"px" // responsive size
  p.style.animationDuration=(6+Math.random()*5)+"s"
  document.body.appendChild(p)
  setTimeout(()=>p.remove(),11000)
}
setInterval(createPetal,500)
</script>

</body>
</html>
