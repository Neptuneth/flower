<!DOCTYPE html>
<html lang="th">
<head>
<meta charset="UTF-8">
<title>Sunflower for You 🌻</title>
<meta name="viewport" content="width=device-width, initial-scale=1">

<link href="https://cdn.jsdelivr.net/npm/bootstrap@5.3.2/dist/css/bootstrap.min.css" rel="stylesheet">

<style>
body{
    min-height:100vh;
    margin:0;
    display:flex;
    justify-content:center;
    align-items:center;
    font-family:'Segoe UI',sans-serif;
    overflow:hidden;
}

.overlay{
    position:fixed;
    inset:0;
    background:rgba(255,200,0,.3);
    z-index:0;
}

.card{
    border-radius:28px;
    background:rgba(255,255,255,.85);
    backdrop-filter:blur(10px);
    padding:35px 30px;
    max-width:440px;
    text-align:center;
    z-index:1;
    box-shadow:0 20px 40px rgba(0,0,0,.25);
}

.flower{
    font-size:64px;
    animation:float 3s ease-in-out infinite;
}

@keyframes float{
    0%{transform:translateY(0)}
    50%{transform:translateY(-10px)}
    100%{transform:translateY(0)}
}

h1{color:#c99700;font-weight:700}

.btn-flower{
    background:linear-gradient(135deg,#ffd84d,#f4c430);
    border:none;
    border-radius:30px;
    padding:12px 28px;
    font-weight:600;
}

.petal{
    position:fixed;
    top:-40px;
    font-size:24px;
    animation:fall linear infinite;
    pointer-events:none;
    z-index:0;
}

@keyframes fall{
    to{transform:translateY(110vh) rotate(360deg)}
}
</style>
</head>

<body>

<div class="overlay"></div>

<div class="card">

<div id="sweetText" style="display:none;color:#d97706;margin-bottom:10px">
ชอบอยู่ข้าง ๆ แม้ไม่ใกล้เกินไป<br>
แต่เต็มไปด้วยความจริงใจและมั่นคงง<br>
เหมือนดอกทานตะวันที่หันหาแสงเพียงดวงเดียวเสมอ 🌻
</div>

<div class="flower">🌻🌻🌻</div>
<h1>Sunflower for You 🌻</h1>

<p>
ดอกทานตะวันไม่ได้หันหาใครมากมาย<br>
แค่หันไปทางที่มีแสงก็พอ<br>
บางความรู้สึกก็เหมือนกัน<br>
แค่ได้อยู่ไกล ๆ แล้วสบายใจก็ดีแล้ว
</p>

<button class="btn btn-flower mt-2" onclick="addFlowers()">รับดอกไม้ 💐</button>

<div class="mt-3">
<input id="ytUrl" class="form-control mb-2" placeholder="ลิงก์ YouTube (เพลง)">
<button class="btn btn-outline-secondary w-100" onclick="playMedia()">▶️ เล่นเพลง</button>
</div>

<div id="ytPlayer"></div>

<script src="https://www.youtube.com/iframe_api"></script>
<script>
let petalInterval = null;
let player = null;

function addFlowers(){
    const t = document.getElementById('sweetText');
    t.style.display = "block";
    spawnPetals(10);
}

function spawnPetals(n){
    for(let i=0;i<n;i++){
        const p = document.createElement('div');
        p.className = 'petal';
        p.textContent = Math.random() > .5 ? '🌻' : '💛';
        p.style.left = Math.random()*100 + 'vw';
        p.style.animationDuration = (4 + Math.random()*4) + 's';
        document.body.appendChild(p);
        setTimeout(()=>p.remove(),9000);
    }
}

function startPetals(){
    if(petalInterval) return;
    petalInterval = setInterval(()=>spawnPetals(4),600);
}

function playMedia(){
    const ytUrl = document.getElementById('ytUrl').value;
    if(!ytUrl) return alert("ใส่ลิงก์ YouTube ก่อนนะ");

    const match = ytUrl.match(/(?:v=|be\/)([A-Za-z0-9_-]{11})/);
    if(!match) return alert("ลิงก์ YouTube ไม่ถูกต้อง");

   
    if(player){
        player.loadVideoById(match[1]);
    } else {
        player = new YT.Player('ytPlayer', {
            height: '0',
            width: '0',
            videoId: match[1],
            events: {
                'onReady': (e) => {
                    e.target.unMute();
                    e.target.playVideo();
                    startPetals();
                }
            }
        });
    }
}
</script>

</body>
</html>
