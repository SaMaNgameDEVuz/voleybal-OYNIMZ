<!DOCTYPE html>
<html lang="uz">

<head>
    <meta charset="utf-8">
    <meta name="viewport" content="width=device-width, initial-scale=1, viewport-fit=cover">
    <link href="https://fonts.googleapis.com/css2?family=Lilita+One&display=swap" rel="stylesheet">
    <title>Voleybol 3D</title>
    <style>
        :root {
            --ink: #2b1f6b;
            --acc: #ff8a00;
            box-sizing: border-box;
            padding-top: env(safe-area-inset-top, 0px);
            padding-bottom: env(safe-area-inset-bottom, 0px)
        }

        html,
        body {
            height: 100%;
            margin: 0
        }

        body {
            background: radial-gradient(circle at 20% 20%, rgba(255, 255, 255, .18) 0 60px, transparent 62px), radial-gradient(circle at 80% 70%, rgba(255, 255, 255, .14) 0 90px, transparent 92px), linear-gradient(160deg, #5b3df5, #ff5fa2);
            color: var(--ink);
            font-family: 'Lilita One', 'Trebuchet MS', Arial, sans-serif;
            letter-spacing: .5px;
            display: flex;
            align-items: center;
            justify-content: center
        }

        .panel {
            display: none;
            background: #fff8e7;
            border: 5px solid var(--ink);
            border-radius: 26px;
            box-shadow: 0 10px 0 var(--ink);
            padding: 24px 28px;
            width: min(560px, 92vw);
            max-height: 92vh;
            overflow: auto;
            box-sizing: border-box
        }

        .panel.on {
            display: block
        }

        h1 {
            font-size: 50px;
            margin: 0 0 4px;
            color: #ffd23f;
            -webkit-text-stroke: 3px var(--ink);
            paint-order: stroke fill;
            text-shadow: 0 5px 0 var(--ink);
            line-height: 1.05;
            text-align: center
        }

        h2 {
            font-size: 32px;
            margin: 0 0 12px;
            color: #ff3b4a;
            -webkit-text-stroke: 2px var(--ink);
            paint-order: stroke fill;
            text-shadow: 0 3px 0 var(--ink)
        }

        .sub {
            font-family: Arial, sans-serif;
            font-weight: 700;
            font-size: 13px;
            opacity: .75;
            margin-bottom: 14px;
            text-align: center
        }

        button {
            font-family: inherit;
            font-size: 24px;
            letter-spacing: .5px;
            background: linear-gradient(#ffe066, #ffb703);
            color: var(--ink);
            border: 4px solid var(--ink);
            border-radius: 16px;
            padding: 9px 16px;
            margin: 10px 0;
            cursor: pointer;
            box-shadow: 0 6px 0 var(--ink);
            display: block;
            width: 100%;
            text-align: center;
            transition: transform .08s
        }

        button:hover {
            transform: translateY(-2px);
            filter: brightness(1.08)
        }

        button:active {
            transform: translateY(5px);
            box-shadow: 0 1px 0 var(--ink)
        }

        button:disabled {
            background: #d8d2f0;
            cursor: default;
            transform: none
        }

        button.sm {
            display: inline-block;
            width: auto;
            font-size: 16px;
            padding: 5px 12px;
            margin: 0 0 6px;
            white-space: nowrap;
            background: linear-gradient(#6df0b0, #27c985)
        }

        .coin {
            font-size: 26px;
            color: var(--acc);
            -webkit-text-stroke: 1.5px var(--ink);
            paint-order: stroke fill;
            margin-bottom: 10px;
            text-align: center
        }

        .row {
            display: flex;
            justify-content: space-between;
            align-items: center;
            gap: 10px;
            border-bottom: 3px dashed #c9bdf5;
            padding: 10px 0;
            font-size: 19px
        }

        .row small {
            display: block;
            font-family: Arial, sans-serif;
            font-weight: 700;
            font-size: 12px;
            opacity: .7
        }

        #game {
            display: none;
            width: min(1000px, 98vw)
        }

        #game.on {
            display: block
        }

        #wrap {
            position: relative;
            border: 5px solid var(--ink);
            border-radius: 20px;
            overflow: hidden;
            box-shadow: 0 10px 0 var(--ink)
        }

        #wrap canvas {
            width: 100%;
            height: auto;
            display: block;
            touch-action: none
        }

        #cv {
            position: absolute;
            left: 0;
            top: 0
        }

        .hint {
            font-size: 14px;
            text-align: center;
            margin-top: 16px;
            color: #fff;
            text-shadow: 0 2px 0 var(--ink)
        }
    </style>
</head>

<body>

    <div class="panel on" id="menu">
        <h1>🏐 VOLEYBOL 3D</h1>
        <div class="sub">3 vs 3 · 7 ochkogacha o'yna</div>
        <div class="coin">🪙 <span class="cn">0</span></div>
        <button onclick="show('diff')">▶ PLAY</button><button onclick="openMarket()">🛒 MARKET</button><button onclick="openSet()">⚙ SOZLAMALAR</button>
    </div>

    <div class="panel" id="diff">
        <h2>DARAJANI TANLA</h2>
        <button onclick="start(0)">NORMAL<div class="sub" style="margin:0">Botlar oddiy · mukofot ×1</div></button>
        <button onclick="start(1)">QIYIN<div class="sub" style="margin:0">Botlar tez va aniq · mukofot ×1.5</div></button>
        <button onclick="show('menu')">← ORQAGA</button>
    </div>

    <div class="panel" id="market">
        <h2>MARKET</h2>
        <div class="coin">🪙 <span class="cn">0</span></div>
        <div id="mlist"></div><button onclick="show('menu')">← ORQAGA</button>
    </div>

    <div class="panel" id="set">
        <h2>SOZLAMALAR</h2>
        <div class="sub">Tugmani bosib, keyin yangi tugma/sichqoncha tugmasini bos. (Esc — bekor)</div>
        <div id="slist"></div>
        <button onclick="resetB()" style="margin-top:12px">ASLIGA QAYTARISH</button><button onclick="show('menu')">← ORQAGA</button>
    </div>

    <div class="panel" id="res">
        <h2 id="rt"></h2>
        <div class="coin" id="rs"></div>
        <div id="rc" class="sub"></div>
        <button onclick="start(hard)">QAYTA O'YNA</button><button onclick="show('menu')">MENYU</button>
    </div>

    <div id="game">
        <div id="wrap"><canvas id="cv" width="900" height="450"></canvas></div>
        <div class="hint">Kamera: ← → yoki sichqonchaning o'ng tugmasini bosib surib aylantir · Esc — chiqish · Dive:
            LCtrl</div>
    </div>

    <script src="https://cdnjs.cloudflare.com/ajax/libs/three.js/r128/three.min.js"></script>
    <script>
        const $=id=>document.getElementById(id);
const DEF={fwd:'KeyW',back:'KeyS',left:'KeyA',right:'KeyD',jump:'Space',setb:'KeyQ',jset:'KeyE',m1:'Mouse0',dive:'ControlLeft',camL:'ArrowLeft',camR:'ArrowRight',a1:'Digit1',a2:'Digit2',a3:'Digit3',a4:'Digit4',a5:'Digit5'};
const LB={fwd:'Oldinga (W)',back:'Orqaga (S)',left:'Chapga',right:"O'ngga",jump:'Sakrash (Jump)',setb:'Set / Block (Jump+)',jset:'JumpSet',m1:'Bump / Spike (Jump+)',dive:'Dive',camL:'Kamera chapga',camR:"Kamera o'ngga",a1:'Super Speed',a2:'Control Ball',a3:'BlockBreak',a4:'Blockker',a5:'Team Speed'};
const AB=[{id:'super',n:'Super Speed',d:'30 soniya tez yuguradi',p:150},{id:'ctrl',n:'Control Ball',d:'Urgan to\'pni havoda A/D bilan yo\'naltirasan (minutiga 1)',p:200},{id:'brk',n:'BlockBreak',d:'Spike blokni sindirib, o\'sha o\'yinchini yiqitadi (minutiga 1)',p:300},{id:'blk',n:'Blockker',d:'Keyingi blok to\'pni yerga tez uradi (minutiga 1)',p:250},{id:'team',n:'Team Speed',d:'30 soniya butun jamoa 1.5× tez',p:350}];
let S={coins:100,own:{},B:{...DEF}};
try{const r=localStorage.getItem('mvb2');if(r){const o=JSON.parse(r);S={...S,...o,B:{...DEF,...(o.B||{})}}}}catch(e){}
function save(){try{localStorage.setItem('mvb2',JSON.stringify(S))}catch(e){}}
let scr='menu',hard=0;
function show(s){scr=s;document.querySelectorAll('.panel').forEach(p=>p.classList.remove('on'));$('game').classList.toggle('on',s==='game');if(s!=='game')$(s).classList.add('on');document.querySelectorAll('.cn').forEach(e=>e.textContent=S.coins)}
function lab(c){return c.replace('ArrowLeft','←').replace('ArrowRight','→').replace('Key','').replace('Digit','').replace('Mouse0','M1 (sichqoncha)').replace('Mouse2','M2').replace('Mouse1','M3').replace('ControlLeft','LCtrl').replace('ControlRight','RCtrl').replace('ShiftLeft','LShift')}
function openMarket(){const l=$('mlist');l.innerHTML='';AB.forEach(a=>{const o=S.own[a.id],r=document.createElement('div');r.className='row';r.innerHTML=`<div>${a.n}<small>${a.d}</small></div>`;const b=document.createElement('button');b.className='sm';b.textContent=o?'✔ SOTIB OLINGAN':'🪙 '+a.p;b.disabled=!!o;b.onclick=()=>{if(S.coins>=a.p){S.coins-=a.p;S.own[a.id]=1;save();openMarket()}else{b.textContent='PUL YETMAYDI'}};r.appendChild(b);l.appendChild(r)});show('market')}
let listen=null;
function openSet(){const l=$('slist');l.innerHTML='';Object.keys(DEF).forEach(k=>{const r=document.createElement('div');r.className='row';r.innerHTML=`<div>${LB[k]}</div>`;const b=document.createElement('button');b.className='sm';b.textContent=lab(S.B[k]);b.onclick=()=>{listen=k;b.textContent='... bos ...'};r.appendChild(b);l.appendChild(r)});show('set')}
function resetB(){S.B={...DEF};save();openSet()}
function setKey(code){if(code!=='Escape'){const old=S.B[listen];for(const k in S.B)if(S.B[k]===code)S.B[k]=old;S.B[listen]=code;save()}listen=null;openSet()}

// ---------- GAME ----------
const C=$('cv'),X=C.getContext('2d'),NT=2.33,GR=.0093,PG=.0183,JV=.383,WIN=25,FL=0;
let P,ball,score,fr,ab,keys={},pops,shake,pl,ch,sv,over,B,camYaw=0,cam={x:-5,z:0};
const rnd=Math.random;
function mk(t,s){const hx=(t?1:-1)*[11,8,4][s],hz=(t?-1:1)*[-3,0,3][s];return{t,s,x:hx,hx,hz,z:hz,y:0,vy:0,fx:t?-1:1,fz:0,ry:t?Math.PI:0,act:null,an:0,dv:0,dvx:0,dvz:0,stun:0,mul:1,cdk:0,ex:0,ez:0,bk:0,will:1,human:false}}
function start(h){hard=h;B=S.B;P=[mk(0,0),mk(0,1),mk(0,2),mk(1,0),mk(1,1),mk(1,2)];P[1].human=true;score=[0,0];over=0;pops=[];shake=0;camYaw=0;
ab={};AB.forEach(a=>ab[a.id]={on:0,cd:0,armed:0});if(!R)init3D();buildP();reset(0);show('game');last=performance.now();acc=0}
function reset(srv){sv=srv;P.forEach(p=>{p.x=p.hx;p.z=p.hz;p.y=0;p.vy=0;p.act=null;p.dv=0;p.stun=0;p.ex=(hard?.8:2.4)*(rnd()*2-1);p.ez=(hard?.8:2.4)*(rnd()*2-1);p.bk=rnd()<.3});
ball={x:srv?8:-8,y:3.5,z:0,vx:0,vy:0,vz:0,r:.4,tt:-1,tc:0,cd:[0,0],brk:0,ctrl:0,ctrlT:0,sv:1,since:0};fr=75}
function pop(s,x,y,z){pops.push({s,x,y,z:z||0,t:40})}
function aim(tx,tz,vy){const t=2*vy/GR;ball.vx=(tx-ball.x)/t;ball.vz=(tz-ball.z)/t}
function predict(){let x=ball.x,y=ball.y,z=ball.z,vx=ball.vx,vy=ball.vy,vz=ball.vz;for(let i=0;i<300&&y>.5;i++){vy-=GR;x+=vx;y+=vy;z+=vz}return{x,z}}
function setAct(p,k,n){p.act={k,n}}
function mvec(){const f=(keys[B.fwd]?1:0)-(keys[B.back]?1:0),r=(keys[B.right]?1:0)-(keys[B.left]?1:0),c=Math.cos(camYaw),s=Math.sin(camYaw);const x=f*c-r*s,z=f*s+r*c,l=Math.hypot(x,z);return l?{x:x/l,z:z/l}:{x:0,z:0}}
function activate(id){const a=ab[id];if(!S.own[id]||a.cd>0||scr!=='game')return;a.cd=3600;if(id==='super'||id==='team')a.on=1800;else a.armed=1200;pop(AB.find(x=>x.id===id).n.toUpperCase()+'!',P[1].x,P[1].y+3,P[1].z)}
function press(code){if(scr!=='game')return;const h=P[1];let k=null;for(const a in B)if(B[a]===code)k=a;if(!k)return;
if(k[0]==='a'&&k.length===2){activate(AB[+k[1]-1].id);return}
if(h.stun>0)return;
if(k==='setb')setAct(h,'q',18);else if(k==='m1')setAct(h,'m1',14);
else if(k==='jset'){if(h.y<=0)h.vy=JV;setAct(h,'e',18)}
else if(k==='dive'&&h.dv<=0&&h.y<=0){let m=mvec();if(!m.x&&!m.z)m={x:h.fx,z:h.fz};h.dvx=m.x;h.dvz=m.z;h.fx=m.x;h.fz=m.z;h.dv=18;h.vy=.1;setAct(h,'dive',20)}}
addEventListener('keydown',e=>{if(listen){e.preventDefault();setKey(e.code);return}
if(scr==='game'){e.preventDefault();if(e.code==='Escape'){show('menu');return}if(!keys[e.code]){keys[e.code]=1;press(e.code)}}});
addEventListener('keyup',e=>{delete keys[e.code]});
addEventListener('mousedown',e=>{if(listen){e.preventDefault();setKey('Mouse'+e.button);return}if(scr==='game'&&e.target===C){e.preventDefault();keys['Mouse'+e.button]=1;press('Mouse'+e.button)}});
addEventListener('mouseup',e=>{delete keys['Mouse'+e.button]});
addEventListener('mousemove',e=>{if(scr==='game'&&(e.buttons&2))camYaw+=e.movementX*.006});
C.addEventListener('contextmenu',e=>e.preventDefault());

function hit(p,ty){const b=ball,d=p.t?-1:1,own=b.tt===p.t,H=p.human;
if(ty==='block'||(ty==='jumpset'&&b.brk)){
 if(b.brk){p.stun=55;p.vy=.12;pop('BREAK!!',p.x,p.y+3,p.z);shake=10;return 1}
 const bk=H&&ab.blk.armed>0;b.vx=d*(bk?.1:.17);b.vz*=.5;b.vy=bk?-.47:-.17;if(bk)ab.blk.armed=0;b.tt=p.t;b.tc=0;b.ctrl=0;b.since=0;b.cd[p.t]=10;pop(bk?'BLOCKKER!':'BLOCK!',p.x,p.y+3,p.z);shake=6;return 1}
b.brk=0;b.ctrl=0;
if(own)b.tc++;else{b.tt=p.t;b.tc=1}
const ov=(own&&b.tc>=3)||b.sv,tz=Math.max(-4.2,Math.min(4.2,(H?p.fz*3.2:0)+(rnd()*2-1)*(H?1.2:3)));
if(ty==='spike'){b.vx=d*(H?.32:(hard?.28:.2));b.vz=(H?p.fz*.1:0)-p.z*.006+(rnd()*2-1)*.02;b.vy=b.y>NT+1?.017:.083;if(H&&ab.brk.armed>0){b.brk=1;ab.brk.armed=0}pop('SPIKE!',b.x,b.y+1,b.z);shake=8}
else if(ov){b.vy=.35;aim(d*(3+rnd()*7.5),tz,.35);if(b.sv)pop('SERVE',b.x,b.y+1,b.z)}
else if(ty==='bump'){b.vy=.333;aim(-d*4.7,0,.333);pop('BUMP',b.x,b.y+1,b.z)}
else if(ty==='set'){b.vy=.317;aim(-d*2,0,.317);pop('SET',b.x,b.y+1,b.z)}
else if(ty==='jumpset'){b.vy=.267;aim(-d*1.3,0,.267);pop('JUMPSET',b.x,b.y+1,b.z)}
else{b.vy=.3;aim(-d*5,0,.3);pop('DIVE!',b.x,b.y+1,b.z)}
b.sv=0;b.since=0;b.cd[p.t]=10;
if(H&&ab.ctrl.armed>0&&ty!=='block'){b.ctrl=1;b.ctrlT=320;ab.ctrl.armed=0}
return 1}
function tryHit(p){if(!p.act||p.stun>0)return;const ag=p.y<=0,k=p.act.k;
const ty=k==='q'?(ag?'set':'block'):k==='m1'?(ag?'bump':'spike'):k==='e'?'jumpset':'dive';
if(ty==='jumpset'&&ag)return;
const cy=p.y+(p.dv>0?.5:(ag?1.1:1.5)),r=(ty==='block'||ty==='spike')?2.4:2;
if(Math.hypot(ball.x-p.x,ball.y-cy,ball.z-p.z)<r&&ball.cd[p.t]<=0){if(p.human||p.will)hit(p,ty);p.act=null;p.an=14}}
function bot(p){const t=p.t,ag=p.y<=0,own=t?pl.x>0:pl.x<0,chs=ch[t]===p,b=ball;let tx=p.hx,tz=p.hz;
if(own&&chs&&(b.tt===t||b.tt===-1||b.since>(hard?15:50))){tx=pl.x+p.ex;tz=pl.z+p.ez}
const dx=tx-p.x,dz=tz-p.z,l=Math.hypot(dx,dz),mv=l>.25?{x:dx/l,z:dz/l}:{x:0,z:0};
if(chs&&own&&ag&&b.tt===t&&b.tc>=2&&b.y<5&&Math.hypot(b.x-p.x,b.z-p.z)<3&&Math.abs(p.x)<6&&p.cdk<=0){p.vy=JV;p.cdk=60}
if(chs&&own&&!p.act&&p.an<=0&&Math.hypot(b.x-p.x,b.y-(p.y+1.3),b.z-p.z)<2){p.will=rnd()<(hard?.7:.45);setAct(p,(b.tt===t&&b.tc===1)?'q':'m1',10)}
if(!chs&&hard&&ag&&b.tt===1-t&&Math.abs(b.x)<4&&b.y<5&&Math.abs(p.x)<3&&p.bk&&p.cdk<=0){p.vy=JV;p.cdk=70;p.will=1;setAct(p,'q',22)}
return mv}
function upd(p){if(p.an>0)p.an--;if(p.cdk>0)p.cdk--;const ag=p.y<=0;let m={x:0,z:0};
p.mul=1;if(p.t===0){if(ab.team.on>0)p.mul=1.5;if(p.human&&ab.super.on>0)p.mul=2}
const sp=(p.human?.12:(hard?.07:.045))*p.mul;
if(p.act&&--p.act.n<=0)p.act=null;
if(p.stun>0)p.stun--;
else if(p.dv>0){p.dv--;p.x+=p.dvx*.2;p.z+=p.dvz*.2}
else{if(p.human){m=mvec();if(keys[B.jump]&&ag)p.vy=JV;if(m.x||m.z){p.fx=m.x;p.fz=m.z}}else{m=bot(p);p.fx=p.t?-1:1;p.fz=0}p.x+=m.x*sp;p.z+=m.z*sp}
p.vy-=PG;p.y+=p.vy;if(p.y<=0){p.y=0;p.vy=0}
const lo=p.t?.7:-14.7,hi=p.t?14.7:-.7;p.x=Math.max(lo,Math.min(hi,p.x));p.z=Math.max(-5.8,Math.min(5.8,p.z))}
function step(){
if(keys[B.camL])camYaw-=.03;if(keys[B.camR])camYaw+=.03;
if(fr>0){fr--;return}
for(const k in ab){const a=ab[k];if(a.on>0)a.on--;if(a.cd>0)a.cd--;if(a.armed>0)a.armed--}
pl=predict();ch=[null,null];
[0,1].forEach(t=>{let best=1e9;P.forEach(p=>{if(p.t===t&&p.stun<=0){const d=Math.hypot(p.x-pl.x,p.z-pl.z);if(d<best){best=d;ch[t]=p}}})});
P.forEach(upd);P.forEach(tryHit);
const b=ball;b.since++;
if(b.ctrl&&b.ctrlT>0){b.ctrlT--;const m=mvec();b.vx+=m.x*.012;b.vz+=m.z*.012;if(keys[B.jump])b.vy+=.004;b.vx=Math.max(-.4,Math.min(.4,b.vx));b.vz=Math.max(-.4,Math.min(.4,b.vz))}
b.vy-=GR;b.x+=b.vx;b.y+=b.vy;b.z+=b.vz;b.cd[0]--;b.cd[1]--;
if(Math.abs(b.x)>19){b.x=Math.sign(b.x)*19;b.vx*=-.6}if(Math.abs(b.z)>12){b.z=Math.sign(b.z)*12;b.vz*=-.6}
if(Math.abs(b.x)<b.r+.05&&b.y<NT+b.r&&Math.abs(b.z)<5.4){if(b.y-b.vy>=NT&&b.vy<0){b.y=NT+b.r;b.vy*=-.5}else{b.x=(b.x<0?-1:1)*(b.r+.05);b.vx*=-.35;b.brk=0;b.ctrl=0}}
if(b.y<=b.r){const inn=Math.abs(b.x)<=15&&Math.abs(b.z)<=5.2,w=inn?(b.x<0?1:0):(b.tt<0?1-sv:1-b.tt);score[w]++;pop(inn?'POINT!':'AUT!',b.x,.8,b.z);shake=12;
 if(score[w]>=WIN)end(w);else reset(w)}}
function end(w){over=1;const win=w===0,pts=score[0],c=Math.round((pts*3+(win?150:0))*(hard?1.5:1));S.coins+=c;save();
$('rt').textContent=win?'🏆 G\'ALABA!':'💥 MAG\'LUBIYAT';$('rs').textContent=score[0]+' : '+score[1];
$('rc').textContent=`Mukofot: +${c} 🪙 (${pts} ochko${win?' + g\'alaba bonusi':''}${hard?' · qiyin ×1.5':''})`;show('res')}
let R,SC,CM,pm=[],bm;
const wx=x=>(x-450)/30,wy=y=>(FL-y)/30;
function tex(w,h,f){const c=document.createElement('canvas');c.width=w;c.height=h;f(c.getContext('2d'),w,h);const t=new THREE.CanvasTexture(c);t.encoding=THREE.sRGBEncoding;return t}
let T,OL,bsh;
function mesh(g,mt,x,y,z,par,ol){const o=new THREE.Mesh(g,mt);o.position.set(x,y,z);o.castShadow=true;if(ol!==0){const k=new THREE.Mesh(g,OL);k.scale.setScalar(ol||1.07);o.add(k)}par.add(o);return o}
function init3D(){
R=new THREE.WebGLRenderer({antialias:true});R.setSize(900,450,false);R.shadowMap.enabled=true;R.shadowMap.type=THREE.PCFSoftShadowMap;R.outputEncoding=THREE.sRGBEncoding;
$('wrap').insertBefore(R.domElement,$('wrap').firstChild);
SC=new THREE.Scene();SC.background=tex(8,256,(g,w,h)=>{const q=g.createLinearGradient(0,0,0,h);q.addColorStop(0,'#4fb3ff');q.addColorStop(1,'#ffe9b8');g.fillStyle=q;g.fillRect(0,0,w,h)});SC.fog=new THREE.Fog(0xbfe6ff,55,130);
CM=new THREE.PerspectiveCamera(48,2,.1,200);
SC.add(new THREE.HemisphereLight(0xffffff,0xb0a0ff,.95));
const L=new THREE.DirectionalLight(0xffffff,.8);L.position.set(-8,22,10);L.castShadow=true;L.shadow.mapSize.set(2048,2048);const q=L.shadow.camera;q.left=-24;q.right=24;q.top=16;q.bottom=-16;q.near=1;q.far=60;SC.add(L);
const gm=new THREE.DataTexture(Uint8Array.from([120,190,255]),3,1,THREE.LuminanceFormat);gm.minFilter=gm.magFilter=THREE.NearestFilter;gm.needsUpdate=true;
T=(c,o)=>new THREE.MeshToonMaterial(Object.assign({color:c,gradientMap:gm},o));OL=new THREE.MeshBasicMaterial({color:0x1a1040,side:THREE.BackSide});
const chk=tex(64,64,g=>{g.fillStyle='#7fe0c0';g.fillRect(0,0,64,64);g.fillStyle='#93ecd0';g.fillRect(0,0,32,32);g.fillRect(32,32,32,32)});chk.wrapS=chk.wrapT=THREE.RepeatWrapping;chk.repeat.set(45,30);
const court=tex(1200,400,(g,w,h)=>{g.fillStyle='#4aa8ff';g.fillRect(0,0,w/2,h);g.fillStyle='#ff7a8a';g.fillRect(w/2,0,w/2,h);const u=w/30;g.fillStyle='rgba(255,255,255,.22)';g.fillRect(w/2-5*u,0,10*u,h);g.strokeStyle='#fff';g.lineWidth=8;g.strokeRect(4,4,w-8,h-8);[0,-5,5].forEach(a=>{g.beginPath();g.moveTo(w/2+a*u,0);g.lineTo(w/2+a*u,h);g.stroke()});g.beginPath();g.arc(w/2,h/2,34,0,7);g.stroke()});
const fl=new THREE.Mesh(new THREE.PlaneGeometry(90,60),T(0xffffff,{map:chk}));fl.rotation.x=-Math.PI/2;fl.receiveShadow=true;SC.add(fl);
const ct=new THREE.Mesh(new THREE.PlaneGeometry(30,10),T(0xffffff,{map:court}));ct.rotation.x=-Math.PI/2;ct.position.y=.01;ct.receiveShadow=true;SC.add(ct);
const cr=new THREE.InstancedMesh(new THREE.SphereGeometry(.38,10,8),T(0xffffff),360),cc=[0xffd23f,0xff6ad5,0x3ddc97,0x7c4dff,0xff8a3d,0x2de2e6,0xffffff],mt=new THREE.Matrix4();let n=0;
[[0,-1,17,60],[0,1,17,60],[-1,0,24,40],[1,0,24,40]].forEach(([ax,az,d,len])=>{for(let r=0;r<3;r++){const dist=d+r*1.4,y=.9+r*1.1;
 mesh(new THREE.BoxGeometry(az?len:1.2,.6,ax?len:1.2),T(0x5b3df5),ax*dist,y-.5,az*dist,SC,0);
 for(let i=0;i<30;i++){const u=(i/29-.5)*len*.9;mt.setPosition(ax*dist+(az?u:0),y+.2,az*dist+(ax?u:0));cr.setMatrixAt(n,mt);cr.setColorAt(n,new THREE.Color(cc[rnd()*7|0]));n++}}});
SC.add(cr);
const nt=tex(256,64,g=>{g.strokeStyle='rgba(255,255,255,.9)';g.lineWidth=2;for(let i=0;i<=256;i+=8){g.beginPath();g.moveTo(i,0);g.lineTo(i,64);g.stroke()}for(let j=0;j<=64;j+=8){g.beginPath();g.moveTo(0,j);g.lineTo(256,j);g.stroke()}});
const net=new THREE.Mesh(new THREE.PlaneGeometry(10.4,2.33),new THREE.MeshBasicMaterial({map:nt,transparent:true,side:THREE.DoubleSide,depthWrite:false}));net.rotation.y=Math.PI/2;net.position.y=1.165;SC.add(net);
mesh(new THREE.BoxGeometry(.1,.16,10.4),T(0xffffff),0,2.33,0,SC);
[-5.3,5.3].forEach(z=>{mesh(new THREE.CylinderGeometry(.1,.1,2.7,12),T(0xffd23f),0,1.35,z,SC);mesh(new THREE.SphereGeometry(.16,10,8),T(0xff3b4a),0,2.75,z,SC)});
bm=mesh(new THREE.SphereGeometry(.42,32,24),T(0xffffff,{map:tex(256,128,(g,w,h)=>{g.fillStyle='#fff';g.fillRect(0,0,w,h);for(let i=0;i<8;i++){g.fillStyle=i%3===0?'#ff3b4a':i%3===1?'#2f80ff':'#ffd23f';g.fillRect(i*w/8,0,w/8-8,h)}})}),0,0,0,SC,1.06);
bsh=new THREE.Mesh(new THREE.CircleGeometry(.5,24),new THREE.MeshBasicMaterial({color:0,transparent:true,opacity:.3}));bsh.rotation.x=-Math.PI/2;SC.add(bsh)}
function buildP(){pm.forEach(o=>SC.remove(o));pm=[];const sk=T(0xffd4a8),wh=T(0xffffff),dk=T(0x222244),HC=[0xffd23f,0x3ddc97,0xff6ad5,0x7c4dff,0xff8a3d,0x2de2e6];
P.forEach((p,i)=>{const out=new THREE.Group(),m=new THREE.Group();out.add(m);const jer=T(p.t?0xff3b4a:0x2f80ff),hair=T(HC[i]);
const a=(g,mt,x,y,z,par,ol)=>mesh(g,mt,x,y,z,par||m,ol);
[-.17,.17].forEach(z=>{a(new THREE.BoxGeometry(.42,.14,.26),dk,.06,.07,z);a(new THREE.CylinderGeometry(.09,.09,.4,10),sk,0,.34,z)});
a(new THREE.CylinderGeometry(.2,.2,.3,12),wh,0,.7,0).scale.z=1.35;
a(new THREE.CylinderGeometry(.26,.3,.62,12),jer,0,1.1,0).scale.z=1.3;
a(new THREE.SphereGeometry(.45,20,16),sk,0,1.72,0);
a(new THREE.SphereGeometry(.48,20,16),hair,-.08,1.8,0).scale.set(1,.8,1);
a(new THREE.ConeGeometry(.18,.4,6),hair,-.02,2.3,0);
[-.17,.17].forEach(z=>{a(new THREE.SphereGeometry(.12,10,8),wh,.36,1.72,z,m,0);a(new THREE.SphereGeometry(.065,8,6),dk,.46,1.72,z,m,0)});
const arms=[-.38,.38].map(z=>{const g=new THREE.Group();g.position.set(0,1.35,z);m.add(g);a(new THREE.CylinderGeometry(.085,.075,.55,8),jer,0,-.27,0,g);a(new THREE.SphereGeometry(.12,10,8),sk,0,-.57,0,g);return g});
out.userData={m,arms};
if(p.human){const c=new THREE.Mesh(new THREE.ConeGeometry(.28,.6,4),new THREE.MeshBasicMaterial({color:0xffd23f}));c.rotation.x=Math.PI;c.position.y=3.2;out.add(c);out.userData.mark=c}
SC.add(out);pm.push(out)})}
function sync3(t){
P.forEach((p,i)=>{const o=pm[i],u=o.userData;o.position.set(p.x,p.y,p.z);
const tg=Math.atan2(-p.fz,p.fx),df=Math.atan2(Math.sin(tg-p.ry),Math.cos(tg-p.ry));p.ry+=df*.25;o.rotation.y=p.ry;
let lean=0,off=0;if(p.stun>0){lean=1.3;off=.3}else if(p.dv>0){lean=-1.25;off=.35}
u.m.rotation.z=lean;u.m.position.y=off;
const up=p.act&&p.dv<=0?3:(p.y>0?2.3:.2+.15*Math.sin(t*.01+i));u.arms.forEach(a=>a.rotation.z+=(up-a.rotation.z)*.35);
if(u.mark){u.mark.rotation.y+=.05;u.mark.position.y=3+.1*Math.sin(t*.006)}});
bm.position.set(ball.x,ball.y,ball.z);bsh.position.set(ball.x,.03,ball.z);bsh.scale.setScalar(1/(1+ball.y*.12));bm.rotation.z-=ball.vx*1.2;bm.rotation.x+=ball.vz*1.2;
const h=P[1],c=Math.cos(camYaw),sn=Math.sin(camYaw),tx=h.x*.6,tz=h.z*.6;cam.x+=(tx-cam.x)*.1;cam.z+=(tz-cam.z)*.1;const q=shake*.02;
CM.position.set(cam.x-c*13+(rnd()-.5)*q,6.5+(rnd()-.5)*q,cam.z-sn*13);CM.lookAt(cam.x,2,cam.z)}
function hud(){X.clearRect(0,0,900,450);
const rr=(x,y,w,h,r,c)=>{X.fillStyle=c;X.beginPath();X.moveTo(x+r,y);X.arcTo(x+w,y,x+w,y+h,r);X.arcTo(x+w,y+h,x,y+h,r);X.arcTo(x,y+h,x,y,r);X.arcTo(x,y,x+w,y,r);X.fill();X.lineWidth=3;X.strokeStyle='#fff';X.stroke()};
rr(335,10,230,46,10,'#2b1f6b');rr(335,10,70,46,10,'#2f80ff');rr(495,10,70,46,10,'#ff3b4a');
X.fillStyle='#fff';X.textAlign='center';X.font='30px Lilita One, Arial';X.fillText(score[0],370,44);X.fillText(score[1],530,44);X.font='14px Lilita One, Arial';X.fillText('SEN',370,24);X.fillText('BOT',530,24);X.font='26px Lilita One, Arial';X.fillText('–',450,42);X.font='11px Lilita One, Arial';X.fillText(WIN+' gacha',450,22);
if(fr>0){const s=sv?'Raqib serve qiladi':'Sen serve qilasan (Bump tugmasi)';X.font='22px Lilita One, Arial';X.lineWidth=5;X.strokeStyle='rgba(0,0,0,.7)';X.strokeText(s,450,100);X.fillStyle='#fff';X.fillText(s,450,100)}
pops.forEach(q=>{q.t--;const v=new THREE.Vector3(q.x,q.y+(40-q.t)*.03,q.z).project(CM);X.save();X.translate((v.x*.5+.5)*900,(-v.y*.5+.5)*450);X.globalAlpha=Math.min(1,q.t/20);X.font='italic 30px Lilita One, Arial';X.lineWidth=6;X.strokeStyle='rgba(0,0,0,.8)';X.strokeText(q.s,0,0);X.fillStyle=/BREAK|SPIKE|POINT/.test(q.s)?'#ffd23f':'#fff';X.fillText(q.s,0,0);X.restore()});
pops=pops.filter(q=>q.t>0);
AB.forEach((a,i)=>{const s=ab[a.id],x=12+i*88,y=402,own=S.own[a.id];
rr(x,y,82,40,6,own?'#3b2a95':'rgba(60,60,60,.7)');
if(own&&s.cd>0){X.fillStyle='rgba(255,255,255,.18)';X.fillRect(x,y,82*s.cd/3600,40)}
if(s.on>0||s.armed>0){X.fillStyle='#ffd23f';X.fillRect(x,y+36,82,4)}
X.fillStyle='#fff';X.textAlign='center';X.font='12px Lilita One, Arial';X.fillText(a.n,x+41,y+16);X.font='11px Lilita One, Arial';X.fillStyle='#bcd';X.fillText(own?'['+lab(B['a'+(i+1)])+']'+(s.cd>0?' '+Math.ceil(s.cd/60)+'s':''):'🔒 market',x+41,y+31)})}
function draw(){const t=performance.now();if(shake>0){shake*=.85;if(shake<.5)shake=0}sync3(t);R.render(SC,CM);hud()}
let last=0,acc=0;
function loop(t){requestAnimationFrame(loop);if(scr!=='game'){last=t;return}
acc+=Math.min(50,t-last);last=t;while(acc>=16.67&&scr==='game'){step();acc-=16.67}if(scr==='game'&&!over)draw()}
requestAnimationFrame(loop);show('menu');
    </script>
</body>

</html>
