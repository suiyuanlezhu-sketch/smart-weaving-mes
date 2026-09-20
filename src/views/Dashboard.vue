<template>
<div>
<div class="cards">
<div v-for="i in stats" class="card">
<h2>{{i.value}}</h2><p>{{i.name}}</p>
</div>
</div>
<h2>织布机实时状态</h2>
<div class="machines">
<div v-for="m in machines" class="machine" :class="m.status">
<h3>{{m.id}}</h3>
<p>{{m.statusText}}</p>
<p>{{m.product}}</p>
<p>{{m.spec}}</p>
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
{id:'A01',status:'run',statusText:'🟢生产中',product:'纯棉布',spec:'40S/120',worker:'张三'},
{id:'A02',status:'idle',statusText:'⚪空闲',product:'上次：涤纶布',spec:'30S/100',worker:'-'},
{id:'A03',status:'repair',statusText:'🔴维修',product:'设备维护',spec:'-',worker:'-'}
]
</script>

<style scoped>
.cards{display:flex;gap:20px}
.card{background:white;padding:20px;border-radius:12px;min-width:150px}
.machines{display:flex;gap:20px;flex-wrap:wrap}
.machine{padding:20px;border-radius:12px;width:220px;background:white}
.run{border-left:6px solid #22c55e}
.idle{border-left:6px solid #999}
.repair{border-left:6px solid #ef4444}
</style>