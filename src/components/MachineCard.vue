<template>
  <div class="machine-card" :class="machine.status">
    <div class="machine-head">
      <div class="machine-title">{{ machine.id }}机位</div>
      <div class="machine-status">
        <span class="status-icon" :class="machine.status">
          <el-icon v-if="machine.status === 'running'"><Setting /></el-icon>
          <span v-else-if="machine.status === 'idle'" class="idle-dot"></span>
          <el-icon v-else><Tools /></el-icon>
        </span>
        <span class="status-text">{{ statusMap[machine.status] }}</span>
      </div>
    </div>

    <div class="machine-grid">
      <div class="info-item">
        <span class="label">产品</span>
        <span class="value">{{ machine.product }}</span>
      </div>
      <div class="info-item">
        <span class="label">规格</span>
        <span class="value">{{ machine.spec }}</span>
      </div>
      <div class="info-item">
        <span class="label">工人</span>
        <span class="value">{{ machine.worker }}</span>
      </div>
      <div class="info-item">
        <span class="label">{{ machine.status === 'repair' ? '故障时间' : '开始时间' }}</span>
        <span class="value">{{ machine.time }}</span>
      </div>
    </div>
  </div>
</template>

<script setup>
defineProps({
  machine: {
    type: Object,
    required: true
  }
})

const statusMap = {
  running: '生产中',
  idle: '空闲',
  repair: '维修'
}
</script>

<style scoped>
.machine-card{
  background:linear-gradient(180deg,rgba(19,29,52,.98),rgba(15,23,42,.98));
  border:1px solid #1d2b4c;
  border-radius:16px;
  padding:16px;
  box-shadow:var(--card-shadow);
  min-height:170px;
  transition:.25s ease;
}
.machine-card:hover{
  transform:translateY(-3px);
  border-color:#335494;
}
.machine-card.running{
  box-shadow:0 12px 28px rgba(34,197,94,.08);
}
.machine-card.idle{
  box-shadow:0 12px 28px rgba(148,163,184,.08);
}
.machine-card.repair{
  box-shadow:0 12px 28px rgba(239,68,68,.10);
}
.machine-head{
  display:flex;
  justify-content:space-between;
  align-items:center;
  margin-bottom:14px;
}
.machine-title{
  font-size:18px;
  font-weight:700;
  color:#fff;
}
.machine-status{
  display:flex;
  align-items:center;
  gap:8px;
}
.status-icon{
  width:28px;
  height:28px;
  border-radius:50%;
  display:flex;
  justify-content:center;
  align-items:center;
  font-size:15px;
}
.status-icon.running{
  color:#22c55e;
  background:rgba(34,197,94,.15);
  animation: spin 2.5s linear infinite;
}
.status-icon.idle{
  background:rgba(148,163,184,.16);
}
.idle-dot{
  width:10px;
  height:10px;
  border-radius:50%;
  background:#94a3b8;
}
.status-icon.repair{
  color:#ef4444;
  background:rgba(239,68,68,.15);
  animation: pulse 1.6s ease-in-out infinite;
}
.status-text{
  color:#c9d7f3;
  font-size:13px;
}
.machine-grid{
  display:grid;
  grid-template-columns:1fr 1fr;
  gap:12px 16px;
}
.info-item{
  display:flex;
  flex-direction:column;
  gap:5px;
}
.label{
  color:#7e93bd;
  font-size:12px;
}
.value{
  color:#edf4ff;
  font-size:14px;
  font-weight:600;
  line-height:1.35;
}
@keyframes spin{
  from{transform:rotate(0)}
  to{transform:rotate(360deg)}
}
@keyframes pulse{
  0%,100%{transform:scale(1); box-shadow:0 0 0 0 rgba(239,68,68,.3)}
  50%{transform:scale(1.08); box-shadow:0 0 0 8px rgba(239,68,68,0)}
}
</style>
