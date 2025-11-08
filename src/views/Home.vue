<template>
  <div class="home">
    <HeroCarousel />

    <!-- 便宜好物快速购买区 -->
    <section class="quick-buy-section">
      <div class="section-title">
        <h2>💰 {{ t('home.quickBuy.title') }}</h2>
        <p>{{ t('home.quickBuy.subtitle') }}</p>
      </div>
      <div class="quick-buy-grid">
        <div 
          v-for="item in quickBuyItems" 
          :key="item.id"
          class="quick-buy-card"
          @click="addToCart(item)"
        >
          <div class="item-image">
            <img :src="item.image" :alt="item.name" />
            <span class="discount-badge">-{{ item.discount }}%</span>
          </div>
          <div class="item-info">
            <h3>{{ item.name }}</h3>
            <div class="price-row">
              <span class="current-price">${{ item.price }}</span>
              <span class="original-price">${{ item.originalPrice }}</span>
            </div>
            <button class="quick-add-btn">
              <svg width="16" height="16" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2">
                <path d="M9 2v6h6V2M19 9v6h-6V9M15 15v6H9v-6"></path>
              </svg>
              {{ t('product.quickAdd') }}
            </button>
          </div>
        </div>
      </div>
    </section>

    <!-- 原创搭配设计 -->
    <section class="outfit-section">
      <div class="section-header">
        <h2>{{ t('home.outfitDesign.title') }}</h2>
        <p>{{ t('home.outfitDesign.subtitle') }}</p>
      </div>
      <div class="outfit-grid">
        <div 
          v-for="(outfit, index) in designedOutfits" 
          :key="outfit.id"
          class="outfit-card"
          :style="{ animationDelay: `${index * 0.1}s` }"
          @click="handleOutfitClick(outfit)"
        >
          <div class="outfit-images">
            <div class="main-image" :style="{ background: outfit.bgColor }">
              <img :src="outfit.mainImage" :alt="outfit.name" />
              <span class="designer-badge">
                <svg width="12" height="12" viewBox="0 0 24 24" fill="currentColor">
                  <path d="M12 2L15.09 8.26L22 9.27L17 14.14L18.18 21.02L12 17.77L5.82 21.02L7 14.14L2 9.27L8.91 8.26L12 2Z"/>
                </svg>
                {{ outfit.designer }}
              </span>
            </div>
            <div class="thumbnail-grid">
              <img 
                v-for="(thumb, idx) in outfit.thumbnails" 
                :key="idx"
                :src="thumb" 
                :alt="`Item ${idx + 1}`"
                class="thumbnail-item"
              />
            </div>
          </div>
          <div class="outfit-info">
            <div class="outfit-header">
              <h3>{{ outfit.name }}</h3>
              <span class="outfit-tag">{{ outfit.pieces }}件套装</span>
            </div>
            <p class="outfit-desc">{{ outfit.description }}</p>
            <div class="outfit-footer">
              <div class="price-info">
                <span class="outfit-price">${{ outfit.price }}</span>
                <span class="original-price" v-if="outfit.originalPrice">${{ outfit.originalPrice }}</span>
                <span class="save-badge" v-if="outfit.originalPrice">省${{ outfit.originalPrice - outfit.price }}</span>
              </div>
              <button class="view-outfit-btn">
                {{ t('home.outfitDesign.viewDetails') }}
                <svg width="16" height="16" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2">
                  <path d="M5 12h14M12 5l7 7-7 7"/>
                </svg>
              </button>
            </div>
          </div>
        </div>
      </div>
    </section>

    <section class="featured-products">
      <div class="section-header">
        <h2>{{ t('home.trending.title') }}</h2>
        <p>{{ t('home.trending.subtitle') }}</p>
      </div>
      <div class="products-grid">
        <div 
          v-for="product in trendingProducts" 
          :key="product.id"
          class="product-card"
          @click="handleProductClick(product)"
        >
          <div class="product-image">
            <img :src="product.image" :alt="product.name" />
            <span v-if="product.isNew" class="new-badge">{{ t('product.new') }}</span>
            <button 
              class="wishlist-btn" 
              :class="{ active: cartStore.isInWishlist(product.id) }"
              @click.stop="toggleWishlist(product)"
            >
              <svg width="20" height="20" viewBox="0 0 24 24" :fill="cartStore.isInWishlist(product.id) ? 'currentColor' : 'none'" stroke="currentColor" stroke-width="2">
                <path d="M20.84 4.61a5.5 5.5 0 0 0-7.78 0L12 5.67l-1.06-1.06a5.5 5.5 0 0 0-7.78 7.78l1.06 1.06L12 21.23l7.78-7.78 1.06-1.06a5.5 5.5 0 0 0 0-7.78z"></path>
              </svg>
            </button>
          </div>
          <div class="product-info">
            <h3>{{ product.name }}</h3>
            <div class="rating">
              <span class="stars">★★★★★</span>
              <span class="review-count">({{ product.reviews }} {{ t('product.reviews') }})</span>
            </div>
            <p class="product-price">${{ product.price }}</p>
          </div>
        </div>
      </div>
    </section>

    <section class="newsletter">
      <div class="newsletter-content">
        <h2>{{ t('home.newsletter.title') }}</h2>
        <p>{{ t('home.newsletter.subtitle') }}</p>
        <form @submit.prevent="handleSubscribe" class="newsletter-form">
          <input 
            type="email" 
            v-model="email"
            :placeholder="t('home.newsletter.placeholder')" 
            required
          />
          <button type="submit">{{ t('home.newsletter.subscribe') }}</button>
        </form>
        <p class="privacy-note">{{ t('home.newsletter.privacy') }}</p>
      </div>
    </section>

    <!-- Toast Notification -->
    <Transition name="toast">
      <div v-if="showToast" class="toast">
        {{ toastMessage }}
      </div>
    </Transition>
  </div>
</template>

<script setup>
import { ref, computed } from 'vue'
import { useRouter } from 'vue-router'
import { useI18n } from 'vue-i18n'
import { useCartStore } from '../stores/cart'
import { useProductsStore } from '../stores/products'
import HeroCarousel from '../components/HeroCarousel.vue'

const router = useRouter()
const { t } = useI18n()
const cartStore = useCartStore()
const productsStore = useProductsStore()

const email = ref('')
const showToast = ref(false)
const toastMessage = ref('')

const quickBuyItems = ref([
  { 
    id: 'qb1', 
    name: '纯棉袜子 5双装', 
    price: 9.99, 
    originalPrice: 19.99, 
    discount: 50,
    image: 'https://images.unsplash.com/photo-1586350977771-b3b0abd50c82?w=300&h=300&fit=crop',
    category: 'accessories'
  },
  { 
    id: 'qb2', 
    name: '发圈发带套装', 
    price: 6.99, 
    originalPrice: 14.99, 
    discount: 53,
    image: 'https://images.unsplash.com/photo-1535632066927-ab7c9ab60908?w=300&h=300&fit=crop',
    category: 'accessories'
  },
  { 
    id: 'qb3', 
    name: '便携化妆棉 200片', 
    price: 5.99, 
    originalPrice: 11.99, 
    discount: 50,
    image: 'https://images.unsplash.com/photo-1596462502278-27bfdc403348?w=300&h=300&fit=crop',
    category: 'accessories'
  },
  { 
    id: 'qb4', 
    name: '隐形船袜 10双', 
    price: 12.99, 
    originalPrice: 24.99, 
    discount: 48,
    image: 'https://images.unsplash.com/photo-1556306535-0f09a537f0a3?w=300&h=300&fit=crop',
    category: 'accessories'
  },
  { 
    id: 'qb5', 
    name: '一次性口罩 50只', 
    price: 8.99, 
    originalPrice: 15.99, 
    discount: 44,
    image: 'https://images.unsplash.com/photo-1584634731339-252c581abfc5?w=300&h=300&fit=crop',
    category: 'accessories'
  },
  { 
    id: 'qb6', 
    name: '简约发夹 12件套', 
    price: 7.99, 
    originalPrice: 16.99, 
    discount: 53,
    image: 'https://images.unsplash.com/photo-1610770001333-0b27c3d37a8e?w=300&h=300&fit=crop',
    category: 'accessories'
  }
])

const designedOutfits = ref([
  {
    id: 'outfit1',
    name: '甜美约会风',
    designer: '小美设计师',
    pieces: 3,
    description: '粉色针织开衫 + 白色连衣裙 + 小白鞋',
    price: 159.99,
    originalPrice: 219.99,
    bgColor: 'linear-gradient(135deg, #ffeef8 0%, #fff5f9 100%)',
    mainImage: 'https://images.unsplash.com/photo-1515886657613-9f3515b0c78f?w=400&h=500&fit=crop',
    thumbnails: [
      'https://images.unsplash.com/photo-1434389677669-e08b4cac3105?w=100&h=100&fit=crop',
      'https://images.unsplash.com/photo-1595777457583-95e059d581b8?w=100&h=100&fit=crop',
      'https://images.unsplash.com/photo-1525966222134-fcfa99b8ae77?w=100&h=100&fit=crop'
    ]
  },
  {
    id: 'outfit2',
    name: '职场精英装',
    designer: 'Emma设计师',
    pieces: 4,
    description: '黑色西装外套 + 白衬衫 + 高腰裤 + 高跟鞋',
    price: 289.99,
    originalPrice: 389.99,
    bgColor: 'linear-gradient(135deg, #f5f5f5 0%, #e8e8e8 100%)',
    mainImage: 'https://images.unsplash.com/photo-1487412720507-e7ab37603c6f?w=400&h=500&fit=crop',
    thumbnails: [
      'https://images.unsplash.com/photo-1591369822096-ffd140ec948f?w=100&h=100&fit=crop',
      'https://images.unsplash.com/photo-1596783074918-c84cb06531ca?w=100&h=100&fit=crop',
      'https://images.unsplash.com/photo-1605518216938-7c31b7b14ad0?w=100&h=100&fit=crop'
    ]
  },
  {
    id: 'outfit3',
    name: '街头潮流范',
    designer: 'Coco设计师',
    pieces: 4,
    description: '牛仔外套 + 印花T恤 + 破洞牛仔裤 + 运动鞋',
    price: 199.99,
    originalPrice: 279.99,
    bgColor: 'linear-gradient(135deg, #e3f2fd 0%, #bbdefb 100%)',
    mainImage: 'https://images.unsplash.com/photo-1483985988355-763728e1935b?w=400&h=500&fit=crop',
    thumbnails: [
      'https://images.unsplash.com/photo-1551028719-00167b16eac5?w=100&h=100&fit=crop',
      'https://images.unsplash.com/photo-1583743814966-8936f5b7be1a?w=100&h=100&fit=crop',
      'https://images.unsplash.com/photo-1542291026-7eec264c27ff?w=100&h=100&fit=crop'
    ]
  },
  {
    id: 'outfit4',
    name: '度假休闲风',
    designer: 'Lisa设计师',
    pieces: 3,
    description: '碎花连衣裙 + 编织包 + 平底凉鞋',
    price: 139.99,
    originalPrice: 199.99,
    bgColor: 'linear-gradient(135deg, #fff9e6 0%, #ffecb3 100%)',
    mainImage: 'https://images.unsplash.com/photo-1496747611176-843222e1e57c?w=400&h=500&fit=crop',
    thumbnails: [
      'https://images.unsplash.com/photo-1572804013309-59a88b7e92f1?w=100&h=100&fit=crop',
      'https://images.unsplash.com/photo-1590874103328-eac38a683ce7?w=100&h=100&fit=crop',
      'https://images.unsplash.com/photo-1603487742131-4160ec999306?w=100&h=100&fit=crop'
    ]
  },
  {
    id: 'outfit5',
    name: '优雅晚宴装',
    designer: 'Sophie设计师',
    pieces: 4,
    description: '丝绸衬衫 + 半身长裙 + 高跟鞋 + 珍珠项链',
    price: 349.99,
    originalPrice: 499.99,
    bgColor: 'linear-gradient(135deg, #f3e5f5 0%, #e1bee7 100%)',
    mainImage: 'https://images.unsplash.com/photo-1490481651871-ab68de25d43d?w=400&h=500&fit=crop',
    thumbnails: [
      'https://images.unsplash.com/photo-1594633312681-425c7b97ccd1?w=100&h=100&fit=crop',
      'https://images.unsplash.com/photo-1583496661160-fb5886a0aaaa?w=100&h=100&fit=crop',
      'https://images.unsplash.com/photo-1535632066927-ab7c9ab60908?w=100&h=100&fit=crop'
    ]
  },
  {
    id: 'outfit6',
    name: '运动活力装',
    designer: 'Anna设计师',
    pieces: 3,
    description: '运动内衣 + 瑜伽裤 + 运动鞋',
    price: 119.99,
    originalPrice: 169.99,
    bgColor: 'linear-gradient(135deg, #e8f5e9 0%, #c8e6c9 100%)',
    mainImage: 'https://images.unsplash.com/photo-1518002171953-a080ee817e1f?w=400&h=500&fit=crop',
    thumbnails: [
      'https://images.unsplash.com/photo-1598971639058-fab3c3109a00?w=100&h=100&fit=crop',
      'https://images.unsplash.com/photo-1506629082955-511b1aa562c8?w=100&h=100&fit=crop',
      'https://images.unsplash.com/photo-1460353581641-37baddab0fa2?w=100&h=100&fit=crop'
    ]
  }
])

const trendingProducts = computed(() => {
  return productsStore.products.slice(0, 4)
})

const toggleWishlist = (product) => {
  const added = cartStore.toggleWishlist(product)
  showToastMessage(added ? t('product.addedToWishlist') : t('product.removedFromWishlist'))
}

const addToCart = (product) => {
  cartStore.addToCart(product)
  showToastMessage(t('product.addedToCart'))
}

const handleProductClick = (product) => {
  console.log('Product clicked:', product)
}

const handleOutfitClick = (outfit) => {
  console.log('Outfit clicked:', outfit)
  showToastMessage(`查看 ${outfit.name} 详情`)
}

const handleSubscribe = () => {
  if (email.value) {
    showToastMessage(t('home.newsletter.success'))
    email.value = ''
  }
}

const showToastMessage = (message) => {
  toastMessage.value = message
  showToast.value = true
  setTimeout(() => {
    showToast.value = false
  }, 2000)
}
</script>

<style scoped>
.home {
  padding: 20px 0;
}

/* 快速购买区域 */
.quick-buy-section {
  margin-bottom: 80px;
  padding: 40px;
  background: linear-gradient(135deg, #fff5f7 0%, #fffaf5 100%);
  border-radius: 24px;
  box-shadow: 0 5px 20px rgba(0, 0, 0, 0.05);
}

.section-title {
  text-align: center;
  margin-bottom: 40px;
}

.section-title h2 {
  font-size: 36px;
  font-weight: 800;
  margin-bottom: 10px;
  color: #333;
}

.section-title p {
  font-size: 16px;
  color: #666;
}

.quick-buy-grid {
  display: grid;
  grid-template-columns: repeat(auto-fit, minmax(160px, 1fr));
  gap: 20px;
}

.quick-buy-card {
  background: #fff;
  border-radius: 16px;
  overflow: hidden;
  cursor: pointer;
  transition: all 0.3s cubic-bezier(0.4, 0, 0.2, 1);
  border: 2px solid transparent;
  box-shadow: 0 2px 12px rgba(0, 0, 0, 0.06);
}

.quick-buy-card:hover {
  transform: translateY(-8px);
  box-shadow: 0 12px 28px rgba(0, 0, 0, 0.15);
  border-color: #ff69b4;
}

.item-image {
  position: relative;
  width: 100%;
  aspect-ratio: 1;
  overflow: hidden;
  background: #f5f5f5;
}

.item-image img {
  width: 100%;
  height: 100%;
  object-fit: cover;
  transition: transform 0.4s;
}

.quick-buy-card:hover .item-image img {
  transform: scale(1.1);
}

.discount-badge {
  position: absolute;
  top: 10px;
  right: 10px;
  background: linear-gradient(135deg, #ff6b6b 0%, #ee5a6f 100%);
  color: #fff;
  padding: 4px 10px;
  border-radius: 12px;
  font-size: 12px;
  font-weight: 700;
  box-shadow: 0 2px 8px rgba(255, 107, 107, 0.4);
}

.item-info {
  padding: 16px;
}

.item-info h3 {
  font-size: 14px;
  font-weight: 600;
  margin-bottom: 10px;
  color: #333;
  line-height: 1.4;
  min-height: 40px;
}

.price-row {
  display: flex;
  align-items: center;
  gap: 8px;
  margin-bottom: 12px;
}

.current-price {
  font-size: 20px;
  font-weight: 800;
  color: #ff69b4;
}

.original-price {
  font-size: 14px;
  color: #999;
  text-decoration: line-through;
}

.quick-add-btn {
  width: 100%;
  padding: 10px;
  background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
  color: #fff;
  border: none;
  border-radius: 8px;
  font-size: 13px;
  font-weight: 600;
  display: flex;
  align-items: center;
  justify-content: center;
  gap: 6px;
  transition: all 0.3s;
  cursor: pointer;
}

.quick-add-btn:hover {
  background: linear-gradient(135deg, #5568d3 0%, #6a3f8f 100%);
  transform: scale(1.02);
}

.quick-add-btn svg {
  width: 16px;
  height: 16px;
}

/* 原创搭配设计区域 */
.outfit-section {
  margin-top: 80px;
}

.section-header {
  text-align: center;
  margin-bottom: 50px;
}

.section-header h2 {
  font-size: 48px;
  font-weight: 800;
  margin-bottom: 12px;
  letter-spacing: -1px;
}

.section-header p {
  font-size: 18px;
  color: #666;
}

.outfit-grid {
  display: grid;
  grid-template-columns: repeat(auto-fit, minmax(320px, 1fr));
  gap: 30px;
  margin-bottom: 40px;
}

.outfit-card {
  cursor: pointer;
  border-radius: 20px;
  overflow: hidden;
  transition: all 0.4s cubic-bezier(0.4, 0, 0.2, 1);
  background: #fff;
  box-shadow: 0 4px 20px rgba(0, 0, 0, 0.08);
  animation: fadeIn 0.6s ease-out both;
}

@keyframes fadeIn {
  from {
    opacity: 0;
    transform: translateY(20px);
  }
  to {
    opacity: 1;
    transform: translateY(0);
  }
}

.outfit-card:hover {
  transform: translateY(-10px);
  box-shadow: 0 20px 40px rgba(0, 0, 0, 0.15);
}

.outfit-images {
  position: relative;
}

.main-image {
  width: 100%;
  height: 320px;
  display: flex;
  align-items: center;
  justify-content: center;
  overflow: hidden;
  position: relative;
}

.main-image img {
  width: 100%;
  height: 100%;
  object-fit: cover;
  transition: transform 0.4s;
}

.outfit-card:hover .main-image img {
  transform: scale(1.08);
}

.designer-badge {
  position: absolute;
  top: 15px;
  left: 15px;
  background: rgba(255, 255, 255, 0.95);
  backdrop-filter: blur(10px);
  padding: 6px 14px;
  border-radius: 20px;
  font-size: 12px;
  font-weight: 600;
  color: #333;
  display: flex;
  align-items: center;
  gap: 5px;
  box-shadow: 0 2px 10px rgba(0, 0, 0, 0.1);
}

.designer-badge svg {
  color: #ffd700;
}

.thumbnail-grid {
  display: flex;
  gap: 8px;
  padding: 12px;
  background: rgba(255, 255, 255, 0.95);
  position: absolute;
  bottom: 0;
  left: 0;
  right: 0;
}

.thumbnail-item {
  width: 60px;
  height: 60px;
  border-radius: 10px;
  object-fit: cover;
  border: 2px solid #fff;
  box-shadow: 0 2px 8px rgba(0, 0, 0, 0.1);
  transition: all 0.3s;
}

.thumbnail-item:hover {
  transform: scale(1.1);
  border-color: #ff69b4;
}

.outfit-info {
  padding: 20px;
}

.outfit-header {
  display: flex;
  align-items: center;
  justify-content: space-between;
  margin-bottom: 12px;
}

.outfit-header h3 {
  font-size: 20px;
  font-weight: 700;
  color: #333;
}

.outfit-tag {
  background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
  color: #fff;
  padding: 4px 12px;
  border-radius: 12px;
  font-size: 11px;
  font-weight: 600;
  white-space: nowrap;
}

.outfit-desc {
  font-size: 14px;
  color: #666;
  line-height: 1.6;
  margin-bottom: 16px;
  min-height: 44px;
}

.outfit-footer {
  display: flex;
  align-items: center;
  justify-content: space-between;
  gap: 12px;
  padding-top: 16px;
  border-top: 1px solid #f0f0f0;
}

.price-info {
  display: flex;
  align-items: center;
  gap: 8px;
  flex-wrap: wrap;
}

.outfit-price {
  font-size: 24px;
  font-weight: 800;
  color: #ff69b4;
}

.outfit-info .original-price {
  font-size: 14px;
  color: #999;
  text-decoration: line-through;
}

.save-badge {
  background: linear-gradient(135deg, #ff6b6b 0%, #ee5a6f 100%);
  color: #fff;
  padding: 3px 8px;
  border-radius: 8px;
  font-size: 11px;
  font-weight: 700;
}

.view-outfit-btn {
  background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
  color: #fff;
  padding: 10px 18px;
  border-radius: 20px;
  font-size: 13px;
  font-weight: 600;
  display: flex;
  align-items: center;
  gap: 6px;
  transition: all 0.3s;
  white-space: nowrap;
  border: none;
  cursor: pointer;
}

.view-outfit-btn:hover {
  background: linear-gradient(135deg, #5568d3 0%, #6a3f8f 100%);
  transform: scale(1.05);
}

.view-outfit-btn svg {
  transition: transform 0.3s;
}

.view-outfit-btn:hover svg {
  transform: translateX(3px);
}

.featured-products {
  margin-top: 80px;
}

.products-grid {
  display: grid;
  grid-template-columns: repeat(auto-fill, minmax(250px, 1fr));
  gap: 30px;
}

.product-card {
  cursor: pointer;
  transition: transform 0.3s;
}

.product-card:hover {
  transform: translateY(-8px);
}

.product-image {
  position: relative;
  width: 100%;
  aspect-ratio: 3/4;
  background: #f5f5f5;
  border-radius: 16px;
  overflow: hidden;
  margin-bottom: 15px;
}

.product-image img {
  width: 100%;
  height: 100%;
  object-fit: cover;
  transition: transform 0.4s;
}

.product-card:hover .product-image img {
  transform: scale(1.1);
}

.new-badge {
  position: absolute;
  top: 15px;
  left: 15px;
  background: #ff69b4;
  color: #fff;
  padding: 5px 12px;
  border-radius: 15px;
  font-size: 12px;
  font-weight: 700;
  letter-spacing: 0.5px;
}

.wishlist-btn {
  position: absolute;
  top: 15px;
  right: 15px;
  width: 40px;
  height: 40px;
  background: rgba(255, 255, 255, 0.95);
  border-radius: 50%;
  display: flex;
  align-items: center;
  justify-content: center;
  transition: all 0.3s;
  box-shadow: 0 2px 10px rgba(0, 0, 0, 0.1);
  opacity: 0;
  backdrop-filter: blur(10px);
}

.product-card:hover .wishlist-btn {
  opacity: 1;
}

.wishlist-btn:hover,
.wishlist-btn.active {
  background: #ff69b4;
  color: #fff;
  transform: scale(1.15);
}

.product-info h3 {
  font-size: 16px;
  font-weight: 600;
  margin-bottom: 8px;
  color: #333;
}

.rating {
  display: flex;
  align-items: center;
  gap: 5px;
  margin-bottom: 8px;
}

.stars {
  color: #ffc107;
  font-size: 14px;
  letter-spacing: 1px;
}

.review-count {
  font-size: 12px;
  color: #999;
}

.product-price {
  font-size: 18px;
  font-weight: 700;
  color: #000;
}

.newsletter {
  margin-top: 100px;
  padding: 80px 40px;
  background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
  border-radius: 24px;
  position: relative;
  overflow: hidden;
}

.newsletter::before {
  content: '';
  position: absolute;
  top: -50%;
  right: -50%;
  width: 100%;
  height: 200%;
  background: radial-gradient(circle, rgba(255, 255, 255, 0.1) 0%, transparent 70%);
  animation: rotate 20s linear infinite;
}

@keyframes rotate {
  from { transform: rotate(0deg); }
  to { transform: rotate(360deg); }
}

.newsletter-content {
  text-align: center;
  position: relative;
  z-index: 1;
}

.newsletter h2 {
  font-size: 48px;
  font-weight: 800;
  color: #fff;
  margin-bottom: 15px;
}

.newsletter p {
  font-size: 18px;
  color: rgba(255, 255, 255, 0.9);
  margin-bottom: 40px;
  max-width: 600px;
  margin-left: auto;
  margin-right: auto;
}

.newsletter-form {
  display: flex;
  gap: 15px;
  max-width: 600px;
  margin: 0 auto 15px;
}

.newsletter-form input {
  flex: 1;
  padding: 16px 24px;
  border-radius: 50px;
  border: none;
  font-size: 16px;
  background: rgba(255, 255, 255, 0.95);
}

.newsletter-form input::placeholder {
  color: #999;
}

.newsletter-form button {
  padding: 16px 40px;
  background: #000;
  color: #fff;
  border-radius: 50px;
  font-weight: 600;
  font-size: 16px;
  transition: all 0.3s;
  white-space: nowrap;
}

.newsletter-form button:hover {
  background: #333;
  transform: scale(1.05);
}

.privacy-note {
  font-size: 14px;
  color: rgba(255, 255, 255, 0.7);
  margin-top: 15px;
}

.toast {
  position: fixed;
  bottom: 30px;
  right: 30px;
  background: #4caf50;
  color: #fff;
  padding: 16px 24px;
  border-radius: 50px;
  font-weight: 600;
  box-shadow: 0 10px 30px rgba(0, 0, 0, 0.3);
  z-index: 1000;
}

.toast-enter-active, .toast-leave-active {
  transition: all 0.3s ease;
}

.toast-enter-from, .toast-leave-to {
  opacity: 0;
  transform: translateY(20px);
}

@media (max-width: 768px) {
  .quick-buy-section {
    padding: 30px 20px;
    margin-bottom: 50px;
  }

  .section-title h2 {
    font-size: 28px;
  }

  .section-title p {
    font-size: 14px;
  }

  .quick-buy-grid {
    grid-template-columns: repeat(auto-fit, minmax(140px, 1fr));
    gap: 15px;
  }

  .item-info h3 {
    font-size: 13px;
    min-height: 36px;
  }

  .current-price {
    font-size: 18px;
  }

  .original-price {
    font-size: 12px;
  }

  .quick-add-btn {
    padding: 8px;
    font-size: 12px;
  }

  .section-header h2 {
    font-size: 32px;
  }

  .outfit-grid {
    grid-template-columns: 1fr;
    gap: 20px;
  }

  .main-image {
    height: 280px;
  }

  .thumbnail-item {
    width: 50px;
    height: 50px;
  }

  .outfit-footer {
    flex-direction: column;
    align-items: flex-start;
    gap: 12px;
  }

  .view-outfit-btn {
    width: 100%;
    justify-content: center;
  }

  .products-grid {
    grid-template-columns: repeat(auto-fill, minmax(150px, 1fr));
    gap: 15px;
  }

  .newsletter {
    padding: 40px 20px;
  }

  .newsletter h2 {
    font-size: 32px;
  }

  .newsletter-form {
    flex-direction: column;
  }
}

@media (max-width: 480px) {
  .quick-buy-section {
    padding: 20px 15px;
  }

  .section-title h2 {
    font-size: 24px;
  }

  .quick-buy-grid {
    grid-template-columns: repeat(2, 1fr);
    gap: 12px;
  }

  .item-info {
    padding: 12px;
  }

  .item-info h3 {
    font-size: 12px;
    min-height: 32px;
  }

  .current-price {
    font-size: 16px;
  }

  .discount-badge {
    font-size: 10px;
    padding: 3px 8px;
  }

  .outfit-section {
    margin-top: 50px;
  }

  .outfit-grid {
    gap: 15px;
  }

  .main-image {
    height: 250px;
  }

  .outfit-info {
    padding: 15px;
  }

  .outfit-header h3 {
    font-size: 18px;
  }

  .outfit-tag {
    font-size: 10px;
    padding: 3px 10px;
  }

  .outfit-desc {
    font-size: 13px;
    min-height: 40px;
  }

  .outfit-price {
    font-size: 20px;
  }

  .thumbnail-grid {
    padding: 10px;
    gap: 6px;
  }

  .thumbnail-item {
    width: 45px;
    height: 45px;
  }
}
</style>
