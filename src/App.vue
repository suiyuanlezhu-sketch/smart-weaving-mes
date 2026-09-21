<template>
  <div class="layout">
    <aside class="sidebar">
      <div class="brand">
        <div class="brand-logo">MES</div>
        <div>
          <div class="brand-title">织布生产管理系统</div>
          <div class="brand-subtitle">Smart Weaving MES V9.3.1</div>
        </div>
      </div>

      <div class="menu-section">主导航</div>
      <div class="menu active">生产驾驶舱</div>
      <div class="menu">织布机管理</div>
      <div class="menu">二维码管理</div>
      <div class="menu">产品与规格库</div>
      <div class="menu">工人管理</div>
      <div class="menu">生产记录</div>
      <div class="menu">产量统计</div>
      <div class="menu">报表导出</div>
    </aside>

    <main class="main">
      <section class="hero">
        <h1>生产智能驾驶舱</h1>
        <p>设备实时产量 · 生产效率 · 产能分析</p>
      </section>

      <section class="summary">
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
        <div class="card">
          <h2>165台</h2>
          <p>当前生产中</p>
        </div>
      </section>

      <section class="panel">
        <div class="panel-header">
          <div>
            <h2>设备类型产能分析</h2>
            <p>梭机 / 大喷织机 / 小喷织机 实时概览</p>
          </div>
        </div>

        <div class="groups">
          <div v-for="g in groups" :key="g.name" class="group">
            <h3>{{ g.name }}</h3>
            <p>设备数量：{{ g.total }}台</p>
            <p>生产中：{{ g.running }}台</p>
            <p>今日产量：{{ g.output }}米</p>
            <p>平均效率：{{ g.efficiency }}%</p>
            <div class="progress">
              <span :style="{ width: g.efficiency + '%' }"></span>
            </div>
          </div>
        </div>
      </section>

      <section class="panel">
        <div class="panel-header">
          <div>
            <h2>单机实时生产数据</h2>
            <p>展示每台机器的类型、规格、产量、运行时间与效率</p>
          </div>
        </div>

        <div class="machines">
          <div class="machine" v-for="m in machines" :key="m.id">
            <div class="machine-head">
              <div class="gear" :class="m.status">⚙</div>
              <b>{{ m.id }}</b>
            </div>
            <p>{{ m.type }}</p>
            <p>{{ m.statusText }}</p>
            <p>规格：{{ m.spec }}</p>
            <p>日产量：{{ m.output }}米</p>
            <p>运行：{{ m.hours }}小时</p>
            <p>效率：{{ m.efficiency }}%</p>
          </div>
        </div>
      </section>
    </main>
  </div>
</template>

<script setup>
const groups = [
  { name: '梭机', total: 120, running: 82, output: 14500, efficiency: 92 },
  { name: '大喷织机', total: 80, running: 56, output: 9800, efficiency: 94 },
  { name: '小喷织机', total: 40, running: 27, output: 4260, efficiency: 89 }
]

const machines = []

for (let i = 1; i <= 240; i++) {
  const type = i <= 120 ? '梭机' : i <= 200 ? '大喷织机' : '小喷织机'
  const status = i % 10 < 7 ? 'run' : i % 10 < 9 ? 'idle' : 'repair'
  machines.push({
    id: type === '梭机' ? 'SJ-' + i : type === '大喷织机' ? 'DP-' + (i - 120) : 'XP-' + (i - 200),
    type,
    status,
    statusText: status === 'run' ? '生产中' : status === 'idle' ? '空闲' : '维修',
    spec: type === '梭机' ? '40S/120' : type === '大喷织机' ? '50S/140' : '32S/108',
    output: status === 'run' ? 680 : 0,
    hours: status === 'run' ? 8.5 : 0,
    efficiency: status === 'run' ? 92 : 0
  })
}
</script>

<style>
body{
  margin:0;
  background:#06111f;
  color:white;
  font-family:"Microsoft YaHei", Arial, sans-serif;
}
.layout{
  display:flex;
  min-height:100vh;
}
.sidebar{
  width:240px;
  background:#081525;
  padding:20px 16px;
  border-right:1px solid #132845;
}
.brand{
  display:flex;
  gap:12px;
  align-items:center;
  margin-bottom:20px;
  padding-bottom:16px;
  border-bottom:1px solid #173051;
}
.brand-logo{
  width:42px;
  height:42px;
  border-radius:12px;
  display:flex;
  align-items:center;
  justify-content:center;
  background:linear-gradient(135deg,#1d4ed8,#38bdf8);
  font-weight:700;
}
.brand-title{
  font-size:18px;
  font-weight:700;
}
.brand-subtitle{
  font-size:12px;
  color:#8da4d0;
  margin-top:4px;
}
.menu-section{
  font-size:12px;
  color:#7d93bd;
  margin-bottom:10px;
}
.menu{
  padding:13px 14px;
  background:#112038;
  margin:8px 0;
  border-radius:12px;
  color:#d9e6ff;
  border:1px solid transparent;
}
.menu.active{
  background:#132847;
  border-color:#294d86;
  box-shadow:inset 0 0 0 1px rgba(86,145,255,.15);
}
.main{
  flex:1;
  padding:28px;
}
.hero h1{
  margin:0;
  font-size:38px;
  font-weight:800;
}
.hero p{
  color:#9bb0d6;
  margin-top:10px;
}
.summary{
  display:grid;
  grid-template-columns:repeat(4,1fr);
  gap:16px;
  margin-top:22px;
}
.card,.panel{
  background:#101d34;
  border:1px solid #1c3154;
  border-radius:18px;
  padding:20px;
}
.card h2{
  margin:0 0 10px;
  font-size:30px;
}
.card p{
  margin:0;
  color:#dbe7ff;
}
.panel{
  margin-top:20px;
}
.panel-header h2{
  margin:0;
  font-size:24px;
}
.panel-header p{
  margin:8px 0 0;
  color:#8fa6d2;
}
.groups{
  display:grid;
  grid-template-columns:repeat(3,1fr);
  gap:16px;
  margin-top:18px;
}
.group{
  background:#172842;
  padding:18px;
  border-radius:15px;
  border:1px solid #243c64;
}
.group h3{
  margin-top:0;
  font-size:20px;
}
.progress{
  height:10px;
  background:#263a58;
  border-radius:999px;
  overflow:hidden;
  margin-top:14px;
}
.progress span{
  display:block;
  height:100%;
  background:#22c55e;
}
.machines{
  display:grid;
  grid-template-columns:repeat(10,1fr);
  gap:10px;
  margin-top:18px;
}
.machine{
  background:#172842;
  padding:10px;
  border-radius:10px;
  font-size:12px;
  border:1px solid #20365b;
}
.machine-head{
  display:flex;
  align-items:center;
  gap:8px;
  margin-bottom:6px;
}
.gear{
  width:18px;
  height:18px;
  line-height:18px;
  font-size:16px;
  display:inline-flex;
  align-items:center;
  justify-content:center;
  transform-origin:center center;
}
.run{
  color:#22c55e;
  animation:smallSpin 2.2s linear infinite;
}
.idle{
  color:#94a3b8;
}
.repair{
  color:#ef4444;
  animation:softBlink 1.2s ease-in-out infinite;
}
@keyframes smallSpin{
  from{ transform:rotate(0deg); }
  to{ transform:rotate(360deg); }
}
@keyframes softBlink{
  50%{ opacity:.45; }
}
@media (max-width:1600px){
  .machines{
    grid-template-columns:repeat(6,1fr);
  }
  .summary{
    grid-template-columns:repeat(2,1fr);
  }
}
@media (max-width:1200px){
  .layout{
    display:block;
  }
  .sidebar{
    width:auto;
  }
  .groups{
    grid-template-columns:1fr;
  }
  .machines{
    grid-template-columns:repeat(4,1fr);
  }
}
@media (max-width:768px){
  .machines{
    grid-template-columns:repeat(2,1fr);
  }
  .summary{
    grid-template-columns:1fr;
  }
}
</style>
