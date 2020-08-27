<template>
  <div class="slider">
    <div class="frame">
      <div class="container" :style="containerStyle">
        <div class="container__spot" :style="spotStyle" v-for="item in items" :key="item.id">
          <div class="item">
            <div class="loading-message">Loading</div>
            <img
              class="img"
              :src="item.url"
              :alt="item.id"
              v-show="!item.failed"
              @error="onLoadError(item.id)"
            >
          </div>
        </div>
      </div>
    </div>
    <button class="arrow arrow-left" type="button" @click="onArrowLeftClick">
      <span class="fas fa-caret-left icon-arrow-left"></span>
    </button>
    <button class="arrow arrow-right" type="button" @click="onArrowRightClick">
      <span class="fas fa-caret-right icon-arrow-right"></span>
    </button>
  </div>
</template>

<script>
export default {
  name: 'Slider',
  props: {
    items: {
      type: Array,
      default: () => ([]),
    },
  },
  data() {
    return {
      spotStyle: {
        width: '0px',
      },
      containerStyle: {
        transform: 'translateX(0)',
      },
      frameWidth: 0,
      spotWidth: 0,
      offset: 0,
    };
  },
  mounted() {
    this.recalcFrameWidth();
  },
  methods: {
    recalcFrameWidth() {
      this.frameWidth = document.querySelector('.frame').getBoundingClientRect().width;
      this.spotWidth = this.frameWidth / 3;
      this.spotStyle.width = `${this.spotWidth}px`;
    },
    scroll(offset) {
      this.offset = offset;
      this.containerStyle.transform = `translateX(-${offset * this.spotWidth}px)`;
    },
    scrollLeft() {
      if (this.offset > 0) {
        this.scroll(this.offset - 1);
      }
    },
    scrollRight() {
      if (this.items.length > this.offset + 3) {
        this.scroll(this.offset + 1);
      }
    },
    onLoadError(id) {
      const errorItem = this.items.find((item) => item.id === id);
      errorItem.failed = true;
    },
    onArrowLeftClick() {
      this.scrollLeft();
    },
    onArrowRightClick() {
      this.scrollRight();
    },
  },
};
</script>

<style scoped>
.slider {
  position: relative;
  display: flex;
  height: 220px;
  padding: 20px;
}
.frame {
  position: relative;
  width: 100%;
  height: 100%;
  overflow: hidden;
}
.container {
  position: absolute;
  display: flex;
  height: 100%;
  transition: transform .5s;
}
.container__spot {
  display: flex;
  width: 0;
  height: 100%;
  padding: 5px;
  justify-content: center;
  align-items: center;
}
.item {
  position: relative;
  display: flex;
  width: 100%;
  height: 100%;
  border-radius: 10px;
  justify-content: center;
  align-items: center;
  overflow: hidden;
  background: #aaaaaa;
  box-shadow: 1px 1px 4px 0 #333333;
}
.loading-message {
  color: #666666;
}
.img {
  position: absolute;
  /* width: 100%; */
  height: 100%;
}
.arrow {
  position: absolute;
  display: flex;
  width: 40px;
  height: 40px;
  border: 0;
  border-radius: 50%;
  top: 110px;
  justify-content: center;
  align-items: center;
  transform: translateY(-50%);
  opacity: .8;
  background: rgb(211, 122, 58);
  color: #ffffff;
  outline: none;
}
.arrow-left {
  left: 10px;
}
.arrow-right {
  right: 10px;
}
.icon-arrow-left {
  margin-right: 5px;
  font-size: 20px;
}
.icon-arrow-right {
  margin-left: 5px;
  font-size: 20px;
}
</style>
