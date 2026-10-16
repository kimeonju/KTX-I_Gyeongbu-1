<!DOCTYPE html>
<html lang="ko">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width,initial-scale=1">
<title>ERR · Eco Rapid Rail</title>
<style>
:root{color-scheme:dark;--bg:#0b1018;--panel:#151e2b;--line:#304154;--text:#edf4fc;--muted:#a7b8cb;--green:#47e59b;--yellow:#ffd166;--red:#ff6474;--blue:#5bb9ff}
*{box-sizing:border-box}
body{margin:0;background:var(--bg);color:var(--text);font:14px/1.5 system-ui,sans-serif}
header{padding:18px 20px;background:#111b29;border-bottom:1px solid var(--line)}
header h1{font-size:23px;margin:0}header p{color:var(--muted);margin:5px 0 0}
main{max-width:1450px;margin:auto;padding:14px;display:grid;grid-template-columns:repeat(12,minmax(0,1fr));gap:12px}
.panel{background:var(--panel);border:1px solid var(--line);border-radius:12px;padding:14px;min-width:0}
.s12{grid-column:span 12}.s8{grid-column:span 8}.s6{grid-column:span 6}.s4{grid-column:span 4}
h2{font-size:16px;margin:0 0 12px}h3{font-size:14px;margin:13px 0 7px}
.muted,small{color:var(--muted)}
.row{display:flex;align-items:center;gap:8px;flex-wrap:wrap}.between{justify-content:space-between}
.g2{display:grid;grid-template-columns:repeat(2,minmax(0,1fr));gap:8px}
.g3{display:grid;grid-template-columns:repeat(3,minmax(0,1fr));gap:8px}
.stat{background:#0d1520;border:1px solid var(--line);padding:10px;border-radius:9px;min-width:0}
.stat small{display:block}.stat strong{font-size:17px}
.big{font-size:44px;font-weight:800;line-height:1.2}.unit{color:var(--muted)}
button,input,select{background:#0b1420;color:var(--text);border:1px solid #41566d;border-radius:7px;padding:9px;font:inherit}
button{cursor:pointer}button:hover{border-color:var(--blue)}button.active{background:#174b3a;border-color:var(--green)}
button.danger{border-color:#943947}button.warn{border-color:#8a5d25}
input[type=number]{width:100%;min-width:60px}select{max-width:100%}
label{display:block;color:var(--muted);margin:7px 0 4px}
.progress{height:12px;background:#08101a;border-radius:99px;overflow:hidden;border:1px solid var(--line)}
.progress span{display:block;height:100%;background:var(--green);width:0%}
.badge{display:inline-block;padding:3px 8px;border-radius:99px;background:#27364a}
.good{color:var(--green)}.warntext{color:var(--yellow)}.bad{color:var(--red)}
.track{height:100px;position:relative;background:linear-gradient(#17283a,#0b121d);border-radius:99px;overflow:hidden;border:1px solid var(--line);margin-top:8px}
.rail{position:absolute;left:0;right:0;top:53%;border-top:3px solid #72869c}
.mark{position:absolute;top:30%;width:12px;height:26px;background:var(--blue);border:2px solid #e7f6ff;border-radius:3px;transform:translateX(-50%)}
.mark.user{background:var(--green)}.mark.target{background:var(--yellow)}
.tracklabel{position:absolute;top:5px;font-size:11px;color:var(--muted);transform:translateX(-50%);white-space:nowrap}
table{width:100%;border-collapse:collapse;font-size:12px}
th,td{text-align:left;padding:7px 5px;border-bottom:1px solid var(--line);white-space:nowrap}
th{color:var(--muted)}.scroll{overflow:auto;max-height:310px}
.log{height:145px;overflow:auto;background:#080e16;border:1px solid var(--line);border-radius:8px;padding:8px;font:12px/1.6 monospace}
.log div{border-bottom:1px solid #1b2838;padding:2px}
hr{border:0;border-top:1px solid var(--line);margin:12px 0}
#timetable tr.running,#timetable tr.running td{color:#47e59b}
#timetable tr.completed,#timetable tr.completed td{color:#ff6474}
#timetable tr.pending,#timetable tr.pending td{color:#ffd166}
#otherTrains tr.running,#otherTrains tr.running td{color:#47e59b}
#otherTrains tr.completed,#otherTrains tr.completed td{color:#ff6474}
#otherTrains tr.pending,#otherTrains tr.pending td{color:#ffd166}
@media(max-width:850px){.s8,.s6,.s4{grid-column:span 12}.big{font-size:36px}}
</style>
</head>
<body>
<header>
<h1>ERR · Eco Rapid Rail</h1>
<p>서울 → 부산센텀 · 경고음 반복 재생 및 가감속 시스템 연동 시뮬레이터</p>
</header>
<main>
<section class="panel s8">
<div class="row between"><h2>운전실 · ERR 001</h2><span id="clock" class="badge">--:--:--</span></div>
<div class="g2">
<div class="stat"><small>현재 속도</small><span id="speed" class="big">0</span> km/h<div class="progress"><span id="speedbar"></span></div></div>
<div class="stat"><small>현재 위치</small><strong id="station">서울</strong><div id="distance" class="muted">0.0 km</div>다음 역: <strong id="next">광명</strong></div>
</div>
<div class="g3" style="margin-top:8px">
<div class="stat"><small>노치</small><strong id="notch">N</strong></div>
<div class="stat"><small>선로 제한속도</small><strong id="limit">330 km/h</strong></div>
<div class="stat"><small>목표속도</small><strong id="targetSpeed">330 km/h</strong></div>
<div class="stat"><small>VCB</small><strong id="vcb">OFF</strong></div>
<div class="stat"><small>팬터그래프</small><strong id="pant">하강</strong></div>
<div class="stat"><small>상태</small><strong id="status">정차</strong></div>
</div>
<h3>운전 제어</h3>
<div class="row">
<button id="forward">F · 전진</button><button id="reverse">R · 후진</button>
<button id="vcbButton">O · VCB</button><button id="pantButton">P · 팬터그래프</button>
<button id="coast">C · 타행</button><button id="eb" class="danger">비상제동</button><button id="release">제동 해제</button>
</div>
<h3>역행 / 제동 노치</h3>
<div class="row">
<button data-notch="P1">P1</button><button data-notch="P2">P2</button><button data-notch="P3">P3</button><button data-notch="P4">P4</button><button data-notch="P5">P5</button><button data-notch="N">N</button>
<button data-notch="B1">B1</button><button data-notch="B2">B2</button><button data-notch="B3">B3</button><button data-notch="B4">B4</button><button data-notch="B5">B5</button>
</div>
<p class="muted">P5: +1.8 km/h/s · B5: −1.8 km/h/s · Space: 경계 페달 · T: 추월 모드</p>
</section>

<section class="panel s4">
<h2>추월 모드 · 앞앞차 기준</h2>
<button id="overtake" class="warn">추월 모드 OFF</button>
<label for="baseTrain">기준 열차</label><select id="baseTrain" style="width:100%"></select>
<div class="g2" style="margin-top:8px">
<div class="stat"><small>앞앞차</small><strong id="frontFront">미검출</strong></div>
<div class="stat"><small>간격</small><strong id="gap">- km</strong></div>
<div class="stat"><small>목표 간격</small><strong id="gapSetting">2.0 km</strong></div>
<div class="stat"><small>간격 제어</small><strong id="gapStatus">대기</strong></div>
</div>
<label for="gapInput">목표 간격 (km)</label>
<div class="row"><input id="gapInput" type="number" min=".5" max="20" step=".5" value="2"><button id="setGap">적용</button></div>
<p id="autoExplanation" class="muted">추월 모드 OFF</p>
<div class="stat"><small>목표 제한속도</small><strong id="autoLimit">330 km/h</strong></div>
</section>

<section class="panel s12">
<h2>노선 및 열차 위치</h2><div class="track" id="track"><div class="rail"></div><div id="trackMarks"></div></div>
<div class="row between"><span class="muted">서울 0 km</span><span class="muted">대전 166.3 km</span><span class="muted">부산센텀 417.4 km</span></div>
</section>

<section class="panel s6">
<h2>안전 · 경계장치</h2>
<div class="g2">
<div class="stat"><small>감속 예고</small><strong id="speedWarning" class="good">정상</strong></div>
<div class="stat"><small>속도 초과</small><strong id="overspeed" class="good">정상</strong></div>
<div class="stat"><small>경계장치</small><strong id="vigilance" class="good">정상</strong></div>
<div class="stat"><small>비상제동</small><strong id="emergencyStatus">해제</strong></div>
</div>
<p class="muted">55초 동안 입력이 없으면 경고, 추가 15초 동안 응답이 없으면 비상제동합니다.</p>
<button id="pedal">Space · 경계 페달</button> <span id="pedalTime" class="muted">입력 대기</span>
</section>

<section class="panel s6">
<h2>진로 · 출입문</h2>
<div class="g3"><button id="turnL">4 · 좌측</button><button id="turnS" class="active">5 · 직진</button><button id="turnR">6 · 우측</button></div>
<p>현재 진로: <strong id="turnout">직진</strong></p><hr>
<div class="g2">
<div class="stat"><strong>좌측 출입문</strong><p id="leftStatus" class="muted">닫힘 · 허가 없음</p><button id="permitL">2 · 허가</button> <button id="leftDoor">W · 문</button></div>
<div class="stat"><strong>우측 출입문</strong><p id="rightStatus" class="muted">닫힘 · 허가 없음</p><button id="permitR">3 · 허가</button> <button id="rightDoor">E · 문</button></div>
</div>
<p class="muted">주행 중에는 출입문을 열 수 없습니다.</p>
</section>

<section class="panel s6">
<h2>시간표 · ERR 001</h2>
<p class="muted">시계는 기기 시각, 색상은 역별 운행 진행 상태를 표시합니다.</p>
<div class="scroll"><table><thead><tr><th>역</th><th>도착</th><th>출발</th><th>거리</th><th>상태</th></tr></thead><tbody id="timetable"></tbody></table></div>
</section>

<section class="panel s6">
<h2>다른 KTX 운행 현황</h2>
<div class="scroll"><table><thead><tr><th>열차</th><th>상태</th><th>위치</th><th>속도</th><th>ERR와 거리</th></tr></thead><tbody id="otherTrains"></tbody></table></div>
<p class="muted">일반 KTX 최고 300km/h · 가감속 1km/h/s · 대전/동대구 5분, 그외 3분 정차</p>
</section>
<section class="panel s12"><h2>운행 기록</h2><div id="log" class="log"></div></section>
</main>

<script>
"use strict";
const $=id=>document.getElementById(id);
const LEN=417.4,DAY=86400;

const stations=[
{name:"서울",km:0,limit:330,arr:"",dep:5*3600},
{name:"광명",km:17,limit:330,arr:5*3600+20*60,dep:5*3600+23*60},
{name:"천안아산",km:96.5,limit:330,arr:5*3600+50*60,dep:5*3600+53*60},
{name:"오송",km:131.5,limit:330,arr:6*3600+10*60,dep:6*3600+13*60},
{name:"대전",km:166.3,limit:330,arr:6*3600+30*60,dep:6*3600+35*60},
{name:"김천(구미)",km:227.3,limit:330,arr:7*3600+0*60,dep:7*3600+3*60},
{name:"동대구",km:293.1,limit:330,arr:7*3600+20*60,dep:7*3600+25*60},
{name:"경주",km:346.8,limit:330,arr:7*3600+43*60,dep:7*3600+46*60},
{name:"태화강",km:380,limit:330,arr:8*3600+0*60,dep:8*3600+3*60},
{name:"부산센텀",km:417.4,limit:330,arr:8*3600+23*60,dep:""}
];

let speed=0,distance=0,direction=0,power=0,vcb=false,pantograph=false;
let emergency=false,leftDoor=false,rightDoor=false,allowL=false,allowR=false;
let turnout="직진",stationIndex=0,arrived=false;
let lastFrame=performance.now(),vigilanceLast=performance.now(),warningStart=null;
let overtakeMode=false,targetGap=2,targetSpeed=330,notchLabel="N";
const trains=[],heldKeys=new Set();

let warningSoundInterval = null;

// 경고음 재생 함수 (Web Audio API)
function playWarningSound() {
 try {
  const audioCtx = new (window.AudioContext || window.webkitAudioContext)();
  const osc = audioCtx.createOscillator();
  const gain = audioCtx.createGain();
  osc.type = 'sine';
  osc.frequency.setValueAtTime(880, audioCtx.currentTime); // 삐- 소리 (A5)
  gain.gain.setValueAtTime(0.2, audioCtx.currentTime);
  osc.connect(gain);
  gain.connect(audioCtx.destination);
  osc.start();
  osc.stop(audioCtx.currentTime + 0.4);
 } catch(e) {}
}

function startWarningSound() {
 if (warningSoundInterval) return;
 playWarningSound();
 warningSoundInterval = setInterval(() => {
  if (warningStart !== null && !emergency) {
   playWarningSound();
  } else {
   stopWarningSound();
  }
 }, 1000); // 1초마다 삐- 소리 반복
}

function stopWarningSound() {
 if (warningSoundInterval) {
  clearInterval(warningSoundInterval);
  warningSoundInterval = null;
 }
}

function deviceSeconds(){
 const d=new Date();
 return d.getHours()*3600+d.getMinutes()*60+d.getSeconds()+d.getMilliseconds()/1000;
}
function deviceTimeText(){
 const d=new Date();
 return [d.getHours(),d.getMinutes(),d.getSeconds()].map(v=>String(v).padStart(2,"0")).join(":");
}
function formatSec(sec){
 if(isNaN(sec)||sec==="")return "-";
 let h=Math.floor(sec/3600), m=Math.floor((sec%3600)/60);
 return String(h).padStart(2,"0")+":"+String(m).padStart(2,"0");
}
function syncClock(){$("clock").textContent=deviceTimeText()}
function log(msg){
 const el=document.createElement("div");
 el.textContent="["+deviceTimeText()+"] "+msg;
 $("log").prepend(el);
 while($("log").children.length>100)$("log").lastChild.remove();
}
function currentLimit(){return 330;}
function currentStationName(){
 let result=stations[0].name;
 for(const s of stations)if(s.km<=distance+.02)result=s.name;
 return result;
}
function nextStationIndex(){return stations.findIndex(s=>s.km>distance+.02)}
function setNotch(v){
 if(emergency&&!v.startsWith("B")&&v!=="N")return;
 if(/^P[1-5]$/.test(v)){
  if(!vcb||!pantograph||direction===0){
   power=0;notchLabel="N";log("역행 불가: 방향·VCB·팬터그래프 확인");render();return;
  }
  power=Number(v.slice(1));notchLabel=v;emergency=false;
 }else if(/^B[1-5]$/.test(v)){
  power=-Number(v.slice(1));notchLabel=v;
 }else{power=0;notchLabel="N";}
 render();
}
function setDirection(d){
 if(speed>.1){log("정차 후 방향 변경 필요");return}
 direction=d;log(d===1?"전진 설정":"후진 설정");render();
}
function applyEmergency(){
 emergency=true;power=-5;notchLabel="EB";
 stopWarningSound();
 log("비상제동 작동");
 render();
}
function releaseBrake(){emergency=false;power=0;notchLabel="N";log("비상제동 해제");render()}
function toggleVCB(){vcb=!vcb;if(!vcb){power=0;notchLabel="N"}log("VCB "+(vcb?"ON":"OFF"));render()}
function togglePantograph(){pantograph=!pantograph;if(!pantograph){power=0;notchLabel="N"}log("팬터그래프 "+(pantograph?"올림":"내림"));render()}
function setTurnout(v){turnout=v;render();log("진로 설정: "+v)}
function togglePermission(side){
 if(side==="L"){allowL=!allowL;if(!allowL)leftDoor=false}
 else{allowR=!allowR;if(!allowR)rightDoor=false}
 render();
}
function toggleDoor(side){
 if(speed>.1){log("주행 중 출입문 조작 불가");return}
 if(side==="L"){
  if(!allowL){log("좌측 출입문 허가가 필요합니다");return}
  leftDoor=!leftDoor;
 }else{
  if(!allowR){log("우측 출입문 허가가 필요합니다");return}
  rightDoor=!rightDoor;
 }
 log((side==="L"?"좌측":"우측")+" 출입문 "+((side==="L"?leftDoor:rightDoor)?"열림":"닫힘"));render();
}

function initTrains(){
 for(let n=1;n<=109;n+=2){
  const id="KTX"+String(n).padStart(3,"0");
  trains.push({id,km:-1,speed:0,active:false,departure:5*3600+((n-1)/2)*1200});
  const opt=document.createElement("option");opt.value=id;opt.textContent=id;
  $("baseTrain").appendChild(opt);
 }
 $("baseTrain").value="KTX001";
}

function getTrainState(t, nowSec){
 let elapsed=(nowSec-t.departure+DAY)%DAY;
 if(elapsed<0 || elapsed>25000) return {active:false, km:-1, speed:0};
 
 let currentT=0;
 for(let i=0; i<stations.length-1; i++){
  let startKm=stations[i].km;
  let endKm=stations[i+1].km;
  let dist=endKm - startKm;
  let isLongStop=(stations[i].name==="대전" || stations[i].name==="동대구");
  let stopTime=(i===0)?0:(isLongStop?300:180);
  
  let a=1.0; 
  let vPeak=Math.min(300, Math.sqrt(dist * a * 3600));
  
  let accelTime=vPeak / a;
  let accelDist=0.5 * a * (accelTime * accelTime) / 3600;
  let decelTime=vPeak / a;
  let decelDist=0.5 * a * (decelTime * decelTime) / 3600;
  
  let cruiseDist=Math.max(0, dist - accelDist - decelDist);
  let cruiseTime=cruiseDist / (vPeak / 3600);
  
  let sectionTime=accelTime + cruiseTime + decelTime + stopTime;
  
  if(elapsed >= currentT && elapsed < currentT + sectionTime){
   let relT=elapsed - currentT;
   let curSpd=0; let currentKm=startKm;
   
   if(relT < accelTime){
    curSpd=relT * a;
    currentKm=startKm + 0.5 * a * (relT * relT) / 3600;
   }else if(relT < accelTime + cruiseTime){
    curSpd=vPeak;
    currentKm=startKm + accelDist + (relT - accelTime) * (vPeak / 3600);
   }else if(relT < accelTime + cruiseTime + decelTime){
    let dt=relT - (accelTime + cruiseTime);
    curSpd=Math.max(0, vPeak - dt * a);
    currentKm=endKm - decelDist + curSpd * (dt / 3600) + 0.5 * a * (dt * dt) / 3600;
   }else{
    curSpd=0;
    currentKm=endKm;
   }
   return {active:true, km:currentKm, speed:curSpd};
  }
  currentT += sectionTime;
 }
 if(elapsed >= currentT) return {active:true, km:LEN, speed:0};
 return {active:false, km:-1, speed:0};
}

function updateOtherTrains(){
 const now=deviceSeconds();
 for(const t of trains){
  const st=getTrainState(t, now);
  t.active=st.active; t.km=st.km; t.speed=st.speed;
 }
}

function getTrain(id){return trains.find(t=>t.id===id)}
function getAheadAhead(){
 const base=getTrain($("baseTrain").value);
 if(!base||!base.active)return {base,front:null,gap:null};
 const ahead=trains.filter(t=>t.active&&t.id!==base.id&&t.km>base.km).sort((a,b)=>a.km-b.km);
 const front=ahead[1]||ahead[0]||null;
 return {base,front,gap:front?front.km-distance:null};
}
function calculateTargetSpeed(){
 const limit=330;
 if(!overtakeMode){
  $("autoExplanation").textContent="추월 모드 OFF · 수동 운전";
  $("autoLimit").textContent=limit+" km/h";return limit;
 }
 const info=getAheadAhead();
 if(!info.front){
  $("autoExplanation").textContent="앞앞차 미검출 · 선로 제한속도 적용";
  $("autoLimit").textContent=limit+" km/h";return limit;
 }
 const g=info.gap;let allowed=limit;
 if(g<=.2)allowed=0;
 else if(g<targetGap*.5)allowed=Math.min(limit,60);
 else if(g<targetGap*.75)allowed=Math.min(limit,120);
 else if(g<targetGap)allowed=Math.min(limit,180);
 else if(g<targetGap+1)allowed=Math.min(limit,260);
 $("autoExplanation").textContent=info.front.id+" 기준 · 간격 "+g.toFixed(2)+" km";
 $("autoLimit").textContent=Math.round(allowed)+" km/h";
 return allowed;
}

function updateTimetable(){
 const body=$("timetable");body.innerHTML="";
 stations.forEach((s,i)=>{
  let state,cls;
  if(arrived || distance > s.km + 0.15){
   state="운행 완료"; cls="completed";
  }else if(Math.abs(distance - s.km) <= 0.15){
   if(i === 0 && distance === 0 && speed === 0){
    state="운행 전"; cls="pending";
   } else {
    state="운행 중"; cls="running";
   }
  }else if(distance > s.km){
   state="운행 완료"; cls="completed";
  }else{
   state="운행 전"; cls="pending";
  }
  const tr=document.createElement("tr");tr.className=cls;
  [s.name, formatSec(s.arr), formatSec(s.dep), s.km.toFixed(1)+" km", state].forEach(v=>{
   const td=document.createElement("td");td.textContent=v;tr.appendChild(td);
  });
  body.appendChild(tr);
 });
}

function updateOtherTable(){
 const body=$("otherTrains");body.innerHTML="";
 const now=deviceSeconds();
 for(const t of trains){
  const st=getTrainState(t, now);
  let actualStatus, cls;
  if(st.km >= LEN - 0.1){
   actualStatus="운행 완료"; cls="completed";
  }else if(st.active){
   actualStatus="운행 중"; cls="running";
  }else if(now < t.departure){
   actualStatus="운행 전"; cls="pending";
  }else{
   actualStatus="운행 완료"; cls="completed";
  }

  const row=document.createElement("tr");
  row.className=cls;
  const vals=[t.id, actualStatus, st.active?st.km.toFixed(1)+" km":"-", st.active?Math.round(st.speed)+" km/h":"-",
   st.active?((st.km-distance>=0?"+":"")+(st.km-distance).toFixed(1)+" km"):"-"];
  vals.forEach(v=>{const td=document.createElement("td");td.textContent=v;row.appendChild(td)});
  body.appendChild(row);
 }
}

function updateTrack(){
 const box=$("trackMarks");box.innerHTML="";
 [{n:"서울",k:0},{n:"광명",k:17},{n:"대전",k:166.3},{n:"동대구",k:293.1},{n:"부산센텀",k:LEN}].forEach(p=>{
  const l=document.createElement("div");l.className="tracklabel";l.style.left=p.k/LEN*100+"%";l.textContent=p.n;box.appendChild(l);
 });
 const u=document.createElement("div");u.className="mark user";u.style.left=Math.max(0,Math.min(100,distance/LEN*100))+"%";u.title="ERR 001";box.appendChild(u);
 trains.filter(t=>t.km >= 0 && t.km <= LEN).forEach(t=>{
  const m=document.createElement("div");m.className="mark";m.style.left=t.km/LEN*100+"%";m.title=t.id;box.appendChild(m);
 });
 const info=getAheadAhead();
 if(overtakeMode&&info.front){
  const m=document.createElement("div");m.className="mark target";m.style.left=info.front.km/LEN*100+"%";m.title="앞앞차";box.appendChild(m);
 }
}

function render(){
 $("speed").textContent=Math.round(speed);$("speedbar").style.width=Math.min(100,speed/330*100)+"%";
 $("distance").textContent=distance.toFixed(1)+" km";$("station").textContent=currentStationName();
 const ni=nextStationIndex();$("next").textContent=ni<0?"종착역":stations[ni].name;
 $("notch").textContent=notchLabel;$("limit").textContent="330 km/h";
 $("targetSpeed").textContent=Math.round(targetSpeed)+" km/h";
 $("vcb").textContent=vcb?"ON":"OFF";$("pant").textContent=pantograph?"상승":"하강";
 $("status").textContent=arrived?"운행 완료":emergency?"비상제동":speed>.1?"주행":"정차";
 $("turnout").textContent=turnout;
 $("leftStatus").textContent=(leftDoor?"열림":"닫힘")+" · "+(allowL?"허가":"허가 없음");
 $("rightStatus").textContent=(rightDoor?"열림":"닫힘")+" · "+(allowR?"허가":"허가 없음");
 $("permitL").classList.toggle("active",allowL);$("permitR").classList.toggle("active",allowR);
 $("turnL").classList.toggle("active",turnout==="좌측");$("turnS").classList.toggle("active",turnout==="직진");$("turnR").classList.toggle("active",turnout==="우측");
 $("overtake").textContent="추월 모드 "+(overtakeMode?"ON":"OFF");$("overtake").classList.toggle("active",overtakeMode);
 $("gapSetting").textContent=targetGap.toFixed(1)+" km";$("emergencyStatus").textContent=emergency?"작동":"해제";
 const over=speed>331;$("overspeed").textContent=over?"초과":"정상";$("overspeed").className=over?"bad":"good";
 $("speedWarning").textContent="정상";$("speedWarning").className="good";
 $("vigilance").textContent=warningStart!==null?"경고":"정상";$("vigilance").className=warningStart!==null?"bad":"good";
 const info=getAheadAhead();
 $("frontFront").textContent=info.front?info.front.id:"미검출";
 $("gap").textContent=info.gap===null?"- km":info.gap.toFixed(2)+" km";
 $("gapStatus").textContent=!overtakeMode?"대기":!info.front?"기준 열차 대기":info.gap<=targetGap?"간격 조절":"간격 확보";
 syncClock();
}

function passStations(){
 while(stationIndex<stations.length&&distance>=stations[stationIndex].km){
  if(stationIndex===0){stationIndex++;continue}
  const s=stations[stationIndex];
  if(stationIndex===stations.length-1){
   if(distance>=LEN){distance=LEN;speed=0;power=0;notchLabel="N";arrived=true;log("부산센텀 도착")}
   break;
  }
  if(overtakeMode){stationIndex++;continue}
  if(distance>s.km+.2){stationIndex++;continue}
  break;
 }
}

function tick(now){
 const dt=Math.min((now-lastFrame)/1000,.1);lastFrame=now;
 syncClock();updateOtherTrains();
 const limit=330;targetSpeed=calculateTargetSpeed();
 let acceleration=0;
 if(emergency)acceleration=-5;
 else if(power>0){
  if(vcb&&pantograph&&direction!==0)acceleration=power*0.36; 
  else{power=0;notchLabel="N"}
 }else if(power<0)acceleration=power*0.36;

 if(overtakeMode&&!emergency){
  const info=getAheadAhead();
  if(info.front&&info.gap!==null){
   const g=info.gap;
   if(g<=.2)acceleration=Math.min(acceleration,-1.8);
   else if(g<targetGap*.5)acceleration=Math.min(acceleration,-1.4);
   else if(g<targetGap*.75)acceleration=Math.min(acceleration,-.9);
   else if(g<targetGap)acceleration=Math.min(acceleration,-.36);
  }
  if(speed>targetSpeed+1)acceleration=Math.min(acceleration,-1.8);
 }
 speed=Math.max(0,Math.min(330,speed+acceleration*dt));
 if(speed>limit){speed=Math.max(limit,speed-1.8*dt);if(speed>limit+5&&!emergency)applyEmergency()}
 if(speed>.05&&direction!==0&&!arrived){distance+=direction*speed/3600*dt;distance=Math.max(0,Math.min(LEN,distance))}
 if(distance>=LEN&&direction===1&&!arrived){distance=LEN;speed=0;power=0;notchLabel="N";arrived=true;log("부산센텀 도착")}
 if(distance<=0&&direction===-1){distance=0;speed=0}

 const since=performance.now()-vigilanceLast;
 if(since>55000&&warningStart===null){
  warningStart=performance.now();
  startWarningSound(); // 15초 동안 경고음 반복 재생 시작
  log("경계장치 경고");
 }
 if(warningStart!==null&&performance.now()-warningStart>15000&&!emergency){
  applyEmergency();
  log("경계장치 응답 없음 · 비상제동");
 }

 passStations();render();updateTimetable();updateOtherTable();updateTrack();
 requestAnimationFrame(tick);
}

function pedalPress(){
 vigilanceLast=performance.now();
 warningStart=null;
 stopWarningSound(); // 페달 누르면 경고음 중지
 $("pedalTime").textContent="최근 입력: "+deviceTimeText();
}
function toggleOvertake(){overtakeMode=!overtakeMode;log("추월 모드 "+(overtakeMode?"ON · 앞앞차 기준":"OFF"));render();updateTrack()}

function notchUp(){
 if(power < 0){
  setNotch(power === -1 ? "N" : "B" + (Math.abs(power) - 1));
 } else if(power === 0){
  setNotch("P1");
 } else {
  setNotch("P" + Math.min(5, power + 1));
 }
}

function notchDown(){
 if(power > 0){
  setNotch(power === 1 ? "N" : "P" + (power - 1));
 } else if(power === 0){
  setNotch("B1");
 } else {
  setNotch("B" + Math.min(5, Math.abs(power) + 1));
 }
}

$("forward").addEventListener("click",()=>setDirection(1));
$("reverse").addEventListener("click",()=>setDirection(-1));
$("vcbButton").addEventListener("click",toggleVCB);
$("pantButton").addEventListener("click",togglePantograph);
$("coast").addEventListener("click",()=>setNotch("N"));
$("eb").addEventListener("click",applyEmergency);$("release").addEventListener("click",releaseBrake);
$("turnL").addEventListener("click",()=>setTurnout("좌측"));
$("turnS").addEventListener("click",()=>setTurnout("직진"));
$("turnR").addEventListener("click",()=>setTurnout("우측"));
$("permitL").addEventListener("click",()=>togglePermission("L"));
$("permitR").addEventListener("click",()=>togglePermission("R"));
$("leftDoor").addEventListener("click",()=>toggleDoor("L"));
$("rightDoor").addEventListener("click",()=>toggleDoor("R"));
$("pedal").addEventListener("click",pedalPress);
$("overtake").addEventListener("click",toggleOvertake);
$("setGap").addEventListener("click",()=>{
 const v=Number($("gapInput").value);
 if(Number.isFinite(v)&&v>=.5&&v<=20){targetGap=v;log("목표 간격 "+v.toFixed(1)+" km");render()}
 else alert("목표 간격은 0.5~20km로 입력하세요.");
});
$("baseTrain").addEventListener("change",()=>{render();updateTrack()});
document.querySelectorAll("[data-notch]").forEach(b=>b.addEventListener("click",()=>setNotch(b.dataset.notch)));
document.addEventListener("keydown",e=>{
 if(["INPUT","SELECT","TEXTAREA"].includes(document.activeElement.tagName))return;
 if(e.code==="Space"){e.preventDefault();if(!heldKeys.has("Space"))pedalPress();heldKeys.add("Space");return}
 if(e.code==="ArrowUp"||e.code==="ArrowDown"){e.preventDefault();}
 if(heldKeys.has(e.code))return;heldKeys.add(e.code);
 const k=e.key.toLowerCase();
 if(k==="f")setDirection(1);else if(k==="r")setDirection(-1);
 else if(k==="o")toggleVCB();else if(k==="p")togglePantograph();
 else if(k==="c")setNotch("N");else if(k==="t")toggleOvertake();
 else if(k==="w")toggleDoor("L");else if(k==="e")toggleDoor("R");
 else if(k==="a")togglePermission("L");else if(k==="d")togglePermission("R");
 else if(k==="4")setTurnout("좌측");else if(k==="5")setTurnout("직진");else if(k==="6")setTurnout("우측");
 else if(e.key==="ArrowUp") notchUp();
 else if(e.key==="ArrowDown") notchDown();
});
document.addEventListener("keyup",e=>heldKeys.delete(e.code));
window.addEventListener("blur",()=>heldKeys.clear());

initTrains();
syncClock();
updateOtherTrains();
updateTimetable();
updateOtherTable();
updateTrack();
render();
log("시뮬레이터 시작 · 경고음 반복 재생 및 규정 적용 완료");
requestAnimationFrame(tick);
</script>
</body>
</html>
