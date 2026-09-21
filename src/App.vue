<template>
<div class="layout">
<aside>
<h2>Smart MES</h2>
<div class="nav" v-for="n in nav">{{n}}</div>
</aside>

<main>
<h1>织布生产智能驾驶舱 V10.1.3</h1>

<div class="panel">
<h2>单机实时生产数据</h2>

<div class="machines">

<div class="machine" v-for="m in machines">

<div class="status">
<div v-if="m.status==='run'" class="gear-box">
<div class="gear">⚙</div>
<div class="pulse"></div>
</div>
<div v-else class="gear-static">⚙</div>

<b>{{m.id}}</b>
</div>

<p>设备：{{m.type}}</p>
<p>状态：{{m.statusText}}</p>
<p>规格：{{m.spec}}</p>

<div class="task">
<strong>当前生产任务</strong>
<p>{{m.task}}</p>
<p v-if="m.owner">业务员：{{m.owner}}</p>
</div>

<p>日产量：{{m.output}}米</p>

</div>

</div>
</div>
</main>
</div>
</template>

<script setup>
const nav=[
"生产驾驶舱",
"织布机管理",
"二维码管理",
"产品规格库",
"订单管理",
"业务员管理",
"工人管理",
"生产记录",
"产量统计",
"报表导出"
]

const machines=[]

for(let i=1;i<=50;i++){
machines.push({
id:"SJ-"+String(i).padStart(3,'0'),
type:"梭机",
status:i%2?'run':'idle',
statusText:i%2?'生产中':'空闲',
spec:"40S/120",
task:i%2?'📋业务员订单':'📦常规备货',
owner:i%2?'张经理':'',
output:680
})
}
</script>

<style>
body{
margin:0;
background:#06111f;
color:white;
font-family:"Microsoft YaHei"
}
.layout{
display:flex;
min-height:100vh
}
aside{
width:230px;
background:#081525;
padding:20px
}
.nav{
background:#14243d;
padding:12px;
margin:8px 0;
border-radius:10px
}
main{
flex:1;
padding:25px
}
.panel{
background:#101d34;
padding:20px;
border-radius:18px
}
.machines{
display:grid;
grid-template-columns:repeat(5,1fr);
gap:15px
}
.machine{
background:#172842;
padding:15px;
border-radius:12px
}
.status{
display:flex;
align-items:center;
gap:12px
}

/* 新版固定尺寸动画，不影响布局 */
.gear-box{
width:32px;
height:32px;
position:relative;
display:flex;
align-items:center;
justify-content:center;
}

.gear{
font-size:24px;
line-height:32px;
color:#22c55e;
animation:gearRotate 1.5s linear infinite;
transform-origin:center center;
will-change:transform;
}

.pulse{
position:absolute;
width:8px;
height:8px;
right:0;
top:0;
border-radius:50%;
background:#22c55e;
animation:pulse 1s infinite;
}

.gear-static{
font-size:24px;
color:#94a3b8;
}

@keyframes gearRotate{
0%{
transform:rotate(0deg);
}
100%{
transform:rotate(360deg);
}
}

@keyframes pulse{
0%{
opacity:.4;
transform:scale(.8);
}
50%{
opacity:1;
transform:scale(1.6);
}
100%{
opacity:.4;
transform:scale(.8);
}
}

.task{
background:#223755;
padding:10px;
border-radius:8px;
margin-top:10px
}
</style>