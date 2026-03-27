<template>
  <a :href="tool.url" target="_blank" class="tool-card" :class="{ 'is-hot': tool.hot }" rel="noopener noreferrer">
    <div class="card-icon">{{ tool.icon }}</div>
    <div class="card-info">
      <div class="card-header">
        <h3 class="card-name">
          {{ tool.name }}
          <span v-if="tool.hot" class="hot-badge">🔥 热门</span>
        </h3>
        <span class="card-price" :class="getPriceClass(tool.price)">{{ tool.price || '免费' }}</span>
      </div>
      <p class="card-desc">{{ tool.description }}</p>
      <div class="card-meta">
        <span class="card-rating">⭐ {{ tool.rating || '-' }}</span>
        <span class="card-views">👀 {{ tool.views || '-' }}</span>
      </div>
      <div class="card-tags">
        <span v-for="tag in tool.tags" :key="tag" class="card-tag">{{ tag }}</span>
      </div>
    </div>
  </a>
</template>

<script setup>
defineProps({
  tool: {
    type: Object,
    required: true
  }
})

function getPriceClass(price) {
  if (!price) return 'free'
  if (price.includes('免费')) return 'free'
  if (price.includes('付费')) return 'paid'
  return 'free'
}
</script>

<style scoped>
.tool-card {
  display: flex;
  align-items: flex-start;
  gap: 12px;
  padding: 16px;
  background: #fff;
  border: 1px solid #eee;
  border-radius: 12px;
  text-decoration: none;
  color: inherit;
  transition: all 0.2s;
  cursor: pointer;
  position: relative;
}

.tool-card.is-hot {
  border-color: #f97316;
  background: linear-gradient(135deg, #fff 0%, #fff7ed 100%);
}

.tool-card:hover {
  border-color: #7c3aed;
  box-shadow: 0 4px 20px rgba(124, 58, 237, 0.15);
  transform: translateY(-2px);
}

.card-icon {
  font-size: 32px;
  flex-shrink: 0;
}

.card-info {
  flex: 1;
  min-width: 0;
}

.card-header {
  display: flex;
  align-items: center;
  justify-content: space-between;
  gap: 8px;
  margin-bottom: 4px;
}

.card-name {
  margin: 0;
  font-size: 16px;
  font-weight: 600;
  color: #1a1a2e;
  display: flex;
  align-items: center;
  gap: 6px;
}

.hot-badge {
  font-size: 11px;
  font-weight: 600;
  color: #fff;
  background: linear-gradient(135deg, #f97316, #ea580c);
  padding: 2px 8px;
  border-radius: 10px;
  animation: pulse 2s infinite;
}

@keyframes pulse {
  0%, 100% { opacity: 1; }
  50% { opacity: 0.8; }
}

.card-price {
  font-size: 11px;
  padding: 2px 8px;
  border-radius: 10px;
  font-weight: 500;
  flex-shrink: 0;
}

.card-price.free {
  background: #e6f7ed;
  color: #16a34a;
}

.card-price.paid {
  background: #fef3c7;
  color: #d97706;
}

.card-desc {
  margin: 0 0 8px;
  font-size: 13px;
  color: #666;
  line-height: 1.4;
  display: -webkit-box;
  -webkit-line-clamp: 2;
  -webkit-box-orient: vertical;
  overflow: hidden;
}

.card-meta {
  display: flex;
  align-items: center;
  gap: 12px;
  margin-bottom: 8px;
}

.card-rating,
.card-views {
  font-size: 12px;
  color: #888;
  font-weight: 500;
}

.card-tags {
  display: flex;
  flex-wrap: wrap;
  gap: 6px;
}

.card-tag {
  display: inline-block;
  padding: 2px 10px;
  background: #f3f0ff;
  color: #7c3aed;
  border-radius: 20px;
  font-size: 12px;
  font-weight: 500;
}

/* ========== 移动端适配 ========== */
@media screen and (max-width: 768px) {
  .tool-card {
    padding: 12px;
    gap: 10px;
  }

  .card-icon {
    font-size: 28px;
  }

  .card-name {
    font-size: 15px;
  }

  .card-desc {
    font-size: 12px;
    -webkit-line-clamp: 2;
  }

  .card-meta {
    gap: 10px;
    margin-bottom: 6px;
  }

  .card-rating,
  .card-views {
    font-size: 11px;
  }

  .card-tags {
    gap: 4px;
  }

  .card-tag {
    padding: 2px 8px;
    font-size: 11px;
  }
}
/* ========== 移动端适配结束 ========== */
</style>
