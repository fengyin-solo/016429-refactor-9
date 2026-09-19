<template>
  <!-- 产品列表页：卡片整体不可点，按钮打开详情弹窗 -->
  <div v-if="variant === 'list'" class="product-card product-card--list">
    <div class="product-image">
      <img :src="product.image" :alt="product.name" />
      <div class="product-badge">{{ product.category }}</div>
    </div>
    <div class="product-content">
      <h3>{{ product.name }}</h3>
      <p>{{ product.description }}</p>
      <ul class="product-features">
        <li v-for="feature in product.features" :key="feature">
          <el-icon><Check /></el-icon>
          {{ feature }}
        </li>
      </ul>
      <el-button type="primary" round @click="emit('detail', product)">
        了解详情 <el-icon><Right /></el-icon>
      </el-button>
    </div>
  </div>

  <!-- 首页：整张卡片可点，跳转产品页 -->
  <div v-else class="product-card product-card--home" @click="emit('click', product)">
    <div class="product-image">
      <img :src="product.image" :alt="product.name" />
      <div class="product-overlay">
        <el-button type="primary" round>查看详情</el-button>
      </div>
    </div>
    <div class="product-content">
      <span class="product-category">{{ product.category }}</span>
      <h3>{{ product.name }}</h3>
      <p>{{ product.description }}</p>
      <div class="product-tags">
        <el-tag v-for="tag in product.features.slice(0, 3)" :key="tag" size="small" effect="plain">
          {{ tag }}
        </el-tag>
      </div>
    </div>
  </div>
</template>

<script setup lang="ts">
import type { ProductItem } from '@/types'

withDefaults(defineProps<{
  product: ProductItem
  /** list：产品列表页卡片；home：首页产品卡片 */
  variant?: 'list' | 'home'
}>(), {
  variant: 'list'
})

const emit = defineEmits<{
  (e: 'detail', product: ProductItem): void
  (e: 'click', product: ProductItem): void
}>()
</script>

<style lang="scss" scoped>
.product-card {
  background: white;
  overflow: hidden;
  transition: all $transition-normal;

  // ==================== 产品列表页 ====================
  &--list {
    display: flex;
    flex-direction: column;
    border: 1px solid $border-color-light;
    border-radius: $border-radius-xl;

    &:hover {
      border-color: transparent;
      box-shadow: $shadow-2xl;
      transform: translateY(-8px);

      .product-image img {
        transform: scale(1.05);
      }
    }

    .product-image {
      position: relative;
      height: 200px;
      overflow: hidden;
      flex-shrink: 0;

      img {
        width: 100%;
        height: 100%;
        object-fit: cover;
        transition: transform $transition-slow;
      }

      .product-badge {
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
    }

    .product-content {
      flex: 1;
      display: flex;
      flex-direction: column;
      padding: $spacing-xl;

      h3 {
        font-size: $font-size-xl;
        margin-bottom: $spacing-sm;
      }

      > p {
        font-size: $font-size-sm;
        color: $text-color-secondary;
        line-height: $line-height-loose;
        margin-bottom: $spacing-md;
        min-height: 42px;
      }

      .product-features {
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

      .el-button {
        width: 100%;
        margin-top: auto;
      }
    }
  }

  // ==================== 首页 ====================
  &--home {
    border-radius: $border-radius-lg;
    cursor: pointer;

    &:hover {
      box-shadow: $shadow-xl;
      transform: translateY(-4px);

      .product-overlay {
        opacity: 1;
      }

      .product-image img {
        transform: scale(1.05);
      }
    }

    .product-image {
      position: relative;
      height: 220px;
      overflow: hidden;

      img {
        width: 100%;
        height: 100%;
        object-fit: cover;
        transition: transform $transition-slow;
      }

      .product-overlay {
        position: absolute;
        inset: 0;
        background: rgba(0, 0, 0, 0.4);
        display: flex;
        align-items: center;
        justify-content: center;
        opacity: 0;
        transition: opacity $transition-normal;
      }
    }

    .product-content {
      padding: $spacing-lg;

      .product-category {
        font-size: $font-size-xs;
        font-weight: 600;
        color: $primary-color;
        text-transform: uppercase;
        letter-spacing: 0.05em;
      }

      h3 {
        font-size: $font-size-xl;
        margin: $spacing-sm 0;
      }

      p {
        font-size: $font-size-sm;
        color: $text-color-secondary;
        line-height: $line-height-loose;
        margin-bottom: $spacing-md;
      }

      .product-tags {
        display: flex;
        flex-wrap: wrap;
        gap: $spacing-xs;
      }
    }
  }
}
</style>
