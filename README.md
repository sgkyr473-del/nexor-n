



<!DOCTYPE html>
<html lang="fa" dir="rtl">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width,initial-scale=1">
<title>NEXOR AI ULTIMATE</title>

<style>
*{box-sizing:border-box}
body{
 margin:0;background:#07080d;color:white;
 font-family:Tahoma,Arial;
}
header{
 padding:18px;text-align:center;
 background:#11131c;border-bottom:1px solid #ff304f;
}
header h2{margin:0;color:#ff304f;letter-spacing:2px}
header p{font-size:11px;color:#999}
.container{
 max-width:500px;margin:auto;padding:15px;
}
.card{
 background:#11141e;border:1px solid #282d3a;
 border-radius:20px;padding:15px;margin-bottom:15px;
}
.stage{
 height:260px;display:flex;align-items:center;
 justify-content:center;
 background:radial-gradient(circle,#45202c,#08090e);
 border-radius:16px;perspective:600px;
 overflow:hidden;
}
.head{
 width:140px;height:185px;
 background:linear-gradient(130deg,#e5b18f,#b77b60);
 border-radius:48% 48% 45% 45%;
 position:relative;
 box-shadow:inset -12px -10px 20px #754536;
 transition:transform .1s;
 transform-style:preserve-3d;
}
.hair{
 position:absolute;top:-7px;left:-3px;
 width:146px;height:60px;
 background:#20191b;border-radius:60% 60% 20% 20%;
}
.eye{
 position:absolute;top:83px;width:13px;height:17px;
 background:#171116;border-radius:50%;
}
.left{left:32px}.right{right:32px}
.nose{
 position:absolute;top:105px;left:65px;
 width:14px;height:20px;
 border-bottom:3px solid #995c4b;
 border-radius:50%;
}
.mouth{
 position:absolute;top:143px;left:50px;
 width:40px;height:7px;
 background:#772e3d;border-radius:50%;
}
video{
 width:100%;height:180px;object-fit:cover;
 background:#050505;border-radius:13px;
 transform:scaleX(-1);margin-top:12px;
}
button{
 border:0;border-radius:12px;padding:13px;
 background:#ff304f;color:white;
 font-weight:bold;font-size:14px;
}
.buttons{display:flex;gap:8px;flex-wrap:wrap}
.buttons button{flex:1}
.secondary{background:#252b3a}
#status{
 background:#1b202c;color:#ff879a;
 padding:12px;border-radius:10px;
 font-size:12px;line-height:2;margin-top:10px;
}
.chat{
 height:190px;overflow-y:auto;
 display:flex;flex-direction:column;gap:8px;
 padding:5px;
}
.msg{
 padding:10px 13px;border-radius:12px;
 max-width:90%;font-size:13px;line-height:1.8;
}
.user{background:#26364c;align-self:flex-start}
.bot{background:#411b27;align-self:flex-end}
.inputrow{display:flex;gap:7px;margin-top:12px}
input{
 min-width:0;flex:1;background:#080b12;
 border:1px solid #343a49;color:white;
 padding:13px;border-radius:12px;
}
h3{font-size:15px;color:#ff6078}
.small{font-size:11px;color:#888}
</style>
</head>

<body>

<header>
<h2>NEXOR AI ULTIMATE</h2>
<p>VIRTUAL BOY • VOICE ASSISTANT • AI CHAT</p>
</header>

<div class="container">

<div class="card">
<h3>شخصیت مجازی NEXOR</h3>

<div class="stage">
<div class="head" id="head">
<div class="hair"></div>
<div class="eye left" id="eye1"></div>
<div class="eye right" id="eye2"></div>
<div class="nose"></div>
<div class="mouth" id="mouth"></div>
</div>
</div>

<video id="video" autoplay muted playsinline></video>

<div class="buttons">
<button onclick="cameraStart()">فعال‌سازی دوربین</button>
<button class="secondary" onclick="cameraStop()">قطع دوربین</button>
</div>
</div>

<div class="card">
<h3>دستیار صوتی NEXOR</h3>

<div class="buttons">
<button onclick="listen()">🎙 شروع صحبت</button>
<button class="secondary" onclick="stopListen()">⏹ توقف</button>
<button class="secondary" onclick="speechStop()">🔇 قطع صدا</button>
</div>

<div id="status">دستیار آماده است</div>
</div>

<div class="card">
<h3>گفتگوی هوشمند</h3>

<div class="chat" id="chat">
<div class="msg bot">سلام! من NEXOR AI هستم. چطور می‌تونم کمکت کنم؟</div>
</div>

<div class="inputrow">
<input id="message" placeholder="پیامت رو بنویس...">
<button onclick="sendMessage()">ارسال</button>
</div>
<p class="small">نسخه اولیه بدون API؛ پاسخ‌ها بر اساس الگوهای محلی هستند.</p>
</div>

</div>

<script>
const video=document.getElementById("video");
const head=document.getElementById("head");
const mouth=document.getElementById("mouth");
const eye1=document.getElementById("eye1");
const eye2=document.getElementById("eye2");
const status=document.getElementById("status");
const chat=document.getElementById("chat");

let stream=null;
let recognition=null;
let listening=false;
let detector=null;
let running=false;

function addMessage(text,type){
 const div=document.createElement("div");
 div.className="msg "+type;
 div.textContent=text;
 chat.appendChild(div);
 chat.scrollTop=chat.scrollHeight;
}

function speak(text){
 if(!("speechSynthesis" in window)){
  status.textContent="مرورگر از پخش صدا پشتیبانی نمی‌کند";
  return;
 }
 speechSynthesis.cancel();
 const utterance=new SpeechSynthesisUtterance(text);
 utterance.lang="fa-IR";
 utterance.rate=0.95;
 utterance.pitch=1;
 speechSynthesis.speak(utterance);
}

function answer(text){
 const t=text.toLowerCase();

 if(t.includes("سلام"))return "سلام! خوش اومدی. من NEXOR هستم.";
 if(t.includes("اسمت"))return "اسم من NEXOR AI هست.";
 if(t.includes("خوبی"))return "ممنون، آماده‌ام باهات صحبت کنم.";
 if(t.includes("ساعت"))return "برای دیدن ساعت دقیق، ساعت گوشی رو بررسی کن.";
 if(t.includes("خداحافظ"))return "خداحافظ! هر وقت خواستی می‌تونی برگردی.";
 if(t.includes("نکسور")||t.includes("nexor"))return "NEXOR یک دستیار مجازی با قابلیت گفتگوی صوتی است.";
 if(t.includes("کمک"))return "بگو چه کاری داری تا راهنماییت کنم.";
 if(t.includes("چیکار"))return "می‌تونم باهات گفتگو کنم و به سوالات ساده جواب بدم.";

 return "متوجه شدم. بیشتر برام توضیح بده تا بهتر راهنماییت کنم.";
}

function sendText(text){
 if(!text.trim())return;

 addMessage(text,"user");

 const reply=answer(text);
 setTimeout(()=>{
  addMessage(reply,"bot");
  speak(reply);
 },400);
}

function sendMessage(){
 const input=document.getElementById("message");
 const text=input.value.trim();
 input.value="";
 sendText(text);
}

document.getElementById("message").addEventListener("keydown",e=>{
 if(e.key==="Enter")sendMessage();
});

// سیستم تشخیص گفتار
function listen(){

 const SpeechRecognition=
 window.SpeechRecognition||window.webkitSpeechRecognition;

 if(!SpeechRecognition){
  status.textContent="تشخیص گفتار در این مرورگر پشتیبانی نمی‌شود.";
  return;
 }

 if(listening)return;

 recognition=new SpeechRecognition();
 recognition.lang="fa-IR";
 recognition.continuous=false;
 recognition.interimResults=false;

 recognition.onstart=()=>{
  listening=true;
  status.textContent="🎙 دارم گوش می‌دم...";
 };

 recognition.onresult=e=>{
  const text=e.results[0][0].transcript;
  status.textContent="صدای تو: "+text;
  sendText(text);
 };

 recognition.onerror=e=>{
  status.textContent="خطای میکروفون: "+e.error;
 };

 recognition.onend=()=>{
  listening=false;
 };

 recognition.start();
}

function stopListen(){
 if(recognition){
  recognition.stop();
 }
 listening=false;
 status.textContent="شنیدن متوقف شد";
}

function speechStop(){
 speechSynthesis.cancel();
}

// دوربین
async function cameraStart(){

 try{
  if(stream)cameraStop();

  status.textContent="در حال دریافت مجوز دوربین...";

  stream=await navigator.mediaDevices.getUserMedia({
   audio:false,
   video:{facingMode:"user"}
  });

  video.srcObject=stream;
  await video.play();

  status.textContent="دوربین فعال شد ✓";

  await loadFaceAI();

 }catch(e){
  status.textContent="خطای دوربین: "+e.message;
 }
}

function cameraStop(){
 running=false;
 if(stream){
  stream.getTracks().forEach(t=>t.stop());
  stream=null;
 }
 video.srcObject=null;
 head.style.transform="";
 status.textContent="دوربین خاموش شد";
}

async function loadFaceAI(){

 try{
  const mp=await import(
   "https://cdn.jsdelivr.net/npm/@mediapipe/tasks-vision@0.10.14"
  );

  const vision=await mp.FilesetResolver.forVisionTasks(
   "https://cdn.jsdelivr.net/npm/@mediapipe/tasks-vision@0.10.14/wasm"
  );

  detector=await mp.FaceLandmarker.createFromOptions(vision,{
   baseOptions:{
    modelAssetPath:
    "https://storage.googleapis.com/mediapipe-models/face_landmarker/face_landmarker/float16/1/face_landmarker.task",
    delegate:"CPU"
   },
   runningMode:"VIDEO",
   numFaces:1,
   outputFaceBlendshapes:true
  });

  running=true;
  status.textContent="دوربین و هوش مصنوعی فعال هستند ✓";
  requestAnimationFrame(track);

 }catch(e){
  console.error(e);
  status.textContent="دوربین فعال است؛ خطای بارگذاری مدل: "+e.message;
 }
}

let lastTime=-1;

function track(time){

 if(!running||!detector)return;

 if(video.readyState>=2&&time!==lastTime){
  lastTime=time;

  try{
   const result=detector.detectForVideo(video,time);

   if(result.faceLandmarks.length){

    const p=result.faceLandmarks[0];

    const x=(p[1].x-0.5)*80;
    const y=(p[1].y-0.5)*55;

    const roll=Math.atan2(
     p[263].y-p[33].y,
     p[263].x-p[33].x
    )*180/Math.PI;

    head.style.transform=
    `rotateY(${Math.max(-35,Math.min(35,x))}deg)
     rotateX(${Math.max(-25,Math.min(25,y))}deg)
     rotateZ(${Math.max(-25,Math.min(25,roll))}deg)`;

    if(result.faceBlendshapes.length){

     const shapes=result.faceBlendshapes[0].categories;

     const getScore=n=>{
      const a=shapes.find(x=>x.categoryName===n);
      return a?a.score:0;
     };

     mouth.style.height=(6+getScore("jawOpen")*38)+"px";

     eye1.style.height=
     getScore("eyeBlinkLeft")>.45?"2px":"17px";

     eye2.style.height=
     getScore("eyeBlinkRight")>.45?"2px":"17px";
    }
   }
  }catch(e){
   console.log(e);
  }
 }

 requestAnimationFrame(track);
}
</script>

</body>
</html>
