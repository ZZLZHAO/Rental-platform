<template>
  <div class="item-list">
    <div class="section-featured">
        <div class="section-featured-container">
          <div class="section-featured-main">
            <commodityCard
            v-for="item in this.myCollects"
            :key=item.object_id
            :cardInfo="item"
            />
          </div>

          <!-- <div class="load-more">Load More</div> -->
        </div>
      </div>
    </div>
</template>

<script>
import { WOW } from 'wowjs';
import commodityCard from '../../components/CommodityCard.vue';
import axios from '../../axios';

export default {
  components: {
    commodityCard,
  },
  mounted() {
    this.$nextTick(() => {
      // 在dom渲染完后,再执行动画
      const wow = new WOW({
        live: false,
      });
      wow.init();
    });

    axios.getCollect()
      .then((res) => {
        console.log(res.data.data);
        const raw = res.data.data;
        const total = [];
        raw.forEach((item) => {
          const data = {
            picture_url: item.urls[0],
            name: item.name,
            description: item.description,
            rent_daliy: item.rent_daliy,
            avatar: item.avatar,
            nick_name: item.nick_name,
            object_id: item.object_id,
          };
          total.push(data);
        });
        console.log('mycollect', total);
        this.myCollects = total;
      });
  },

  data() {
    return {
      myCollects: [],
    };
  },
  watch:{
    myCollects:{
      handler (newName, oldName) {
        console.log(newName)
        axios.getCollect()
            .then((res) => {
              console.log(res.data.data);
              const raw = res.data.data;
              const total = [];
              raw.forEach((item) => {
                const data = {
                  picture_url: item.urls[0],
                  name: item.name,
                  description: item.description,
                  rent_daliy: item.rent_daliy,
                  avatar: item.avatar,
                  nick_name: item.nick_name,
                  object_id: item.object_id,
                };
                total.push(data);
              });
              console.log('mycollect', total);
              this.myCollects = total;
            });
      },
      immediate: true
    }
  }
};
</script>

<style lang="scss" scoped>
.item-list{
  width:100%;
  height:auto;

  .section-featured{
    width:100%;
    .section-featured-container{
      width:100%;
      padding:120px 40px;
      display: flex;
      flex-direction: column;
      justify-content: flex-start;
      align-items: center;
      .section-featured-title{
        width:100%;
        height:auto;
        display: flex;
        flex-direction: column;
        justify-content: flex-start;
        align-items: center;
        margin-bottom: 60px;
        .section-title{
          //这一块的实现需要学习
          user-select: none;
          font-size: 15.6px;
          position: relative;
          color:gray;
          display: inline-block;
          margin-bottom: 18px;
          &:before{
            display: block;
            position: absolute;
            top: 50%;
            width: 37px;
            height: 1px;
            background-color: #E63A28;
            right: calc(100% + 23px);
            content: '';
          }
          &:after{
            display: block;
            position: absolute;
            top: 50%;
            width: 37px;
            height: 1px;
            background-color: #E63A28;
            left: calc(100% + 23px);
            content: '';
          }
        }
        .section-title-main{
          font-size: 45px;
          font-weight:700;
        }
        .section-title-p{
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

      .section-featured-main{
        display: flex;
        flex-direction: row;
        flex-wrap: wrap;
        justify-content: center;
        align-items: center;
        margin-left: -30px;
        width:1170px;
        .section-featured-card{
          flex-shrink: 0;
          margin-left: 30px;
          margin-bottom: 30px;
          width:360px;
          height:535px;
          background-color: lightpink;
          border-radius: 20px;
          transition: all .3s ease;
          &:hover{
            box-shadow:  25px 25px 50px #b8b8b8,

          }
        }
      }

      .load-more{
        user-select: none;
        margin-top:70px;
        height:50px;
        color:white;
        background-color: #E63A28;
        border-radius: 25px;
        font-size:17.2px;
        font-weight: 400;
        width:150px;
        display: flex;
        flex-direction: column;
        justify-content: center;
        align-items: center;
        transition: all .3s ease;
        &:hover{
          background-color: #c43323;
        }
      }
    }
  }
}
</style>
