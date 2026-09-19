<template>
  <div class="case-card" @click="emit('click', caseItem)">
    <div class="case-card__image">
      <img :src="caseItem.coverImage" :alt="caseItem.title" />
      <div class="case-card__overlay">
        <el-button type="primary" round size="small">
          查看详情 <el-icon><Right /></el-icon>
        </el-button>
      </div>
      <div class="case-card__industry">{{ caseItem.industry }}</div>
    </div>

    <div class="case-card__content">
      <div class="case-card__meta">
        <span class="case-card__client">
          <el-icon><OfficeBuilding /></el-icon>
          {{ caseItem.client }}
        </span>
        <span class="case-card__date">
          <el-icon><Calendar /></el-icon>
          {{ caseItem.publishTime }}
        </span>
      </div>

      <h3 class="case-card__title">{{ caseItem.title }}</h3>
      <p class="case-card__desc">{{ caseItem.description }}</p>

      <div class="case-card__tags">
        <span v-for="tag in caseItem.tags" :key="tag" class="case-card__tag">{{ tag }}</span>
      </div>

      <div class="case-card__results">
        <div
          v-for="result in caseItem.results.slice(0, 2)"
          :key="result.label"
          class="case-card__result"
        >
          <span class="case-card__result-value">{{ result.value }}</span>
          <span class="case-card__result-label">{{ result.label }}</span>
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
  background: $bg-color-white;
  border-radius: $border-radius-xl;
  overflow: hidden;
  cursor: pointer;
  transition: all $transition-normal;
  border: 1px solid $border-color-light;

  &:hover {
    border-color: transparent;
    box-shadow: $shadow-2xl;
    transform: translateY(-8px);

    .case-card__overlay {
      opacity: 1;
    }

    .case-card__image img {
      transform: scale(1.05);
    }
  }

  &__image {
    position: relative;
    height: 220px;
    overflow: hidden;

    img {
      width: 100%;
      height: 100%;
      object-fit: cover;
      transition: transform $transition-slow;
    }
  }

  &__overlay {
    position: absolute;
    inset: 0;
    background: rgba(0, 0, 0, 0.5);
    display: flex;
    align-items: center;
    justify-content: center;
    opacity: 0;
    transition: opacity $transition-normal;
  }

  &__industry {
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

  &__content {
    padding: $spacing-xl;
  }

  &__meta {
    display: flex;
    justify-content: space-between;
    margin-bottom: $spacing-md;

    .case-card__client,
    .case-card__date {
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

  &__title {
    font-size: $font-size-xl;
    font-weight: 600;
    margin-bottom: $spacing-sm;
    line-height: 1.4;
  }

  &__desc {
    font-size: $font-size-sm;
    color: $text-color-secondary;
    line-height: $line-height-loose;
    margin-bottom: $spacing-md;
    display: -webkit-box;
    -webkit-line-clamp: 2;
    -webkit-box-orient: vertical;
    overflow: hidden;
  }

  &__tags {
    display: flex;
    flex-wrap: wrap;
    gap: $spacing-xs;
    margin-bottom: $spacing-lg;

    .case-card__tag {
      padding: 4px 10px;
      background: $bg-color-light;
      color: $text-color-regular;
      font-size: $font-size-xs;
      border-radius: $border-radius-full;
    }
  }

  &__results {
    display: grid;
    grid-template-columns: repeat(2, 1fr);
    gap: $spacing-md;
    padding-top: $spacing-md;
    border-top: 1px dashed $border-color-light;

    .case-card__result {
      text-align: center;
    }

    .case-card__result-value {
      display: block;
      font-size: $font-size-xl;
      font-weight: 700;
      background: $gradient-text;
      -webkit-background-clip: text;
      -webkit-text-fill-color: transparent;
    }

    .case-card__result-label {
      font-size: $font-size-xs;
      color: $text-color-secondary;
    }
  }
}
</style>
