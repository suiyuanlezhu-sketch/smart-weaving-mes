
<template>
<div class="layout">
<aside>
<div class="logo">MES</div>
<h3>织布生产管理系统</h3>

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
<h1>织布生产智能驾驶舱 V10.1.3</h1>
<p class="sub">订单生产 · 常规备货 · 设备实时管理</p>

<div class="summary">
<div class="card" v-for="c in summary">
<h2>{{c.value}}</h2>
<p>{{c.name}}</p>
</div>
</div>

<div class="panel">
<h2>设备分类实时状态</h2>
<div class="groups">
<div class="group" v-for="g in groups">
<h3>{{g.name}}</h3>
<p>设备数量：{{g.total}}台</p>
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
<div v-if="m.status==='run'" class="gear-wrap">
<span class="gear">⚙</span>
<span class="dot"></span>
</div>
<div v-else class="static">⚙</div>
<b>{{m.id}}</b>
</div>

<p>设备：{{m.type}}</p>
<p>状态：{{m.statusText}}</p>
<p>规格：{{m.spec}}</p>

<div class="task">
<b>当前生产任务</b>
<p>{{m.task}}</p>
<p v-if="m.owner">业务员：{{m.owner}}</p>
</div>

<p>今日产量：{{m.output}}米</p>
<p>运行时间：8.5小时</p>
</div>
</div>
</div>
</main>
</div>
</template>

<script setup>
const summary=[
{name:"总织布机",value:"240台"},
{name:"生产中",value:"165台"},
{name:"订单生产",value:"105台"},
{name:"今日产量",value:"28560米"}
]

const groups=[
{name:"梭机",total:120,running:82,idle:23.3,output:14500},
{name:"大喷织机",total:80,running:56,idle:22.5,output:9800},
{name:"小喷织机",total:40,running:27,idle:25,output:4260}
]

const machines=[]
for(let i=1;i<=100;i++){
machines.push({
id:"SJ-"+String(i).padStart(3,'0'),
type:"梭机",
status:i%3?'run':'idle',
statusText:i%3?'生产中':'空闲',
spec:"40S/120",
task:i%2?'📦 常规备货':'📋 业务员订单',
owner:i%2?'':'张经理',
output:680
})
}
</script>

<style>
body{margin:0;background:#06111f;color:white;font-family:"Microsoft YaHei"}
.layout{display:flex;min-height:100vh}
aside{width:230px;background:#081525;padding:20px;border-right:1px solid #203554}
.logo{background:#2563eb;width:45px;height:45px;display:flex;align-items:center;justify-content:center;border-radius:10px}
.nav{padding:12px;background:#14243d;border-radius:10px;margin:8px 0}
.nav.active{border:1px solid #3b82f6}
main{flex:1;padding:25px}
.summary{display:grid;grid-template-columns:repeat(4,1fr);gap:15px}
.card,.panel{background:#101d34;border-radius:18px;padding:20px;margin-top:20px}
.groups{display:grid;grid-template-columns:repeat(3,1fr);gap:15px}
.group,.machine{background:#172842;border-radius:12px;padding:15px}
.green{color:#22c55e}
.machines{display:grid;grid-template-columns:repeat(10,1fr);gap:10px}
.machine{font-size:12px}
.head{display:flex;align-items:center;gap:8px}
.gear-wrap{width:30px;height:30px;position:relative;display:flex;align-items:center;justify-content:center}
.gear{font-size:22px;color:#22c55e;animation:rotate 1.5s linear infinite}
.dot{position:absolute;right:0;top:0;width:7px;height:7px;background:#22c55e;border-radius:50%;animation:pulse 1s infinite}
.static{font-size:22px;color:#94a3b8}
.task{background:#223755;padding:8px;border-radius:8px}
@keyframes rotate{to{transform:rotate(360deg)}}
@keyframes pulse{50%{transform:scale(1.6);opacity:.5}}
</style>
