<template>
  <div class="front-page">
    <div class="top-btn top-left-btn">
      <span>T</span>
    </div>
    <div class="top-btn menu-btn" @click="onMenuBtnClick">
      <span class="fas fa-bars"></span>
      <div class="menu" v-show="menuDisplayed" ignore-click>
        <a
          v-for="link in menuLinks"
          :key="link.title"
          class="menu__link"
          ignore-click
          :href="link.url"
          target="_blank"
        >{{ link.title }}</a>
      </div>
    </div>
    <div class="detail">
      <div class="detail__title">Product Items Summary</div>
      <div class="detail__label-area">
        <div class="detail__deposited-label detail__label">Deposited</div>
        <div class="detail__deposited-value detail__value">{{ format(deposited) }}</div>
        <div class="detail__withdrawn-label detail__label">Withdrawn</div>
        <div class="detail__withdrawn-value detail__value">{{ format(withdrawn) }}</div>
      </div>
      <div class="detail__pie-chart" :style="pieChartStyle"></div>
    </div>
    <div class="description">{{ description }}</div>
    <div class="tabs">
      <div
        class="tab"
        :class="{'tab--selected': index === selectedTabIndex}"
        v-for="(tab, index) in tabs"
        :key="tab.id"
        @click="onTabClick(index)"
      >
        <div class="tab__title">{{ tab.title }}</div>
      </div>
    </div>
    <div class="tab-content">
      <Slider :items="items"/>
    </div>
  </div>
</template>

<script>
import Slider from '@/components/Slider.vue';
import axios from 'axios';

export default {
  name: 'FrontPage',
  components: {
    Slider,
  },
  data() {
    return {
      menuDisplayed: false,
      menuLinks: [
        {
          title: 'Image 1',
          url: 'https://via.placeholder.com/150/92c952',
        },
        {
          title: 'Image 2',
          url: 'https://via.placeholder.com/150/771796',
        },
        {
          title: 'Image 3',
          url: 'https://via.placeholder.com/150/24f355',
        },
      ],
      deposited: 2500,
      withdrawn: 700,
      pieChartStyle: {
        background: '',
      },
      description: 'Lorem ipsum dolor sit amet, consectetur adipisicing elit.',
      tabs: [
        {
          id: 0,
          title: 'Available Items',
        },
        {
          id: 1,
          title: 'Information One',
        },
      ],
      selectedTabIndex: 0,
      items: [],
    };
  },
  async created() {
    this.initPieChart();
    await this.loadImages();
  },
  mounted() {
  },
  methods: {
    format(number) {
      return `$${number.toFixed(2)}`;
    },
    initPieChart() {
      const total = this.deposited + this.withdrawn;
      const depositedDeg = (this.deposited / total) * 360;
      const withdrawnDeg = (this.withdrawn / total) * 360;
      this.pieChartStyle.background = `conic-gradient(rgb(229, 113, 63) ${depositedDeg}deg, rgb(223, 63, 52) ${withdrawnDeg}deg)`;
    },
    onTabClick(index) {
      this.selectedTabIndex = index;
    },
    async loadImages() {
      const response = await axios({
        url: 'http://jsonplaceholder.typicode.com/photos',
        method: 'GET',
      });
      const items = response.data
        .filter((_, index) => index < 6)
        .map((item) => ({
          id: item.id,
          url: item.url,
          failed: false,
        }));
      this.items.splice(0, this.items.length, ...items);
    },
    onMenuBtnClick(event) {
      const ignoreClick = event.target.getAttribute('ignore-click');
      if (typeof ignoreClick === 'string') {
        return;
      }
      this.menuDisplayed = !this.menuDisplayed;
    },
  },
};
</script>

<style scoped>
.front-page {
  position: absolute;
  display: flex;
  top: 0;
  bottom: 0;
  left: 0;
  right: 0;
  flex-direction: column;
}
.top-btn {
  position: absolute;
  display: flex;
  top: 20px;
  width: 30px;
  height: 30px;
  border-radius: 4px;
  justify-content: center;
  align-items: center;
  z-index: 1;
  background: rgb(229, 113, 63);
}
.top-left-btn {
  left: 20px;
}
.menu-btn {
  right: 20px;
}
.menu {
  position: absolute;
  display: flex;
  width: 100px;
  padding: 4px 0;
  top: 100%;
  right: 0;
  margin-top: 10px;
  border-radius: 4px;
  flex-direction: column;
  background: rgb(80, 80, 80);
  color: #ffffff;
  z-index: 2;
}
.menu__link {
  margin: 4px 0;
  padding: 0 8px;
  color: #ffffff;
  text-decoration: none;
}
.detail {
  position: relative;
  flex: 1;
  border-radius: 0 0 50px 50px;
  background: #000000;
}
.detail__title {
  position: absolute;
  width: 150px;
  top: 100px;
  left: 50px;
  font-size: 20px;
  color: #ffffff;
}
.detail__label-area {
  position: absolute;
  width: 150px;
  top: 180px;
  left: 50px;
}
.detail__deposited-label {
  color: rgb(229, 113, 63);
}
.detail__withdrawn-label {
  color: rgb(223, 63, 52);
}
.detail__value {
  color: #ffffff;
}
.detail__pie-chart {
  position: absolute;
  width: 150px;
  height: 150px;
  top: 100px;
  right: 50px;
  border-radius: 50%;
}
.description {
  padding: 30px;
  color: #ffffff;
}
.tabs {
  display: flex;
}
.tab {
  flex: 1;
  height: 50px;
  line-height: 50px;
  text-align: center;
  background: rgb(88, 88, 88);
  color: #ffffff;
}
.tab--selected {
  border-bottom: 2px solid rgb(229, 113, 63);
}
</style>
