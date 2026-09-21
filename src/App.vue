<template>
<div class="layout">

<aside class="sidebar">

<div class="logo">
<div class="icon">MES</div>
<div>
<h3>织布生产管理系统</h3>
<p>Smart Weaving MES</p>
</div>
</div>

<div class="title">主导航</div>

<div class="nav active">生产驾驶舱</div>
<div class="nav">织布机管理</div>
<div class="nav">二维码管理</div>
<div class="nav">产品与规格库</div>
<div class="nav">订单管理</div>
<div class="nav">业务员管理</div>
<div class="nav">工人管理</div>
<div class="nav">生产记录</div>
<div class="nav">产量统计</div>
<div class="nav">报表导出</div>

</aside>


<main>

<h1>生产智能驾驶舱</h1>
<p class="sub">订单生产 · 常规备货 · 设备实时管理</p>


<div class="cards">
<div class="card" v-for="c in cards">
<h2>{{c.v}}</h2>
<p>{{c.n}}</p>
</div>
</div>


<div class="panel">

<h2>设备分类实时状态</h2>

<div class="groups">

<div class="group" v-for="g in groups">

<h3>{{g.name}}</h3>

<p>总设备：{{g.total}}台</p>
<p class="green">生产中：{{g.running}}台</p>
<p>空闲率：{{g.idle}}%</p>
<p>今日产量：{{g.output}}米</p>

</div>

</div>

</div>


<div class="panel">

<h2>单机实时生产数据</h2>

<div class="machines">

<div class="machine" v-for="m in machines">

<div class="head">
<span class="gear" :class="m.status">⚙</span>
<b>{{m.id}}</b>
</div>

<p>设备：{{m.type}}</p>
<p>状态：{{m.statusText}}</p>
<p>规格：{{m.spec}}</p>


<div class="task" :class="m.taskType">

<strong>当前生产任务</strong>

<p v-if="m.taskType==='stock'">
📦 常规备货
</p>

<p v-else>
📋 业务员订单
</p>

<p v-if="m.sales">
业务员：{{m.sales}}
</p>

<p>
任务编号：{{m.orderNo}}
</p>

</div>


<p>今日产量：{{m.output}}米</p>
<p>运行时间：{{m.hours}}小时</p>

</div>

</div>

</div>


</main>

</div>
</template>


<script setup>

const cards=[
{n:"总织布机",v:"240台"},
{n:"生产中",v:"165台"},
{n:"订单生产",v:"105台"},
{n:"今日产量",v:"28560米"}
]


const groups=[
{name:"梭机",total:120,running:82,idle:23.3,output:14500},
{name:"大喷织机",total:80,running:56,idle:22.5,output:9800},
{name:"小喷织机",total:40,running:27,idle:25,output:4260}
]


const machines=[]

for(let i=1;i<=60;i++){

let stock=i%3===0

machines.push({

id:i<=30?"SJ-"+i:i<=50?"DP-"+(i-30):"XP-"+(i-50),

type:i<=30?"梭机":i<=50?"大喷织机":"小喷织机",

status:i%10<7?"run":i%10<9?"idle":"repair",

statusText:i%10<7?"生产中":i%10<9?"空闲":"维修",

spec:"40S/120",

taskType:stock?"stock":"order",

sales:stock?"":"张经理",

orderNo:stock?"备货-"+i:"DD2026-"+i,

output:680,

hours:8.5

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

.sidebar{
width:240px;
background:#081525;
padding:20px;
border-right:1px solid #1c3154
}

.logo{
display:flex;
gap:10px;
align-items:center;
padding-bottom:20px;
border-bottom:1px solid #24395f
}

.icon{
background:#2563eb;
padding:12px;
border-radius:10px
}

.logo p,.sub{
color:#8ea4d2
}

.title{
margin:20px 0 10px;
color:#7f95bd
}

.nav{
padding:13px;
margin:8px 0;
background:#111f36;
border-radius:10px
}

.nav.active{
border:1px solid #2563eb;
background:#132848
}

main{
flex:1;
padding:30px
}

.cards{
display:grid;
grid-template-columns:repeat(4,1fr);
gap:15px
}

.card,.panel{
background:#101d34;
padding:20px;
border-radius:18px;
margin-top:20px
}

.groups{
display:grid;
grid-template-columns:repeat(3,1fr);
gap:15px
}

.group,.machine{
background:#172842;
padding:15px;
border-radius:12px
}

.green{
color:#22c55e
}

.machines{
display:grid;
grid-template-columns:repeat(10,1fr);
gap:10px
}

.machine{
font-size:12px
}

.gear{
font-size:18px
}

.run{
color:#22c55e;
animation:spin 2s linear infinite
}

.idle{
color:#94a3b8
}

.repair{
color:#ef4444
}

.task{
margin-top:10px;
padding:10px;
border-radius:8px;
background:#223755
}

.task.stock{
border-left:3px solid #38bdf8
}

.task.order{
border-left:3px solid #22c55e
}

@keyframes spin{
to{transform:rotate(360deg)}
}

</style>
