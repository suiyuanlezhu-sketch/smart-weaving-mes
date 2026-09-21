<template>
<div class="layout">
<aside>
<h2>Smart MES</h2>
<div class="nav" v-for="n in nav">{{n}}</div>
</aside>

<main>
<h1>织布生产智能驾驶舱 V10.1.2</h1>

<div class="cards">
<div class="card">240台<br><small>总织布机</small></div>
<div class="card">165台<br><small>生产中</small></div>
<div class="card">28560米<br><small>今日产量</small></div>
</div>

<div class="panel">
<h2>设备分类实时状态</h2>
<div class="groups">
<div class="box" v-for="g in groups">
<h3>{{g.name}}</h3>
<p>生产中：{{g.running}}台</p>
<p>空闲率：{{g.idle}}%</p>
<p>日产量：{{g.output}}米</p>
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
<p>{{m.type}}</p>
<p>{{m.statusText}}</p>
<p>规格：{{m.spec}}</p>

<div class="task">
<strong>当前生产任务</strong>
<p v-if="m.task==='常规备货'">📦 常规备货</p>
<p v-else>📋 业务员订单</p>
<p>{{m.owner}}</p>
</div>

<p>今日产量：{{m.output}}米</p>
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

const groups=[
{name:"梭机",running:82,idle:23.3,output:14500},
{name:"大喷织机",running:56,idle:22.5,output:9800},
{name:"小喷织机",running:27,idle:25,output:4260}
]

const machines=[]
for(let i=1;i<=50;i++){
machines.push({
id:"SJ-"+String(i).padStart(3,"0"),
type:"梭机",
status:i%3?"run":"idle",
statusText:i%3?"生产中":"空闲",
spec:"40S/120",
task:i%2?"常规备货":"业务员订单",
owner:i%2?"":"张经理",
output:680
})
}
</script>

<style>
body{margin:0;background:#06111f;color:#fff;font-family:Microsoft YaHei}
.layout{display:flex;min-height:100vh}
aside{width:230px;background:#081525;padding:20px}
.nav{background:#14243d;padding:12px;margin:8px 0;border-radius:10px}
main{flex:1;padding:25px}
.cards{display:flex;gap:15px}
.card,.panel{background:#101d34;padding:20px;border-radius:18px;margin:20px 0}
.card{font-size:26px}
.groups{display:grid;grid-template-columns:repeat(3,1fr);gap:15px}
.box,.machine{background:#172842;padding:15px;border-radius:12px}
.machines{display:grid;grid-template-columns:repeat(10,1fr);gap:10px}
.machine{font-size:12px}
.gear{font-size:18px}
.run{color:#22c55e;animation:spin 1.8s linear infinite}
.idle{color:#94a3b8}
.task{background:#223755;padding:8px;border-radius:8px}
@keyframes spin{to{transform:rotate(360deg)}}
</style>
