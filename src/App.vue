<template>
<div class="layout">

<aside>
<h2>织布MES V6.0</h2>
<div v-for="m in menus" class="menu">{{m}}</div>
</aside>

<main>

<h1>生产智能驾驶舱</h1>
<p class="sub">订单生产 · 常规备货 · 设备状态实时管理</p>

<div class="cards">
<div class="card" v-for="c in cards">
<h2>{{c.value}}</h2>
<p>{{c.name}}</p>
</div>
</div>


<div class="panel">
<h2>当前生产任务分布</h2>
<div class="task">
<div>业务员订单：{{salesOrderMachines}}台</div>
<div>常规备货：{{stockMachines}}台</div>
</div>
</div>


<div class="panel" v-for="g in groups">

<h2>{{g.name}} {{g.total}}台</h2>

<div class="state">
<span class="green">🟢生产中 {{g.running}}</span>
<span class="gray">⚪空闲 {{g.idle}}</span>
<span class="red">🔴维修 {{g.repair}}</span>
</div>


<div class="machines">

<div class="machine" v-for="m in g.machines">

<div class="gear" :class="m.status">⚙</div>

<h3>{{m.id}}</h3>

<p>{{m.statusText}}</p>

<p>产品：{{m.product}}</p>

<p>规格：{{m.spec}}</p>

<p class="task-name">
任务：{{m.task}}
</p>

<p v-if="m.sales">
业务员：{{m.sales}}
</p>

<button @click="selected=m">
查看详情
</button>

</div>

</div>

</div>


<div class="dialog" v-if="selected">

<h2>{{selected.id}}</h2>

<p>生产状态：{{selected.statusText}}</p>

<p>生产任务：{{selected.task}}</p>

<p>业务员：{{selected.sales || '无'}}</p>

<p>产品：{{selected.product}}</p>

<p>规格：{{selected.spec}}</p>

<button @click="selected=null">关闭</button>

</div>


</main>

</div>
</template>


<script setup>

import {ref} from 'vue'


const menus=[
'生产驾驶舱',
'机器管理',
'二维码管理',
'业务员管理',
'生产任务管理',
'产品规格库',
'工人管理',
'生产记录',
'产量统计'
]


const cards=[
{name:'总织布机',value:'240台'},
{name:'生产中',value:'165台'},
{name:'订单生产',value:'105台'},
{name:'常规备货',value:'60台'},
{name:'今日产量',value:'28560米'}
]


const sales=[
'张经理',
'李经理',
'王经理'
]


function createMachines(prefix,count,spec){

let arr=[]

for(let i=1;i<=count;i++){

let running=i%10<7

let status=running?'run':i%10<9?'idle':'repair'

let order=i%2===0


arr.push({

id:prefix+String(i).padStart(3,'0'),

status,

statusText:
status==='run'?'生产中':
status==='idle'?'空闲':'维修',

product:
status==='repair'?'设备维护':'纯棉布',

spec,

task:
status==='run'
?(order?'业务员订单':'常规备货')
:'',

sales:
status==='run'&&order
?sales[i%3]
:''

})

}

return arr

}



const groups=[

{
name:'梭机',
total:120,
running:82,
idle:28,
repair:10,
machines:createMachines('SJ-',120,'40S/120')
},

{
name:'大喷织机',
total:80,
running:56,
idle:18,
repair:6,
machines:createMachines('DP-',80,'50S/140')
},

{
name:'小喷织机',
total:40,
running:27,
idle:10,
repair:3,
machines:createMachines('XP-',40,'32S/108')
}

]


const salesOrderMachines=105

const stockMachines=60

const selected=ref(null)

</script>


<style>
body{
margin:0;
background:#07111f;
color:white;
font-family:"Microsoft YaHei"
}

.layout{
display:flex;
min-height:100vh
}

aside{
width:240px;
background:#0d182b;
padding:20px
}

.menu{
background:#16253d;
padding:13px;
margin:8px 0;
border-radius:10px
}

main{
flex:1;
padding:25px
}

.cards{
display:flex;
gap:15px;
flex-wrap:wrap
}

.card,.panel{
background:#111e33;
border-radius:16px;
padding:20px;
margin:15px 0
}

.card{
width:150px
}

.task{
display:flex;
gap:30px;
font-size:20px
}

.machines{
display:grid;
grid-template-columns:repeat(5,1fr);
gap:12px
}

.machine{
background:#17263e;
padding:14px;
border-radius:12px
}

.gear{
font-size:30px
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

.green{color:#22c55e}
.gray{color:#94a3b8}
.red{color:#ef4444}

.task-name{
color:#60a5fa
}

.dialog{
position:fixed;
right:30px;
top:100px;
background:white;
color:black;
padding:25px;
border-radius:15px
}

@keyframes spin{
to{transform:rotate(360deg)}
}

@keyframes blink{
50%{opacity:.3}
}

button{
padding:6px 12px
}
</style>
