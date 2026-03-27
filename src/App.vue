<template>
  <div class="container">
    <!-- 头部 -->
    <header class="header">
      <h1 class="title">AI工具导航</h1>
      <p class="subtitle">发现最实用的AI工具，提升效率快人一步</p>

      <!-- 搜索框 -->
      <div class="search-wrap">
        <input
          v-model="keyword"
          type="text"
          class="search-input"
          placeholder="搜索AI工具..."
          @input="filterTools"
        />
      </div>

      <!-- 分类标签 -->
      <div class="tabs">
        <button
          v-for="cat in categories"
          :key="cat"
          :class="['tab', { active: activeCat === cat }]"
          @click="switchTab(cat)"
        >
          {{ cat }}
        </button>
      </div>
    </header>

    <!-- 工具列表 -->
    <main class="main">
      <div v-if="filteredTools.length > 0" class="grid">
        <ToolCard v-for="tool in filteredTools" :key="tool.id" :tool="tool" />
      </div>
      <div v-else class="empty">
        <p>没有找到相关工具 😢</p>
        <button class="reset-btn" @click="reset">重置搜索</button>
      </div>
    </main>

    <!-- 底部 -->
    <footer class="footer">
      <p>🤖 AI工具导航</p>
    </footer>
  </div>
</template>

<script setup>
import { ref, computed, onMounted } from 'vue'
import ToolCard from './components/ToolCard.vue'
import toolsData from './data/tools.json'

const keyword = ref('')
const activeCat = ref('全部')
const categories = ref([])
const tools = ref([])

onMounted(() => {
  categories.value = toolsData.categories
  tools.value = toolsData.tools
})

const filteredTools = computed(() => {
  return tools.value.filter(tool => {
    const matchCat = activeCat.value === '全部' || tool.category === activeCat.value
    const matchKw = !keyword.value ||
      tool.name.includes(keyword.value) ||
      tool.description.includes(keyword.value)
    return matchCat && matchKw
  })
})

function switchTab(cat) {
  activeCat.value = cat
}

function filterTools() {
  // computed自动处理
}

function reset() {
  keyword.value = ''
  activeCat.value = '全部'
}
</script>

<style>
* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}

body {
  font-family: 'PingFang SC', 'Microsoft YaHei', sans-serif;
  background: #f8f7ff;
  color: #1a1a2e;
}

.container {
  max-width: 1100px;
  margin: 0 auto;
  padding: 0 16px;
}

/* ========== 移动端适配 ========== */
@media screen and (max-width: 768px) {
  .container {
    padding: 0 12px;
  }
  
  .header {
    padding: 32px 0 24px;
  }

  .title {
    font-size: 24px;
    letter-spacing: 1px;
  }

  .subtitle {
    font-size: 14px;
    margin-bottom: 20px;
  }

  .search-input {
    font-size: 15px;
    padding: 12px 18px;
  }

  .tabs {
    overflow-x: auto;
    justify-content: flex-start;
    padding-bottom: 8px;
    -webkit-overflow-scrolling: touch;
    scrollbar-width: none;
  }

  .tabs::-webkit-scrollbar {
    display: none;
  }

  .tab {
    padding: 6px 16px;
    font-size: 13px;
    flex-shrink: 0;
  }

  .main {
    padding: 16px 0 40px;
  }

  .grid {
    grid-template-columns: 1fr;
    gap: 12px;
  }
}

@media screen and (max-width: 480px) {
  .title {
    font-size: 20px;
  }

  .subtitle {
    font-size: 13px;
  }
}
/* ========== 移动端适配结束 ========== */

.header {
  text-align: center;
  padding: 60px 0 40px;
}

.title {
  font-size: 36px;
  font-weight: 700;
  color: #7c3aed;
  margin-bottom: 10px;
  letter-spacing: 2px;
}

.subtitle {
  font-size: 18px;
  color: #666;
  margin-bottom: 30px;
}

.search-wrap {
  margin-bottom: 24px;
}

.search-input {
  width: 100%;
  max-width: 500px;
  padding: 14px 20px;
  font-size: 16px;
  border: 2px solid #e5e7eb;
  border-radius: 50px;
  outline: none;
  transition: border-color 0.2s;
  background: #fff;
}

.search-input:focus {
  border-color: #7c3aed;
}

.tabs {
  display: flex;
  flex-wrap: wrap;
  gap: 10px;
  justify-content: center;
}

.tab {
  padding: 8px 20px;
  border: none;
  border-radius: 50px;
  background: #fff;
  color: #666;
  font-size: 14px;
  font-weight: 500;
  cursor: pointer;
  transition: all 0.2s;
  border: 1.5px solid #e5e7eb;
}

.tab:hover {
  border-color: #7c3aed;
  color: #7c3aed;
}

.tab.active {
  background: #7c3aed;
  color: #fff;
  border-color: #7c3aed;
}

.main {
  padding: 20px 0 60px;
}

.grid {
  display: grid;
  grid-template-columns: repeat(auto-fill, minmax(280px, 1fr));
  gap: 20px;
}

.empty {
  text-align: center;
  padding: 80px 0;
  color: #999;
}

.empty p {
  font-size: 18px;
  margin-bottom: 16px;
}

.reset-btn {
  padding: 10px 24px;
  background: #7c3aed;
  color: #fff;
  border: none;
  border-radius: 8px;
  cursor: pointer;
  font-size: 14px;
}

.reset-btn:hover {
  background: #6d28d9;
}

.footer {
  text-align: center;
  padding: 20px;
  color: #aaa;
  font-size: 14px;
  border-top: 1px solid #eee;
}
</style>
