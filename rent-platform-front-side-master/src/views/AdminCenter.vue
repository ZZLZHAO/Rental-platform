<template>
  <div class="admin">
    <div class="section-banner">
      <div class="section-banner-main">
        <div class="user-head">
          <img class="user-avatar" @click="showOption()" :src="user.avatar" />
          <div
              class="option-box-container"
              v-show="option"
              @mouseleave="hideOption()"
          >
            <div class="option-box" @click="logout()">
              <div class="option-item">
                <i class="el-icon-switch-button" />退出登录
              </div>
            </div>
          </div>
        </div>
        <div class="username">{{ user.nickName }}</div>
        <div class="nav-bar">
          <div @click="showPending" class="nav-item">上架物品</div>
          <div class="split">|</div>
          <div @click="showReturn" class="nav-item">归还订单</div>
        </div>
      </div>
    </div>
    <div v-if="isPending" class="item-list">
      <div class="section-featured">
        <div class="section-featured-container">
          <div class="section-featured-main">
            <commodityReviewCard
                v-for="item in this.myCollects"
                :key="item.object_id"
                :cardInfo="item"
            />
          </div>
        </div>
      </div>
    </div>
    <div v-if="isReturn" class="item-list">
      <div class="section-featured">
        <div class="section-featured-container">
          <div class="section-featured-main">
            <orderCard
                v-for="item in orderList"
                :key="item.order_id"
                :cardInfo="item"
            />
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import { WOW } from 'wowjs';
import commodityReviewCard from '../components/CommodityReviewCard.vue';
import orderCard from '../components/AdminOrderCard.vue';
import axios from '../axios';
export default {
  components: {
    commodityReviewCard,
    orderCard,
  },
  computed: {
    user() {
      return this.$store.state.user;
    },
  },
  data() {
    return {
      option: false,
      isPending:true,
      isReturn:false,
      myCollects: [],
      orderList: [
        {
          picture_url:"",
          order_id:52,
          object_id:2,
          borrower_id:1,
          status:"待评价",
          rent_total:120,
          lentout_time:"2022-06-21T00:00:00",
          return_time:"2022-06-22T00:00:00",
          created_time:"2022-06-21T23:24:57",
          campus:""
        }
      ],
    };
  },
  mounted() {
    this.$nextTick(() => {
      // 在dom渲染完后,再执行动画
      const wow = new WOW({
        live: false,
      });
      wow.init();
    });
    axios.getPendingObj().then((res) => {
      this.myCollects = res.data.data.records;
    });

    axios.getTodayReturnList()
        .then((res) => {
          console.log('return order:', res.data);
          this.orderList = res.data.data;
        });
  },
  methods: {
    logout() {
      this.$store.commit('LOGOUT');
      this.$router.push('/');
    },
    showOption() {
      this.option = true;
    },
    hideOption() {
      this.option = false;
    },
    showPending(){
      this.isPending=true;
      this.isReturn=false;
    },
    showReturn(){
      this.isPending=false;
      this.isReturn=true;
    }
  },
};
</script>


<style lang="scss" scoped>
.admin {
  width: 100%;
  .section-banner {
    padding-top: 70px;
    padding-bottom: 70px;
    display: flex;
    flex-direction: column;
    justify-content: center;
    align-items: center;
    background-image: url("../assets/img/bg-banner-article.jpg");
    background-size: cover;
    background-position: 50% 50%;
    background-repeat: no-repeat;
    .section-banner-main {
      display: flex;
      flex-direction: column;
      justify-content: flex-start;
      align-items: center;
      .user-head {
        display: flex;
        flex-direction: column;
        justify-content: flex-start;
        .user-avatar {
          width: 100px;
          height: 100px;
          border-radius: 50px;
          background-color: gray;
        }
        .option-box-container {
          height: 0;
          width: 0;
          .option-box {
            padding: 8px 0;
            width: 100px;
            background-color: rgb(59, 103, 235);
            border-radius: 10px;
            transform: translateX(-25%) translateY(10px);
            &:hover {
              background-color: rgb(29, 76, 218);
            }
            .option-item {
              user-select: none;
              color: white;
              width: 100%;
              text-align: center;
            }
          }
        }
      }
      .username {
        user-select: none;
        font-size: 19px;
        text-align: center;
        color: #222222;
        padding-bottom: 18px;
      }
      .nav-bar{
        display: flex;
        flex-direction: row;
        justify-content: center;
        align-items: center;
        .split{
          margin-left:15px;
          margin-right: 18px;
          color:gray;
        }
        .nav-item{
          user-select: none;
          font-size: 15.6px;
          color:#222222;
          transition: all .3s ease;
          text-decoration: none;
          &:hover{
            color:#E63A28;
          }
        }
      }
    }
  }
}
.item-list {
  width: 100%;
  height: auto;

  .section-featured {
    width: 100%;
    .section-featured-container {
      width: 100%;
      padding: 120px 40px;
      display: flex;
      flex-direction: column;
      justify-content: flex-start;
      align-items: center;
      .section-featured-title {
        width: 100%;
        height: auto;
        display: flex;
        flex-direction: column;
        justify-content: flex-start;
        align-items: center;
        margin-bottom: 60px;
        .section-title {
          //这一块的实现需要学习
          user-select: none;
          font-size: 15.6px;
          position: relative;
          color: gray;
          display: inline-block;
          margin-bottom: 18px;
          &:before {
            display: block;
            position: absolute;
            top: 50%;
            width: 37px;
            height: 1px;
            background-color: #e63a28;
            right: calc(100% + 23px);
            content: "";
          }
          &:after {
            display: block;
            position: absolute;
            top: 50%;
            width: 37px;
            height: 1px;
            background-color: #e63a28;
            left: calc(100% + 23px);
            content: "";
          }
        }
        .section-title-main {
          font-size: 45px;
          font-weight: 700;
        }
        .section-title-p {
          margin-top: 15px;
          margin-bottom: 0;
          color: gray;
          font-family: Quicksand;
          font-size: 15.4px;
          font-style: normal;
          font-weight: normal;
          letter-spacing: -0.025em;
          line-height: 1.63;
          text-align: center;
          width: 520px;
        }
      }

      .section-featured-main {
        display: flex;
        flex-direction: row;
        flex-wrap: wrap;
        justify-content: center;
        align-items: center;
        margin-left: -30px;
        width: 1470px;
        .section-featured-card {
          flex-shrink: 0;
          margin-left: 30px;
          margin-bottom: 30px;
          width: 360px;
          height: 535px;
          background-color: lightpink;
          border-radius: 20px;
          transition: all 0.3s ease;
          &:hover {
            box-shadow: 25px 25px 50px #b8b8b8;
          }
        }
      }

      .load-more {
        user-select: none;
        margin-top: 70px;
        height: 50px;
        color: white;
        background-color: #e63a28;
        border-radius: 25px;
        font-size: 17.2px;
        font-weight: 400;
        width: 150px;
        display: flex;
        flex-direction: column;
        justify-content: center;
        align-items: center;
        transition: all 0.3s ease;
        &:hover {
          background-color: #c43323;
        }
      }
    }
  }
}
</style>
