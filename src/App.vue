<template>
  <div class="app-shell">
    <aside class="sidebar">
      <div class="brand">
        <div class="brand-logo">MES</div>
        <div>
          <div class="brand-title">织布生产管理</div>
          <div class="brand-subtitle">Smart Weaving MES V2.0</div>
        </div>
      </div>

      <div class="menu-section">主导航</div>
      <div class="menu-item active">
        <el-icon><DataBoard /></el-icon>
        <span>生产驾驶舱</span>
      </div>
      <div class="menu-item">
        <el-icon><Monitor /></el-icon>
        <span>织布机管理</span>
      </div>
      <div class="menu-item">
        <el-icon><Grid /></el-icon>
        <span>二维码管理</span>
      </div>
      <div class="menu-item">
        <el-icon><Goods /></el-icon>
        <span>产品与规格库</span>
      </div>
      <div class="menu-item">
        <el-icon><User /></el-icon>
        <span>工人管理</span>
      </div>
      <div class="menu-item">
        <el-icon><Tickets /></el-icon>
        <span>生产记录</span>
      </div>
      <div class="menu-item">
        <el-icon><Histogram /></el-icon>
        <span>产量统计</span>
      </div>
      <div class="menu-item">
        <el-icon><Download /></el-icon>
        <span>报表导出</span>
      </div>

      <div class="sidebar-footer">
        <div class="footer-chip">测试版</div>
        <div class="footer-note">适用于 100 台织布机位可视化演示</div>
      </div>
    </aside>

    <main class="main-area">
      <header class="topbar">
        <div>
          <div class="page-title">织布生产智能管理系统</div>
          <div class="page-subtitle">实时监控 · 产量统计 · 设备状态管理</div>
        </div>

        <div class="topbar-actions">
          <div class="status-dot"></div>
          <span class="status-text">系统运行正常</span>
          <el-button type="primary" round>今日报表导出</el-button>
        </div>
      </header>

      <DashboardView />
    </main>
  </div>
</template>

<script setup>
import DashboardView from './views/DashboardView.vue'
</script>

<style scoped>
.app-shell{
  display:flex;
  min-height:100vh;
}
.sidebar{
  width:260px;
  background:rgba(8,13,27,.95);
  border-right:1px solid #16213b;
  padding:22px 18px;
  display:flex;
  flex-direction:column;
}
.brand{
  display:flex;
  align-items:center;
  gap:14px;
  padding-bottom:22px;
  border-bottom:1px solid #1b2744;
  margin-bottom:18px;
}
.brand-logo{
  width:48px;
  height:48px;
  border-radius:14px;
  display:flex;
  align-items:center;
  justify-content:center;
  background:linear-gradient(135deg,#1d4ed8,#38bdf8);
  font-weight:800;
  letter-spacing:1px;
}
.brand-title{
  font-size:18px;
  font-weight:700;
  color:#fff;
}
.brand-subtitle{
  font-size:12px;
  color:#8aa2d5;
  margin-top:4px;
}
.menu-section{
  font-size:12px;
  color:#6f84b2;
  margin:10px 8px 10px;
}
.menu-item{
  display:flex;
  align-items:center;
  gap:12px;
  padding:12px 14px;
  border-radius:14px;
  color:#b9c7e8;
  margin-bottom:8px;
  transition:.25s ease;
  cursor:pointer;
}
.menu-item:hover{
  background:#121d34;
  color:#fff;
}
.menu-item.active{
  background:linear-gradient(90deg,rgba(59,130,246,.24),rgba(59,130,246,.08));
  color:#fff;
  border:1px solid rgba(59,130,246,.28);
}
.sidebar-footer{
  margin-top:auto;
  padding:16px;
  border-radius:16px;
  background:#10182d;
  border:1px solid #1c2948;
}
.footer-chip{
  display:inline-block;
  padding:4px 10px;
  border-radius:999px;
  background:rgba(34,197,94,.15);
  color:#7cf0a3;
  font-size:12px;
  margin-bottom:8px;
}
.footer-note{
  color:#8ea4d2;
  font-size:12px;
  line-height:1.6;
}
.main-area{
  flex:1;
  padding:22px 22px 28px;
  overflow:auto;
}
.topbar{
  display:flex;
  align-items:center;
  justify-content:space-between;
  margin-bottom:18px;
}
.page-title{
  font-size:28px;
  font-weight:800;
  color:#fff;
}
.page-subtitle{
  margin-top:6px;
  color:#8ea4d2;
  font-size:13px;
}
.topbar-actions{
  display:flex;
  align-items:center;
  gap:12px;
}
.status-dot{
  width:10px;
  height:10px;
  border-radius:50%;
  background:#22c55e;
  box-shadow:0 0 12px #22c55e;
}
.status-text{
  color:#c9d7f3;
  font-size:13px;
}

@media (max-width: 1180px){
  .sidebar{display:none}
  .main-area{padding:16px}
  .topbar{flex-direction:column;align-items:flex-start;gap:12px}
}
</style>
