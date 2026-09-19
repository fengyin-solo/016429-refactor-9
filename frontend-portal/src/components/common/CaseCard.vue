<template>
  <div class="case-card" @click="emit('click', caseItem)">
    <div class="case-image">
      <img :src="caseItem.coverImage" :alt="caseItem.title" />
      <div class="case-overlay">
        <el-button type="primary" round size="small">
          查看详情 <el-icon><Right /></el-icon>
        </el-button>
      </div>
      <div class="case-industry">{{ caseItem.industry }}</div>
    </div>
    <div class="case-content">
      <div class="case-meta">
        <span class="case-client">
          <el-icon><OfficeBuilding /></el-icon>
          {{ caseItem.client }}
        </span>
        <span class="case-date">
          <el-icon><Calendar /></el-icon>
          {{ caseItem.publishTime }}
        </span>
      </div>
      <h3 class="case-title">{{ caseItem.title }}</h3>
      <p class="case-desc">{{ caseItem.description }}</p>
      <div class="case-tags">
        <span v-for="tag in caseItem.tags" :key="tag" class="tag">{{ tag }}</span>
      </div>
      <div class="case-results">
        <div v-for="result in caseItem.results.slice(0, 2)" :key="result.label" class="result-item">
          <span class="result-value">{{ result.value }}</span>
          <span class="result-label">{{ result.label }}</span>
        </div>
      </div>
    </div>
  </div>
</template>

<script setup lang="ts">
import type { CaseItem } from '@/types'

defineProps<{
  caseItem: CaseItem
}>()

const emit = defineEmits<{
  (e: 'click', caseItem: CaseItem): void
}>()
</script>

<style lang="scss" scoped>
.case-card {
  background: white;
  border-radius: $border-radius-xl;
  overflow: hidden;
  cursor: pointer;
  transition: all $transition-normal;
  border: 1px solid $border-color-light;

  &:hover {
    border-color: transparent;
    box-shadow: $shadow-2xl;
    transform: translateY(-8px);

    .case-image {
      .case-overlay {
        opacity: 1;
      }

      img {
        transform: scale(1.05);
      }
    }
  }

  .case-image {
    position: relative;
    height: 220px;
    overflow: hidden;

    img {
      width: 100%;
      height: 100%;
      object-fit: cover;
      transition: transform $transition-slow;
    }

    .case-overlay {
      position: absolute;
      inset: 0;
      background: rgba(0, 0, 0, 0.5);
      display: flex;
      align-items: center;
      justify-content: center;
      opacity: 0;
      transition: opacity $transition-normal;
    }

    .case-industry {
      position: absolute;
      top: $spacing-md;
      left: $spacing-md;
      padding: $spacing-xs $spacing-md;
      background: $gradient-primary;
      color: white;
      font-size: $font-size-xs;
      font-weight: 600;
      border-radius: $border-radius-full;
    }
  }

  .case-content {
    padding: $spacing-xl;

    .case-meta {
      display: flex;
      justify-content: space-between;
      margin-bottom: $spacing-md;

      .case-client,
      .case-date {
        display: flex;
        align-items: center;
        gap: $spacing-xs;
        font-size: $font-size-xs;
        color: $text-color-secondary;

        .el-icon {
          font-size: 12px;
        }
      }
    }

    .case-title {
      font-size: $font-size-xl;
      font-weight: 600;
      margin-bottom: $spacing-sm;
      line-height: 1.4;
    }

    .case-desc {
      font-size: $font-size-sm;
      color: $text-color-secondary;
      line-height: $line-height-loose;
      margin-bottom: $spacing-md;
      display: -webkit-box;
      -webkit-line-clamp: 2;
      -webkit-box-orient: vertical;
      overflow: hidden;
    }

    .case-tags {
      display: flex;
      flex-wrap: wrap;
      gap: $spacing-xs;
      margin-bottom: $spacing-lg;

      .tag {
        padding: 4px 10px;
        background: $bg-color-light;
        color: $text-color-regular;
        font-size: $font-size-xs;
        border-radius: $border-radius-full;
      }
    }

    .case-results {
      display: grid;
      grid-template-columns: repeat(2, 1fr);
      gap: $spacing-md;
      padding-top: $spacing-md;
      border-top: 1px dashed $border-color-light;

      .result-item {
        text-align: center;

        .result-value {
          display: block;
          font-size: $font-size-xl;
          font-weight: 700;
          background: $gradient-text;
          -webkit-background-clip: text;
          -webkit-text-fill-color: transparent;
        }

        .result-label {
          font-size: $font-size-xs;
          color: $text-color-secondary;
        }
      }
    }
  }
}
</style>
