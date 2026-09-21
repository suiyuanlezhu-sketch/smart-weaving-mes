<template>
<div class="page">
<h1>单机实时生产数据</h1>

<div class="machines">

<div class="machine" v-for="m in machines">

<div class="status-head">

<div v-if="m.status==='run'" class="running-icon">
<span class="dot"></span>
⚙
</div>

<div v-else class="normal-icon" :class="m.status">
⚙
</div>

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

<p>任务编号：{{m.orderNo}}</p>

</div>


<p>今日产量：{{m.output}}米</p>

</div>

</div>
</div>
</template>

<script setup>
const machines=[]

for(let i=1;i<=40;i++){

const stock=i%3===0

machines.push({
id:'SJ-'+String(i).padStart(3,'0'),
type:'梭机',
status:'run',
statusText:'生产中',
spec:'40S/120',
taskType:stock?'stock':'order',
sales:stock?'':'张经理',
orderNo:stock?'备货-'+i:'DD2026-'+i,
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

.page{
padding:30px
}

.machines{
display:grid;
grid-template-columns:repeat(5,1fr);
gap:15px
}

.machine{
background:#172842;
padding:15px;
border-radius:14px
}

.status-head{
display:flex;
align-items:center;
gap:10px
}

.running-icon{
width:34px;
height:34px;
display:flex;
align-items:center;
justify-content:center;
font-size:22px;
color:#22c55e;
position:relative;
animation:rotate 2s linear infinite;
}

.running-icon .dot{
position:absolute;
right:0;
top:0;
width:7px;
height:7px;
border-radius:50%;
background:#22c55e;
animation:pulse 1s infinite;
}

.normal-icon{
font-size:22px
}

.task{
margin-top:12px;
padding:10px;
border-radius:8px;
background:#223755
}

.task.stock{
border-left:4px solid #38bdf8
}

.task.order{
border-left:4px solid #22c55e
}

@keyframes rotate{
from{transform:rotate(0deg)}
to{transform:rotate(360deg)}
}

@keyframes pulse{
0%{transform:scale(.8);opacity:.5}
50%{transform:scale(1.5);opacity:1}
100%{transform:scale(.8);opacity:.5}
}

</style>
