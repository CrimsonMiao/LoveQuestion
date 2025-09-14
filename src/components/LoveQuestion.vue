<template>
  <div class="love-question-container">
    <div class="content-wrapper">
      <!-- 文字部分 -->
      <h1 class="question-text">{{ currentText }}</h1>
      
      <!-- 图片部分 -->
      <div class="image-container">
        <img 
        :src="currentImage" 
        :alt="imageAlt"
        class="question-image"
        />
      </div>
      
      <!-- 按钮部分 -->
      <div class="buttons-container">
        <button class="like-button" @click="handleLike" :style="likeButtonStyle">
          喜欢
        </button>
        <button class="dislike-button" @click="handleDislike" :style="dislikeButtonStyle">
          不喜欢
        </button>
      </div>
    </div>
  </div>
</template>

<script setup>
import { ref, computed } from 'vue'
import initialImage from '@/assets/initial_image.png'
import finalImage from '@/assets/final_image.png'
import image2 from '@/assets/image_2.png'
import image3 from '@/assets/image_3.png'
import image4 from '@/assets/image_4.png'
import image5 from '@/assets/image_5.png'
import image6 from '@/assets/image_6.png'

// 响应式数据
const isLiked = ref(false)
const dislikeClickCount = ref(0)

// 计算属性
const currentImage = computed(() => {
  if (isLiked.value) {
    return finalImage
  } else {
    switch (dislikeClickCount.value) {
      case 0:
        return initialImage
      case 1:
        return image2
      case 2:
        return image3
      case 3:
        return image4
      case 4:
        return image5
      case 5:
        return image6
      default:
        return image6 // 超过5次点击也显示image6
    }
  }
})

const imageAlt = computed(() => {
  if (isLiked.value) {
    return '最终图片'
  } else {
    switch (dislikeClickCount.value) {
      case 0:
        return '初始图片'
      case 1:
        return '图片2'
      case 2:
        return '图片3'
      case 3:
        return '图片4'
      case 4:
        return '图片5'
      case 5:
        return '图片6'
      default:
        return '图片6'
    }
  }
})

const currentText = computed(() => {
  return isLiked.value ? '赫赫也喜欢你！' : '你喜欢赫赫吗？'
})

const likeButtonStyle = computed(() => {
  const scale = Math.pow(1.8, dislikeClickCount.value) // 每次点击翻倍
  return {
    transform: `scale(${scale})`,
    transformOrigin: 'center'
  }
})

const dislikeButtonStyle = computed(() => {
  // 当喜欢按钮变大时，不喜欢按钮向右移动，保持合适的距离
  const scale = Math.pow(1.4, dislikeClickCount.value) // 和喜欢按钮相同的缩放比例
  const translateX = (scale - 1) * 100 // 根据缩放比例计算移动距离
  return {
    transform: `translateX(${translateX}px)`,
    transformOrigin: 'center'
  }
})

// 方法
const handleLike = () => {
  isLiked.value = true
}

const handleDislike = () => {
  dislikeClickCount.value++
}
</script>

<style scoped>
.love-question-container {
  min-height: 100vh;
  width: 100vw;
  background: linear-gradient(135deg, #f5f7fa 0%, #c3cfe2 100%);
  display: flex;
  align-items: center;
  justify-content: center;
  padding: 2vh 2vw;
  font-family: -apple-system, BlinkMacSystemFont, 'Segoe UI', Roboto, sans-serif;
  position: fixed;
  top: 0;
  left: 0;
  right: 0;
  bottom: 0;
  overflow: hidden;
}

.content-wrapper {
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
  text-align: center;
  width: 100%;
  max-width: 90vw;
  gap: 4vh;
  position: relative;
  z-index: 1;
}

.question-text {
  font-size: clamp(1.5rem, 4vw, 3rem);
  font-weight: 700;
  color: #2c3e50;
  margin: 0;
  text-shadow: 0 2px 4px rgba(0, 0, 0, 0.1);
  letter-spacing: 0.05em;
  position: relative;
  z-index: 10;
}

.image-container {
  width: 100%;
  max-width: min(60vw, 400px);
  aspect-ratio: 1/1;
  overflow: hidden;
  box-shadow: 0 8px 32px rgba(0, 0, 0, 0.1);
  transition: all 0.3s ease;
  position: relative;
  z-index: 10;
}

.question-image {
  width: 100%;
  height: 100%;
  object-fit: cover;
  transition: all 0.3s ease;
}

.buttons-container {
  display: flex;
  flex-direction: row;
  gap: 2vw;
  align-items: center;
  justify-content: center;
  position: relative;
  width: 100%;
  max-width: 50vw;
  margin: 0 auto;
  z-index: 1;
}

.like-button,
.dislike-button {
  padding: 1.2em 2.4em;
  border: none;
  border-radius: 2em;
  font-size: clamp(1rem, 2.5vw, 1.5rem);
  font-weight: 600;
  cursor: pointer;
  transition: all 0.3s ease;
  width: 50%;
  max-width: 12em;
  min-width: 8em;
  height: 3.5em;
  position: relative;
  box-shadow: 0 4px 16px rgba(0, 0, 0, 0.1);
  transform-origin: center;
  flex: 0 0 auto;
  display: flex;
  align-items: center;
  justify-content: center;
  z-index: 1;
}

.like-button {
  background: linear-gradient(135deg, #ff6b6b, #ee5a52);
  color: white;
}

.like-button:hover {
  background: linear-gradient(135deg, #ff5252, #e53935);
  transform: translateY(-2px);
  box-shadow: 0 6px 20px rgba(255, 107, 107, 0.3);
}

.dislike-button {
  background: linear-gradient(135deg, #74b9ff, #0984e3);
  color: white;
}

.dislike-button:hover {
  background: linear-gradient(135deg, #0984e3, #0770c4);
  transform: translateY(-2px);
  box-shadow: 0 6px 20px rgba(116, 185, 255, 0.3);
}

.like-button:active,
.dislike-button:active {
  transform: translateY(0);
}

/* 响应式设计 */
@media (min-width: 1024px) {
  .content-wrapper {
    max-width: 600px;
    gap: 5vh;
  }
  
  .image-container {
    max-width: 350px;
  }
  
  .buttons-container {
    max-width: 400px;
    gap: 2rem;
  }
  
  .like-button,
  .dislike-button {
    width: 50%;
    font-size: 1.3rem;
  }
}

@media (max-width: 768px) {
  .content-wrapper {
    gap: 3vh;
    max-width: 95vw;
  }
  
  .image-container {
    max-width: min(70vw, 300px);
  }
  
  .buttons-container {
    gap: 1.5vw;
    max-width: 80vw;
  }
  
  .like-button,
  .dislike-button {
    width: 50%;
    font-size: 1.1rem;
    min-width: 6em;
  }
}

@media (max-width: 480px) {
  .content-wrapper {
    gap: 2.5vh;
    max-width: 98vw;
  }
  
  .image-container {
    max-width: min(80vw, 250px);
  }
  
  .buttons-container {
    flex-direction: row;
    gap: 1vw;
    max-width: 90vw;
  }
  
  .like-button,
  .dislike-button {
    width: 50%;
    font-size: 1rem;
    min-width: 5em;
    height: 3em;
  }
}
</style>
