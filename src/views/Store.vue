<template>
  <div>
    <AppHeader></AppHeader>
    <div class="container container--product">
      <ButtonHome></ButtonHome>
      <div class="product">
        <div class="product__description">
          <h1 class="product__title">Хрень <span class="product__price">999 ₽</span></h1>
          <p class="product__desc">
            Универсальный предмет, который может использоваться в различных сферах жизни. Этот товар является
            незаменимым помощником в быту, строительстве, ремонте и других областях.
          </p>
          <p>
            <i>* Уточняйте наличие у продавца</i>
          </p>
        </div>
        <div class="product__gallery">
          <div class="product__loading">Загрузка <span>0</span>%</div>
          <img :src="this.publicPath + 'assets/swipe.svg'" alt="Свайпните для вращения" class="product__swipe">
          <canvas width="300" height="300"></canvas>
        </div>
      </div>
    </div>
    <AppFooter></AppFooter>
  </div>
</template>

<script>
import AppHeader from "@/components/AppHeader.vue";
import AppFooter from "@/components/AppFooter.vue";
import ButtonHome from "@/components/ButtonHome.vue";

export default {
  name: "store",
  components: { AppHeader, AppFooter, ButtonHome },
  data() {
    return {
      sitename: "Driv3r - Каталог игр",
      publicPath: process.env.BASE_URL
    };
  },
  mounted() {
    let loadingBlock = document.querySelector(".product__loading");
    let element = document.querySelector('canvas');
    let imagesArray = Array.from(new Array(30), (v, k) => {
      let number = String(k).padStart(1, "1");
      return `/catalog-online/store/thing/${number}.jpg`;
    });
    let instance = new AnimateImages(element, {
      images: imagesArray,
      preload: "all",
      preloadNumber: 0,
      loop: true,
      fps: 30,
      autoplay: false,
      draggable: true,
      touchScrollMode: "allowPageScroll",
      poster: '/catalog-online/store/thing/0.jpg',
    });
    element.addEventListener("animate-images:loading-progress", function (e) {
      loadingBlock.querySelector("span").textContent = Math.floor(
        +e.detail.progress * 100
      );
    });
  }
}
</script>

<style scoped>
.container--product {
  width: 100%;
}

.product {
  margin-top: 24px;
}

canvas {
  cursor: grab;
  border-radius: 8px;
}

.product__price {
  display: inline-block;
  padding: 8px 12px;
  background-color: #7e122d;
  border-radius: 8px;
}

.product__desc {
  max-width: 500px;
}

.product__gallery {
  position: relative;
}

.product__swipe {
  position: absolute;
  bottom: 16px;
  right: 16px;
  width: 24px;
}

.product__loading {
  position: absolute;
  top: 0;
  right: 0;
  padding: 8px;
  font-size: 12px;
  background: rgba(0, 0, 0, 0.3);
}

@media (min-width: 576px) {
  .product {
    display: flex;
    gap: 16px;
  }

  .product__description {
    order: 2;
    flex-shrink: 1;
  }

  .product__gallery {
    width: 50%;
    max-width: 300px;
  }
}

@media (min-width: 1024px) {
  canvas {
    width: 350px;
    height: 350px;
  }

  .product {
    gap: 24px;
  }

  .product__gallery {
    max-width: 350px;
  }
}

@media (min-width: 1200px) {
  canvas {
    width: 450px;
    height: 450px;
  }

  .product {
    gap: 24px;
  }

  .product__gallery {
    max-width: 450px;
  }
}
</style>