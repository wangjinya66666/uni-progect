<template>
  <scroll-view  @scrolltolower="handleToLower" scroll-y class="scroll-box" v-if="recommends.length>0">
    <!-- 推荐列表开始 -->
    <view class="images">
      <view v-for="item in recommends" :key="item.id" class="image">
        <image :src="item.thumb" mode="widthFix"></image>
      </view>
    </view>
    <!-- 推荐列表结束 -->
    <!-- 月份开始 -->
    <view class="more">
      <view class="more-title">
        <view class="title-left">
          <text class="left-day">{{month.yue}}</text>{{'/'+month.ri}}月
          <text class="left-txt">{{month.title}}</text>
        </view>
        <view class="title-right">更多></view>
      </view>
      <view class="more-images">
        <view class="image" v-for="item in month.items" :key="item.id">
          <image :src="item.img+item.rule.replace('$<Height>',360)" mode="aspectFill"></image>
        </view>
      </view>
    </view>
    <!-- 月份结束 -->
    <!-- 热门开始 -->
    <view class="new">
      <view class="new-title">热门</view>
      <view class="images">
        <view class="image" v-for="item in newMsg" :key="item.id" >
          <image :src="item.thumb" mode="widthFix"></image>
        </view>
      </view>
    </view>
    <!-- 热门结束 -->
  </scroll-view>
</template>

<script>
import moment from 'moment'
export default {
  data(){
    return{
      //推荐列表
    recommends:[],
    //月份数据
    month:{},
    // 热门数据
    newMsg:{},
    //请求的参数
    params:{
      limit:30,
      order:'hot',
      skip:0
    }
    }
  },
  mounted(){
   this.getMsg()
  },
  methods:{
    //获取接口数据
    getMsg(){
      this.request({
        url:'http://service.picasso.adesk.com/v3/homepage/vertical',
        data:this.params
      })
      .then(result=>{
        console.log('首页总数据',result);
        //推荐列表
        this.recommends=result.res.homepage[1].items;
        //月份数据
        this.month=result.res.homepage[2];
        //时间戳转化月份和日
        this.month.yue=moment(this.month.stime).format('M');
        this.month.ri=moment(this.month.stime).format('D');
        // 热门数据
        this.newMsg=result.res.vertical;
        console.log(this.newMsg);
      })
    },
    //滚动条触底 skip+=limit
    handleToLower(){
      this.params.skip+=this.params.limit;
      console.log(this.params.skip);
      // this.getMsg()

    }
  }
}
</script>

<style lang="scss" scoped>
.scroll-box{
  height: 100vh;
    .images{
    display: flex;
    flex-wrap:wrap;
    .image{
      box-sizing: border-box;
      width: 50%;
      border: 6rpx solid #fff;
      image{
        width: 100%;
      }
    }
  }
  .more {
    .more-title {
      display: flex;
      justify-content: space-between;
      padding: 0 30rpx;
      line-height: 80rpx;
      font-size: 30rpx;
      .title-left {
        .left-day {
          font-size: 36rpx;
          font-weight: 600;
          color: $color;
        }
      }
      .title-right {
          color: $color;
      }
    }
  
    .more-images {
      display: flex;
      flex-wrap: wrap;
      .image{
        box-sizing: border-box;
        width: 33.33%;
        border: 6rpx solid #fff;
        image{
          width: 100%;
        }
      }
    }
  }
  .new{
    .new-title{
    padding: 0 30rpx;
    line-height: 80rpx;
  }
  .images{
      display: flex;
      flex-wrap: wrap;
      .image{
        width: 33.33%;

      }
    }
  }
}

</style>