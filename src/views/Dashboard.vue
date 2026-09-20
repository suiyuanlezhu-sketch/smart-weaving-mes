<template>
<div class="page">

<h1>织布生产智能管理系统</h1>

<div class="cards">
<div class="card" v-for="item in stats" :key="item.name">
<h2>{{item.value}}</h2>
<p>{{item.name}}</p>
</div>
</div>

<h2>织布机实时状态</h2>

<div class="machines">
<div class="machine" v-for="m in machines" :key="m.id" :class="m.type">
<h3>{{m.id}}织布机</h3>
<p>{{m.status}}</p>
<p>产品：{{m.product}}</p>
<p>规格：{{m.spec}}</p>
<p>工人：{{m.worker}}</p>
</div>
</div>

</div>
</template>

<script setup>
const stats=[
{name:'总机器',value:'30台'},
{name:'生产中',value:'18台'},
{name:'空闲',value:'10台'},
{name:'今日产量',value:'28560米'}
]

const machines=[
{id:'A01',type:'run',status:'🟢生产中',product:'纯棉布',spec:'40S/120',worker:'张三'},
{id:'A02',type:'idle',status:'⚪空闲',product:'涤纶布',spec:'30S/100',worker:'李四'},
{id:'A03',type:'repair',status:'🔴维修',product:'-',spec:'-',worker:'-'}
]
</script>

<style scoped>
.page{
padding:30px;
}
.cards{
display:flex;
gap:20px;
margin-bottom:30px;
}
.card{
background:white;
padding:20px;
border-radius:12px;
width:160px;
box-shadow:0 2px 8px #ddd;
}
.machines{
display:flex;
gap:20px;
flex-wrap:wrap;
}
.machine{
background:white;
padding:20px;
width:220px;
border-radius:12px;
}
.run{
border-left:6px solid #22c55e;
}
.idle{
border-left:6px solid #999;
}
.repair{
border-left:6px solid #ef4444;
}
</style>
