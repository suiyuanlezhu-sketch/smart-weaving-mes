<template>
<div class="page">

<h1>织布生产智能驾驶舱 V9.3</h1>
<p class="sub">设备实时产量 · 生产效率 · 异常报警中心</p>


<div class="summary">

<div class="card">
<h2>240台</h2>
<p>总织布机</p>
</div>

<div class="card">
<h2>28560米</h2>
<p>今日产量</p>
</div>

<div class="card">
<h2>91.5%</h2>
<p>平均生产效率</p>
</div>

<div class="card warning">
<h2>{{alarms.length}}台</h2>
<p>异常停机</p>
</div>

</div>


<div class="panel">

<h2>设备类型产能分析</h2>

<div class="groups">

<div v-for="g in groups" class="group">

<h3>{{g.name}}</h3>

<p>设备数量：{{g.total}}台</p>
<p>今日产量：{{g.output}}米</p>
<p>平均效率：{{g.efficiency}}%</p>

<div class="progress">
<span :style="{width:g.efficiency+'%'}"></span>
</div>

</div>

</div>

</div>



<div class="panel">

<h2>生产异常报警</h2>

<div v-for="a in alarms" class="alarm">
⚠ {{a.machine}} 连续停机 {{a.time}} 分钟，请检查设备
</div>

</div>


<div class="panel">

<h2>单机实时生产数据</h2>

<div class="machines">

<div class="machine" v-for="m in machines">

<div class="gear" :class="m.status">⚙</div>

<b>{{m.id}}</b>

<p>{{m.type}}</p>

<p>{{m.statusText}}</p>

<p>规格：{{m.spec}}</p>

<p>日产量：{{m.output}}米</p>

<p>运行：{{m.hours}}小时</p>

<p>效率：{{m.efficiency}}%</p>

</div>

</div>

</div>


</div>
</template>


<script setup>

const groups=[
{name:"梭机",total:120,output:14500,efficiency:92},
{name:"大喷织机",total:80,output:9800,efficiency:94},
{name:"小喷织机",total:40,output:4260,efficiency:89}
]


const alarms=[
{machine:"SJ-023",time:35},
{machine:"DP-008",time:22},
{machine:"XP-015",time:41}
]


const machines=[]

for(let i=1;i<=240;i++){

let type=i<=120?"梭机":i<=200?"大喷织机":"小喷织机"

let status=i%10<7?"run":i%10<9?"idle":"repair"

machines.push({

id:type==="梭机"?"SJ-"+i:type==="大喷织机"?"DP-"+(i-120):"XP-"+(i-200),

type,

status,

statusText:
status==="run"?"生产中":
status==="idle"?"空闲":"维修",

spec:type==="梭机"?"40S/120":type==="大喷织机"?"50S/140":"32S/108",

output:status==="run"?680:0,

hours:status==="run"?8.5:0,

efficiency:status==="run"?92:0

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

.page{
padding:30px
}

.sub{
color:#9db2d5
}

.summary{
display:grid;
grid-template-columns:repeat(4,1fr);
gap:16px
}

.card,.panel{
background:#101d34;
border:1px solid #1c3154;
border-radius:18px;
padding:20px;
margin-top:20px
}

.warning h2{
color:#ef4444
}

.groups{
display:grid;
grid-template-columns:repeat(3,1fr);
gap:20px
}

.group{
background:#172842;
padding:20px;
border-radius:15px
}

.progress{
height:10px;
background:#263a58
}

.progress span{
display:block;
height:100%;
background:#22c55e
}

.alarm{
background:#351b25;
color:#ff9b9b;
padding:15px;
border-radius:10px;
margin:10px 0
}

.machines{
display:grid;
grid-template-columns:repeat(10,1fr);
gap:10px
}

.machine{
background:#172842;
padding:10px;
border-radius:10px;
font-size:12px
}

.gear{
font-size:25px
}

.run{
color:#22c55e;
animation:spin 2s linear infinite
}

.idle{
color:#94a3b8
}

.repair{
color:#ef4444;
animation:blink 1s infinite
}

@keyframes spin{
to{transform:rotate(360deg)}
}

@keyframes blink{
50%{opacity:.3}
}

</style>
