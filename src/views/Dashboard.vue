<template>
  <div class="dashboard">
    <!-- 顶部标题 -->
    <div class="header">
      <div class="header-content">
        <h2>数据看板</h2>
        <p>为平台管理员提供可视化数据看板，多维度展示系统使用数据与业务统计信息。</p>
      </div>
      <el-icon class="header-icon" size="50" color="#409EFF">
        <FolderOpened />
      </el-icon>
    </div>

    <!-- 统计卡片 -->
    <el-row :gutter="20" class="card-row">
      <el-col :xs="12" :sm="6" v-for="item in stats" :key="item.id">
        <el-card class="stat-card" shadow="hover">
          <div class="stat-box">
            <div class="icon-box" :style="{ background: item.color }">
              <el-icon color="#fff"><component :is="item.icon" /></el-icon>
            </div>
            <div>
              <p class="label">{{ item.title }}</p>
              <div class="num">
                {{ formatNumber(item.value) }}
                <span v-if="item.up > 0" class="up">+{{ item.up }}</span>
              </div>
            </div>
          </div>
        </el-card>
      </el-col>
    </el-row>

    <!-- 双列表 -->
    <el-row :gutter="20" class="card-row">
      <el-col :xs="24" :md="12">
        <el-card class="list-card" shadow="hover">
          <h3>热门应用TOP5</h3>
          <div class="list-container">
            <div
              v-for="(item, index) in apps"
              :key="item.name"
              class="list-item"
              :class="{ 'last-item': index === apps.length - 1 }"
            >
              <div class="left">
                <div class="small-icon" :style="{ background: item.color }">
                  <el-icon size="14" color="#fff"><component :is="item.icon" /></el-icon>
                </div>
                <span>{{ item.name }}</span>
              </div>
              <span class="count-text">{{ item.count }}次</span>
            </div>
          </div>
        </el-card>
      </el-col>

      <el-col :xs="24" :md="12">
        <el-card class="list-card" shadow="hover">
          <h3>热门模板TOP5</h3>
          <div class="list-container">
            <div
              v-for="(item, index) in temps"
              :key="item.name"
              class="list-item"
              :class="{ 'last-item': index === temps.length - 1 }"
            >
              <div class="left">
                <div class="small-icon" :style="{ background: item.color }">
                  <Document />
                </div>
              </div>
              <span>{{ item.name }}</span>
              <span class="count-text">{{ item.count }}次</span>
            </div>
          </div>
        </el-card>
      </el-col>
    </el-row>

    <!-- 底部功能卡片 -->
    <el-row :gutter="20" class="card-row">
      <el-col :xs="12" :sm="6" v-for="item in func" :key="item.title">
        <el-card class="func-card" shadow="hover">
          <h3>{{ item.title }}</h3>
          <el-icon size="50" color="#409EFF" class="func-icon">
            <component :is="item.icon" />
          </el-icon>
        </el-card>
      </el-col>
    </el-row>
  </div>
</template>

<script setup lang="ts">
import { ref } from 'vue'
import { markRaw } from 'vue'
import {
  FolderOpened,
  Document,
  Edit,
  Notebook,
  Reading,
  User,
  Setting
} from '@element-plus/icons-vue'

// ============= TS 类型定义 =============
interface StatItem {
  id: number
  title: string
  value: number
  up: number
  icon: any
  color: string
}
interface AppItem {
  name: string
  count: number
  icon: any
  color: string
}
interface TempItem {
  name: string
  count: number
  color: string
}
interface FuncItem {
  title: string
  icon: any
}

// ============= 工具函数：数字千分位格式化 =============
const formatNumber = (num: number): string => {
  return num.toLocaleString()
}

// ============= 【脱敏通用数据】 =============
const stats = ref<StatItem[]>([
  { id: 1, title: '平台总模板数', value: 528, up: 32, icon: markRaw(Document), color: '#409EFF' },
  { id: 2, title: '累计生成文档', value: 86542, up: 156, icon: markRaw(Document), color: '#27AE60' },
  { id: 3, title: '注册用户总数', value: 1268, up: 45, icon: markRaw(User), color: '#F56C6C' },
  { id: 4, title: '系统功能模块', value: 18, up: 2, icon: markRaw(Setting), color: '#67C23A' },
])

const apps = ref<AppItem[]>([
  { name: '在线文档编辑', count: 12658, icon: markRaw(Document), color: '#409EFF' },
  { name: '表单编辑制作', count: 9872, icon: markRaw(Edit), color: '#67C23A' },
  { name: '工作周报管理', count: 3568, icon: markRaw(Notebook), color: '#E6A23C' },
  { name: '数据报表导出', count: 2145, icon: markRaw(Document), color: '#F56C6C' },
  { name: '学习笔记记录', count: 1209, icon: markRaw(Reading), color: '#909399' },
])

const temps = ref<TempItem[]>([
  { name: '业务数据报表模板', count: 28560, color: '#409EFF' },
  { name: '月度工作总结模板', count: 9632, color: '#FF9800' },
  { name: '项目会议纪要模板', count: 5214, color: '#4CAF50' },
  { name: '标准合同模板', count: 1856, color: '#409EFF' },
  { name: '工作计划安排模板', count: 987, color: '#4CAF50' },
])

const func = ref<FuncItem[]>([
  { title: '周报管理', icon: markRaw(Notebook) },
  { title: '文档编辑', icon: markRaw(Document) },
  { title: '报表导出', icon: markRaw(Document) },
  { title: '笔记记录', icon: markRaw(Reading) },
])
</script>

<style scoped>
/* 全局容器 */
.dashboard {
  padding: 24px;
  max-width: 1920px;
  margin: 0 auto;
}

/* 头部样式 */
.header {
  background: linear-gradient(to right, #e6f2ff, #f0f7ff);
  padding: 24px 32px;
  border-radius: 12px;
  margin-bottom: 24px;
  display: flex;
  justify-content: space-between;
  align-items: center;
  box-shadow: 0 2px 12px rgba(0, 0, 0, 0.06);
}
.header-content h2 {
  margin: 0 0 8px 0;
  font-size: 22px;
  font-weight: 600;
  color: #1f2937;
}
.header-content p {
  margin: 0;
  color: #6b7280;
  line-height: 1.6;
}
.header-icon {
  flex-shrink: 0;
}

/* 卡片行间距 */
.card-row {
  margin-bottom: 24px;
}

/* 统计卡片 */
.stat-card {
  border-radius: 12px;
  border: none;
  transition: all 0.3s ease;
  height: 100%;
}
.stat-box {
  display: flex;
  align-items: center;
  gap: 16px;
  padding: 8px 0;
}
.icon-box {
  width: 48px;
  height: 48px;
  border-radius: 10px;
  display: flex;
  align-items: center;
  justify-content: center;
  flex-shrink: 0;
}
.label {
  margin: 0;
  color: #6b7280;
  font-size: 14px;
}
.num {
  font-size: 28px;
  font-weight: 700;
  color: #1f2937;
  display: flex;
  align-items: baseline;
  gap: 8px;
  margin-top: 4px;
}
.up {
  font-size: 13px;
  color: #00b42a;
  font-weight: 500;
}

/* 列表卡片 */
.list-card {
  border-radius: 12px;
  border: none;
  height: 100%;
}
.list-card h3 {
  margin: 0 0 20px 0;
  font-size: 17px;
  font-weight: 600;
  color: #1f2937;
}
.list-item {
  display: flex;
  justify-content: space-between;
  align-items: center;
  padding: 12px 0;
  border-bottom: 1px solid #f3f4f6;
  transition: background 0.2s ease;
}
.list-item:hover {
  background: #f9fafb;
  padding-left: 4px;
}
.last-item {
  border-bottom: none;
}
.left {
  display: flex;
  align-items: center;
  gap: 12px;
}
.small-icon {
  width: 32px;
  height: 32px;
  border-radius: 8px;
  display: flex;
  align-items: center;
  justify-content: center;
  flex-shrink: 0;
}
.count-text {
  color: #4b5563;
  font-weight: 500;
}

/* 功能卡片 */
.func-card {
  border-radius: 12px;
  border: none;
  background: linear-gradient(135deg, #e6f2ff, #f0f7ff);
  height: 200px;
  position: relative;
  transition: transform 0.3s ease;
}
.func-card:hover {
  transform: translateY(-4px);
}
.func-card h3 {
  margin: 20px 0 0 20px;
  font-size: 20px;
  color: #409EFF;
  font-weight: 600;
}
.func-icon {
  position: absolute;
  bottom: 24px;
  right: 24px;
  opacity: 0.9;
}

/* 响应式适配 */
@media (max-width: 768px) {
  .dashboard {
    padding: 16px;
  }
  .header {
    padding: 20px;
  }
  .header-icon {
    display: none;
  }
  .num {
    font-size: 24px;
  }
}
</style>