<template>
  <div class="page-loadmore">

    <div class="page-loadmore-wrapper" ref="wrapper" :style="{ height: wrapperHeight + 'px' }">
      <mt-loadmore
        :top-method="loadTop"
        @translate-change="translateChange"
        @top-status-change="handleTopChange"
        :bottom-method="loadBottom"
        @bottom-status-change="handleBottomChange"
        :bottom-all-loaded="allLoaded"
        ref="loadmore"
      >
        <ul class="page-loadmore-list">
          <li v-for="item in list" :key="item.id" class="page-loadmore-listitem">{{ item }}</li>
        </ul>

        <div slot="top" class="mint-loadmore-top">
          <span v-show="topStatus === 'pull'">下拉刷新</span>
          <span v-show="topStatus === 'drop'">释放更新</span>
          <span v-show="topStatus === 'loading'">加载中...</span>
        </div>

        <div slot="bottom" class="mint-loadmore-bottom">
          <span v-show="bottomStatus === 'pull'">上拉刷新</span>
          <span v-show="bottomStatus === 'drop'">释放更新</span>
          <span v-show="bottomStatus === 'loading'">加载中...</span>
        </div>
        <div v-show="allLoaded">已到达底线！！！</div>
      </mt-loadmore>
    </div>
  </div>
</template>

<style>

.mint-loadmore-top span {
  display: inline-block;
  vertical-align: middle;
}

.page-loadmore .mint-spinner {
  display: inline-block;
  vertical-align: middle;
}

.page-loadmore-listitem {
  border-bottom: 1px solid #eee;
}

.page-loadmore-listitem {
  height: 50px;
  line-height: 50px;
  text-align: center;
}

.page-loadmore-listitem:first-child {
  border-top: 1px solid #eee;
}

.page-loadmore-wrapper {
  overflow: scroll;
}

.mint-loadmore-bottom span {
  display: inline-block;
  vertical-align: middle;
}

</style>

<script type="text/babel">
import MintUI from 'mint-ui'
export default {
  data() {
    return {
      list: [],
      allLoaded: false,

      bottomStatus: "",
      wrapperHeight: 0,

      topStatus: ""
    };
  },

  methods: {
    handleBottomChange(status) {
      this.bottomStatus = status;
    },

    loadBottom() {
      setTimeout(() => {
        let lastValue = this.list[this.list.length - 1];
        if (lastValue < 30) {
          for (let i = 1; i <= 10; i++) {
            this.list.push(lastValue + i);
          }
        } else {
          this.allLoaded = true;
        }
        this.$refs.loadmore.onBottomLoaded();
      }, 1500);
    },

    handleTopChange(status) {
      this.topStatus = status;
      
    },
    translateChange(translate) {

    },
    loadTop() {
      setTimeout(() => {
        let firstValue = this.list[0];
        for (let i = 1; i <= 10; i++) {
          this.list.unshift(firstValue - i);
        }
        this.$refs.loadmore.onTopLoaded();
      }, 1500);
    }
  },

  created() {
    for (let i = 1; i <= 15; i++) {
      this.list.push(i);
    }
  },

  mounted() {
    this.wrapperHeight =
      document.documentElement.clientHeight -
      this.$refs.wrapper.getBoundingClientRect().top;
    MintUI.Toast('Dom加载完')
  }
};
</script>