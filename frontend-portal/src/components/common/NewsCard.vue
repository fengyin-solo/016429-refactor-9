<template>
  <!-- 两种形态点击均跳转新闻详情页 -->
  <div class="news-card" :class="`news-card--${variant}`" @click="handleClick">
    <div class="news-image">
      <img :src="news.coverImage" :alt="news.title" />
    </div>
    <div class="news-content">
      <div class="news-meta">
        <span class="news-category">{{ news.category }}</span>
        <span class="news-date">{{ formatDate(news.publishTime) }}</span>
      </div>
      <h3>{{ news.title }}</h3>
      <p>{{ news.summary }}</p>
      <div v-if="variant === 'list'" class="news-footer">
        <span class="news-author">{{ news.author }}</span>
        <span class="news-views">
          <el-icon><View /></el-icon> {{ news.viewCount }}
        </span>
      </div>
    </div>
  </div>
</template>

<script setup lang="ts">
import { useRouter } from 'vue-router'
import type { NewsItem } from '@/types'

const props = withDefaults(defineProps<{
  news: NewsItem
  /** list：新闻列表页卡片（含作者/浏览量）；home：首页资讯卡片 */
  variant?: 'list' | 'home'
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
  background: white;
  border: 1px solid $border-color-light;
  border-radius: $border-radius-lg;
  overflow: hidden;
  cursor: pointer;
  transition: all $transition-normal;

  &:hover {
    border-color: transparent;
    box-shadow: $shadow-xl;

    .news-image img {
      transform: scale(1.05);
    }

    h3 {
      color: $primary-color;
    }
  }

  .news-image {
    height: 200px;
    overflow: hidden;

    img {
      width: 100%;
      height: 100%;
      object-fit: cover;
      transition: transform $transition-slow;
    }
  }

  .news-content {
    padding: $spacing-lg;

    .news-meta {
      display: flex;
      align-items: center;
      margin-bottom: $spacing-sm;

      .news-category {
        font-size: $font-size-xs;
        font-weight: 600;
        color: $primary-color;
        padding: 2px $spacing-sm;
        background: rgba($primary-color, 0.1);
        border-radius: $border-radius-sm;
      }

      .news-date {
        font-size: $font-size-xs;
        color: $text-color-secondary;
      }
    }

    h3 {
      font-size: $font-size-lg;
      line-height: 1.4;
      margin-bottom: $spacing-sm;
      transition: color $transition-fast;
      display: -webkit-box;
      -webkit-line-clamp: 2;
      -webkit-box-orient: vertical;
      overflow: hidden;
    }

    p {
      font-size: $font-size-sm;
      color: $text-color-secondary;
      line-height: $line-height-loose;
      display: -webkit-box;
      -webkit-line-clamp: 2;
      -webkit-box-orient: vertical;
      overflow: hidden;
    }
  }

  // ==================== 新闻列表页 ====================
  &--list {
    .news-meta {
      justify-content: space-between;
    }

    p {
      margin-bottom: $spacing-md;
    }

    .news-footer {
      display: flex;
      align-items: center;
      justify-content: space-between;
      padding-top: $spacing-md;
      border-top: 1px solid $border-color-light;
      font-size: $font-size-sm;
      color: $text-color-secondary;

      .news-views {
        display: flex;
        align-items: center;
        gap: 4px;
      }
    }
  }

  // ==================== 首页 ====================
  &--home {
    .news-meta {
      gap: $spacing-md;
    }
  }
}
</style>
