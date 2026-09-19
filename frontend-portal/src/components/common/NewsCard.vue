<template>
  <article
    class="news-card"
    :class="`news-card--${variant}`"
    @click="handleClick"
  >
    <div class="news-card__image">
      <img :src="news.coverImage" :alt="news.title" />
    </div>

    <div class="news-card__content">
      <div class="news-card__meta">
        <span class="news-card__category">{{ news.category }}</span>
        <span class="news-card__date">{{ formatDate(news.publishTime) }}</span>
      </div>

      <h3 class="news-card__title">{{ news.title }}</h3>
      <p class="news-card__summary">{{ news.summary }}</p>

      <!-- 列表形态：作者与阅读量页脚 -->
      <div v-if="variant === 'list'" class="news-card__footer">
        <span class="news-card__author">{{ news.author }}</span>
        <span class="news-card__views">
          <el-icon><View /></el-icon> {{ news.viewCount }}
        </span>
      </div>
    </div>
  </article>
</template>

<script setup lang="ts">
import { useRouter } from 'vue-router'
import type { NewsItem } from '@/types'

const props = withDefaults(defineProps<{
  news: NewsItem
  /** list: 新闻列表页卡片；overview: 首页等概览位卡片 */
  variant?: 'list' | 'overview'
}>(), {
  variant: 'list'
})

const router = useRouter()

const handleClick = () => {
  router.push(`/news/${props.news.id}`)
}

const formatDate = (dateStr: string) => {
  return new Date(dateStr).toLocaleDateString('zh-CN', {
    year: 'numeric',
    month: 'short',
    day: 'numeric'
  })
}
</script>

<style lang="scss" scoped>
.news-card {
  background: $bg-color-white;
  border: 1px solid $border-color-light;
  border-radius: $border-radius-lg;
  overflow: hidden;
  cursor: pointer;
  transition: all $transition-normal;

  &:hover {
    border-color: transparent;
    box-shadow: $shadow-xl;

    .news-card__image img {
      transform: scale(1.05);
    }

    .news-card__title {
      color: $primary-color;
    }
  }

  &__image {
    height: 200px;
    overflow: hidden;

    img {
      width: 100%;
      height: 100%;
      object-fit: cover;
      transition: transform $transition-slow;
    }
  }

  &__content {
    padding: $spacing-lg;
  }

  &__meta {
    display: flex;
    align-items: center;
    margin-bottom: $spacing-sm;
  }

  &__category {
    font-size: $font-size-xs;
    font-weight: 600;
    color: $primary-color;
    padding: 2px $spacing-sm;
    background: rgba($primary-color, 0.1);
    border-radius: $border-radius-sm;
  }

  &__date {
    font-size: $font-size-xs;
    color: $text-color-secondary;
  }

  &__title {
    font-size: $font-size-lg;
    line-height: 1.4;
    margin-bottom: $spacing-sm;
    transition: color $transition-fast;
    display: -webkit-box;
    -webkit-line-clamp: 2;
    -webkit-box-orient: vertical;
    overflow: hidden;
  }

  &__summary {
    font-size: $font-size-sm;
    color: $text-color-secondary;
    line-height: $line-height-loose;
    display: -webkit-box;
    -webkit-line-clamp: 2;
    -webkit-box-orient: vertical;
    overflow: hidden;
  }
}

// ==================== 列表形态（新闻列表页） ====================
.news-card--list {
  .news-card__meta {
    justify-content: space-between;
  }

  .news-card__summary {
    margin-bottom: $spacing-md;
  }

  .news-card__footer {
    display: flex;
    align-items: center;
    justify-content: space-between;
    padding-top: $spacing-md;
    border-top: 1px solid $border-color-light;
    font-size: $font-size-sm;
    color: $text-color-secondary;
  }

  .news-card__views {
    display: flex;
    align-items: center;
    gap: 4px;
  }
}

// ==================== 概览形态（首页等） ====================
.news-card--overview {
  .news-card__meta {
    gap: $spacing-md;
  }
}
</style>
