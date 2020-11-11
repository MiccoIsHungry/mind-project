<template>
  <div class = 'page-home'>
    <index-header></index-header>
    <div class="index-main">
      <Swiper :autoplay = "3000" :loop="true" @change="changeHandler" class="my-swiper" v-if = "bannerList.length > 0">
      <SwiperItem v-for = "item in bannerList" :key = "item.id">
        <img
              :src="item.imageurl"
              alt
            />
      </SwiperItem>
    </Swiper>
      <index-nav></index-nav>
      <index-recommend v-for = "item in recommentList" :key="item.specialid" :item = "item"></index-recommend>
      <div class="my-icp">
        <a
          class="record"
          href="http://www.beian.gov.cn/portal/registerSystemInfo?recordcode=31011202006214"
          target="_blank"
        >
          <img class="img" src="https://wechatapp.zhuishushenqi.com/mhd/201711/gongan.jpg" />
          <div>沪公网安备 31011202006214号</div>
        </a>
        <div class="licence">
          增值电信业务经营许可证沪B2-20170022
          <br />网络文化经营许可证沪网文（2016）3206-227号
          <br />出版物经营许可证新出发沪批字第U7659号
        </div>
      </div>
    </div>
  </div>
</template>
<script>
import { Swiper, SwiperItem } from '@/components/Swiper'
import { getBanner, getIndexRecomment } from '@/api/cartoon'
import indexHeader from './components/indexHeader'
import indexNav from './components/indexNav'
import indexRecommend from './components/indexRecommend'
export default {
  name: 'Home',
  data () {
    return {
      bannerList: [],
      recommentList: []
    }
  },
  components: {
    Swiper,
    SwiperItem,
    indexHeader,
    indexNav,
    indexRecommend
  },
  methods: {
    changeHandler (payload) {
      // console.log('index', payload)
    },
    getBanner () {
      getBanner()
        .then(res => {
          // 判断
          if (res.data.code === 200) {
            this.bannerList = res.data.info
            console.log(res.data.info)
          } else {
            console.log(res.data.code_msg)
          }
        })
        .catch((err) => {
          alert('网络异常，请稍后' + err)
        })
    },
    getIndexRecomment () {
      getIndexRecomment().then(res => {
        if (res.data.code === 200) {
          this.recommentList = res.data.info
          // console.log(res.data.info)
        } else {
          console.log(res.code_msg)
        }
      }).catch((err) => {
        alert('网络异常，请稍后' + err)
      })
    }
  },
  created () {
    this.getBanner()
    this.getIndexRecomment()
  }
}
</script>

<style lang="scss" scoped>
@import "~@/assets/styles/mixins.scss";
.page-home {
  .index-main{
    flex: 1;
    overflow-y: auto;
  }
  .my-swiper{
    img {
      width: 100%;
    }
  }
  display: flex;
  flex-direction: column;
  height: 100%;
  .my-icp {
  padding: 5px 0;
  font-size: 12px;
  .record {
    display: flex;
    align-items: center;
    justify-content: center;
    .img {
      width: 15px;
      height: 15px;
      margin-right: 4px;
    }
  }
  .licence {
    margin-top: 4px;
    text-align: center;
  }
}
}
</style>
