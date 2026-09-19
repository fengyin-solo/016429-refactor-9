<template>
  <article
    class="product-card"
    :class="`product-card--${variant}`"
    @click="handleCardClick"
  >
    <div class="product-card__image">
      <img :src="product.image" :alt="product.name" />
      <!-- 列表形态：左上角分类角标 -->
      <div v-if="variant === 'list'" class="product-card__badge">
        {{ product.category }}
      </div>
      <!-- 概览形态：悬浮查看详情遮罩 -->
      <div v-else class="product-card__overlay">
        <el-button type="primary" round>查看详情</el-button>
      </div>
    </div>

    <div class="product-card__content">
      <!-- 概览形态：分类文字 -->
      <span v-if="variant === 'overview'" class="product-card__category">
        {{ product.category }}
      </span>

      <h3 class="product-card__name">{{ product.name }}</h3>
      <p class="product-card__desc">{{ product.description }}</p>

      <!-- 列表形态：核心特性勾选列表 -->
      <ul v-if="variant === 'list'" class="product-card__features">
        <li v-for="feature in product.features" :key="feature">
          <el-icon><Check /></el-icon>
          {{ feature }}
        </li>
      </ul>

      <!-- 概览形态：特性标签（最多展示 3 个） -->
      <div v-else class="product-card__tags">
        <el-tag
          v-for="tag in product.features.slice(0, 3)"
          :key="tag"
          size="small"
          effect="plain"
        >
          {{ tag }}
        </el-tag>
      </div>

      <el-button
        v-if="variant === 'list'"
        type="primary"
        round
        class="product-card__detail-btn"
        @click.stop="emit('detail', product)"
      >
        了解详情 <el-icon><Right /></el-icon>
      </el-button>
    </div>
  </article>
</template>

<script setup lang="ts">
import type { ProductItem } from '@/types'

const props = withDefaults(defineProps<{
  product: ProductItem
  /** list: 产品列表页卡片；overview: 首页等概览位卡片 */
  variant?: 'list' | 'overview'
}>(), {
  variant: 'list'
})

const emit = defineEmits<{
  (e: 'detail', product: ProductItem): void
  (e: 'click', product: ProductItem): void
}>()

// 概览形态整张卡片可点；列表形态的交互入口是“了解详情”按钮（打开详情弹窗）
const handleCardClick = () => {
  if (props.variant === 'overview') {
    emit('click', props.product)
  }
}
</script>

<style lang="scss" scoped>
.product-card {
  background: $bg-color-white;
  overflow: hidden;
  transition: all $transition-normal;

  &__image {
    position: relative;
    overflow: hidden;
    flex-shrink: 0;

    img {
      width: 100%;
      height: 100%;
      object-fit: cover;
      transition: transform $transition-slow;
    }
  }

  &__content {
    display: flex;
    flex-direction: column;
  }

  &__name {
    font-weight: 600;
    color: $text-color-primary;
  }

  &__desc {
    font-size: $font-size-sm;
    color: $text-color-secondary;
    line-height: $line-height-loose;
    display: -webkit-box;
    -webkit-line-clamp: 2;
    -webkit-box-orient: vertical;
    overflow: hidden;
  }
}

// ==================== 列表形态（产品列表页） ====================
.product-card--list {
  display: flex;
  flex-direction: column;
  border: 1px solid $border-color-light;
  border-radius: $border-radius-xl;

  &:hover {
    border-color: transparent;
    box-shadow: $shadow-2xl;
    transform: translateY(-8px);

    .product-card__image img {
      transform: scale(1.05);
    }
  }

  .product-card__image {
    height: 200px;
  }

  .product-card__badge {
    position: absolute;
    top: $spacing-md;
    left: $spacing-md;
    padding: $spacing-xs $spacing-md;
    background: rgba(0, 0, 0, 0.6);
    backdrop-filter: blur(10px);
    color: white;
    font-size: $font-size-xs;
    font-weight: 600;
    border-radius: $border-radius-full;
  }

  .product-card__content {
    flex: 1;
    padding: $spacing-xl;
  }

  .product-card__name {
    font-size: $font-size-xl;
    margin-bottom: $spacing-sm;
  }

  .product-card__desc {
    margin-bottom: $spacing-md;
    min-height: 42px;
  }

  .product-card__features {
    flex: 1;
    margin-bottom: $spacing-lg;

    li {
      display: flex;
      align-items: center;
      gap: $spacing-sm;
      padding: $spacing-xs 0;
      font-size: $font-size-sm;
      color: $text-color-regular;

      .el-icon {
        color: $success-color;
        font-size: 14px;
      }
    }
  }

  .product-card__detail-btn {
    width: 100%;
    margin-top: auto;
  }
}

// ==================== 概览形态（首页等） ====================
.product-card--overview {
  cursor: pointer;
  border-radius: $border-radius-lg;

  &:hover {
    box-shadow: $shadow-xl;
    transform: translateY(-4px);

    .product-card__overlay {
      opacity: 1;
    }

    .product-card__image img {
      transform: scale(1.05);
    }
  }

  .product-card__image {
    height: 220px;
  }

  .product-card__overlay {
    position: absolute;
    inset: 0;
    background: rgba(0, 0, 0, 0.4);
    display: flex;
    align-items: center;
    justify-content: center;
    opacity: 0;
    transition: opacity $transition-normal;
  }

  .product-card__content {
    padding: $spacing-lg;
  }

  .product-card__category {
    font-size: $font-size-xs;
    font-weight: 600;
    color: $primary-color;
    text-transform: uppercase;
    letter-spacing: 0.05em;
  }

  .product-card__name {
    font-size: $font-size-xl;
    margin: $spacing-sm 0;
  }

  .product-card__desc {
    margin-bottom: $spacing-md;
  }

  .product-card__tags {
    display: flex;
    flex-wrap: wrap;
    gap: $spacing-xs;
  }
}
</style>
