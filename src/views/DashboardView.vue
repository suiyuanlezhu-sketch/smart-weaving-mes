<template>
  <section class="dashboard">
    <div class="summary-grid">
      <div class="summary-card blue">
        <div class="summary-label">总机位</div>
        <div class="summary-value">100台</div>
        <div class="summary-foot">当前已接入织布机位总数</div>
      </div>
      <div class="summary-card green">
        <div class="summary-label">生产中</div>
        <div class="summary-value">68台</div>
        <div class="summary-foot">动态转动图标实时展示</div>
      </div>
      <div class="summary-card gray">
        <div class="summary-label">空闲中</div>
        <div class="summary-value">22台</div>
        <div class="summary-foot">灰色状态图标表示待生产</div>
      </div>
      <div class="summary-card red">
        <div class="summary-label">维修中</div>
        <div class="summary-value">10台</div>
        <div class="summary-foot">红色动态告警状态</div>
      </div>
      <div class="summary-card cyan">
        <div class="summary-label">今日产量</div>
        <div class="summary-value">28,560米</div>
        <div class="summary-foot">较昨日 +13.5%</div>
      </div>
    </div>

    <div class="content-grid">
      <div class="panel chart-panel">
        <div class="panel-head">
          <div>
            <div class="panel-title">最近七日产量</div>
            <div class="panel-subtitle">按日统计实际生产米数</div>
          </div>
          <div class="panel-tag">近7日</div>
        </div>
        <OutputBarChart />
      </div>

      <div class="panel quick-panel">
        <div class="panel-head">
          <div>
            <div class="panel-title">生产概览</div>
            <div class="panel-subtitle">实时经营数据</div>
          </div>
        </div>

        <div class="quick-list">
          <div class="quick-item">
            <span class="quick-name">当日生产批次</span>
            <span class="quick-value">126单</span>
          </div>
          <div class="quick-item">
            <span class="quick-name">在产订单</span>
            <span class="quick-value">68单</span>
          </div>
          <div class="quick-item">
            <span class="quick-name">平均单机日产</span>
            <span class="quick-value">420米</span>
          </div>
          <div class="quick-item">
            <span class="quick-name">最高产量机位</span>
            <span class="quick-value">A16 · 680米</span>
          </div>
          <div class="quick-item">
            <span class="quick-name">当前维修工单</span>
            <span class="quick-value warn">10条</span>
          </div>
        </div>

        <div class="quick-highlight">
          <div class="highlight-title">系统提示</div>
          <div class="highlight-text">A03、B11、C07 等机位处于维修状态，建议优先处理，避免影响当日排产。</div>
        </div>
      </div>
    </div>

    <div class="panel machine-panel">
      <div class="panel-head machine-head">
        <div>
          <div class="panel-title">100台织布机位实时状态</div>
          <div class="panel-subtitle">动态状态展示 · 可扩展对接扫码/生产记录模块</div>
        </div>

        <div class="filters">
          <el-input v-model="keyword" placeholder="搜索机位，如 A01 / B12" clearable class="search-input">
            <template #prefix>
              <el-icon><Search /></el-icon>
            </template>
          </el-input>

          <div class="filter-chips">
            <div class="chip" :class="{active: currentFilter==='all'}" @click="currentFilter='all'">全部</div>
            <div class="chip" :class="{active: currentFilter==='running'}" @click="currentFilter='running'">生产中</div>
            <div class="chip" :class="{active: currentFilter==='idle'}" @click="currentFilter='idle'">空闲</div>
            <div class="chip" :class="{active: currentFilter==='repair'}" @click="currentFilter='repair'">维修</div>
          </div>
        </div>
      </div>

      <div class="machine-grid-list">
        <MachineCard
          v-for="machine in filteredMachines"
          :key="machine.id"
          :machine="machine"
        />
      </div>
    </div>
  </section>
</template>

<script setup>
import { computed, ref } from 'vue'
import OutputBarChart from '../components/OutputBarChart.vue'
import MachineCard from '../components/MachineCard.vue'

const keyword = ref('')
const currentFilter = ref('all')

const products = ['纯棉布','涤纶布','混纺布','牛津布','坯布','色织布']
const specs = ['40S/120','50S/140','32S/108','45S/136','60S/160','30S/100']
const workers = ['张三','李四','王五','赵六','陈强','周平','吴刚','刘涛']

const machines = []
for (let i = 1; i <= 100; i++) {
  const rowCode = i <= 25 ? 'A' : i <= 50 ? 'B' : i <= 75 ? 'C' : 'D'
  const num = String(((i - 1) % 25) + 1).padStart(2, '0')
  let status = 'running'
  if (i > 68 && i <= 90) status = 'idle'
  if (i > 90) status = 'repair'

  machines.push({
    id: `${rowCode}${num}`,
    status,
    product: status === 'repair' ? '-' : products[i % products.length],
    spec: status === 'repair' ? '-' : specs[i % specs.length],
    worker: status === 'repair' ? '-' : workers[i % workers.length],
    time: status === 'repair'
      ? `09:${String(i % 60).padStart(2,'0')}`
      : `${String(6 + (i % 12)).padStart(2,'0')}:${String((i * 7) % 60).padStart(2,'0')}`
  })
}

const filteredMachines = computed(() => {
  return machines.filter(m => {
    const statusMatch = currentFilter.value === 'all' ? true : m.status === currentFilter.value
    const keywordMatch = keyword.value
      ? m.id.toLowerCase().includes(keyword.value.toLowerCase())
      : true
    return statusMatch && keywordMatch
  })
})
</script>

<style scoped>
.dashboard{
  display:flex;
  flex-direction:column;
  gap:18px;
}
.summary-grid{
  display:grid;
  grid-template-columns:repeat(5,1fr);
  gap:16px;
}
.summary-card{
  border-radius:18px;
  padding:18px 20px;
  border:1px solid #1e2c4d;
  box-shadow:var(--card-shadow);
  position:relative;
  overflow:hidden;
}
.summary-card::after{
  content:'';
  position:absolute;
  right:-20px;
  top:-20px;
  width:110px;
  height:110px;
  border-radius:50%;
  background:rgba(255,255,255,.05);
}
.summary-card.blue{background:linear-gradient(135deg,#162443,#132544)}
.summary-card.green{background:linear-gradient(135deg,#0f3022,#112e25)}
.summary-card.gray{background:linear-gradient(135deg,#243047,#1d273c)}
.summary-card.red{background:linear-gradient(135deg,#3a1b26,#2e1820)}
.summary-card.cyan{background:linear-gradient(135deg,#163149,#11283f)}
.summary-label{
  color:#8eb2ff;
  font-size:13px;
  margin-bottom:10px;
}
.summary-value{
  color:#fff;
  font-size:34px;
  font-weight:800;
}
.summary-foot{
  color:#9ab0d8;
  font-size:12px;
  margin-top:10px;
}
.content-grid{
  display:grid;
  grid-template-columns:1.5fr .9fr;
  gap:18px;
}
.panel{
  background:linear-gradient(180deg,rgba(17,24,43,.98),rgba(13,19,34,.98));
  border:1px solid #1c2948;
  border-radius:20px;
  box-shadow:var(--card-shadow);
}
.chart-panel{
  padding:18px 18px 8px;
}
.quick-panel{
  padding:18px;
}
.panel-head{
  display:flex;
  align-items:flex-start;
  justify-content:space-between;
  margin-bottom:10px;
}
.panel-title{
  font-size:18px;
  font-weight:700;
  color:#fff;
}
.panel-subtitle{
  color:#8ea4d2;
  font-size:12px;
  margin-top:6px;
}
.panel-tag{
  padding:6px 10px;
  border-radius:999px;
  font-size:12px;
  color:#9fc6ff;
  background:rgba(59,130,246,.14);
  border:1px solid rgba(59,130,246,.2);
}
.quick-list{
  display:flex;
  flex-direction:column;
  gap:12px;
  margin-top:12px;
}
.quick-item{
  display:flex;
  justify-content:space-between;
  align-items:center;
  background:#0f172b;
  border:1px solid #1e2c4d;
  border-radius:14px;
  padding:14px;
}
.quick-name{
  color:#9cb2dc;
  font-size:13px;
}
.quick-value{
  color:#fff;
  font-weight:700;
}
.quick-value.warn{
  color:#f59e0b;
}
.quick-highlight{
  margin-top:16px;
  background:linear-gradient(180deg,rgba(37,99,235,.14),rgba(37,99,235,.05));
  border:1px solid rgba(59,130,246,.18);
  border-radius:16px;
  padding:16px;
}
.highlight-title{
  color:#d7e7ff;
  font-weight:700;
  margin-bottom:8px;
}
.highlight-text{
  color:#a5b8de;
  font-size:13px;
  line-height:1.75;
}
.machine-panel{
  padding:18px;
}
.machine-head{
  gap:18px;
}
.filters{
  display:flex;
  align-items:center;
  gap:12px;
  flex-wrap:wrap;
}
.search-input{
  width:260px;
}
:deep(.el-input__wrapper){
  background:#0f172b;
  box-shadow:none;
  border:1px solid #243353;
}
:deep(.el-input__inner){
  color:#e8eefc;
}
.filter-chips{
  display:flex;
  gap:10px;
  flex-wrap:wrap;
}
.chip{
  padding:8px 14px;
  border-radius:999px;
  background:#0f172b;
  border:1px solid #243353;
  color:#9cb2dc;
  cursor:pointer;
  transition:.2s ease;
  font-size:13px;
}
.chip:hover,.chip.active{
  color:#fff;
  background:rgba(59,130,246,.18);
  border-color:rgba(59,130,246,.4);
}
.machine-grid-list{
  margin-top:16px;
  display:grid;
  grid-template-columns:repeat(4,minmax(0,1fr));
  gap:14px;
}

@media (max-width: 1600px){
  .summary-grid{grid-template-columns:repeat(3,1fr)}
  .machine-grid-list{grid-template-columns:repeat(3,minmax(0,1fr))}
}
@media (max-width: 1180px){
  .summary-grid{grid-template-columns:repeat(2,1fr)}
  .content-grid{grid-template-columns:1fr}
  .machine-grid-list{grid-template-columns:repeat(2,minmax(0,1fr))}
}
@media (max-width: 760px){
  .summary-grid{grid-template-columns:1fr}
  .machine-grid-list{grid-template-columns:1fr}
  .machine-head{flex-direction:column;align-items:flex-start}
  .search-input{width:100%}
}
</style>
