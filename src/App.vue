<template>
<div class="page">
<h1>织布生产智能驾驶舱</h1>

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

<!-- 新增订单归属展示 -->
<div class="task-box" :class="m.taskType">
<p class="task-title">当前生产任务</p>

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
</div>
</template>


<script setup>

const machines=[]

for(let i=1;i<=30;i++){

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

.page{
padding:30px
}

.panel{
background:#101d34;
border-radius:18px;
padding:20px
}

.machines{
display:grid;
grid-template-columns:repeat(5,1fr);
gap:15px
}

.machine{
background:#172842;
border-radius:14px;
padding:15px
}

.head{
display:flex;
align-items:center;
gap:10px
}

.gear{
font-size:18px
}

.run{
color:#22c55e;
animation:spin 2s linear infinite
}

.task-box{
margin-top:12px;
padding:10px;
border-radius:10px;
background:#213554
}

.task-box.stock{
border-left:4px solid #38bdf8
}

.task-box.order{
border-left:4px solid #22c55e
}

.task-title{
font-weight:bold
}

@keyframes spin{
to{transform:rotate(360deg)}
}
</style>
