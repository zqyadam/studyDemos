<template>
  <ul class="rating" @mouseout="mouseOut" :style="{width:ratingWidth}">
    <li class="rating-item" v-for="(title,index) in titles" :title="title" @mousemove="!lock && lightStar(index+1,$event)" @click="!lock && setLightOnNum(index+1,$event)" :class="ratingItemsClass[index]">
    </li>
  </ul>
</template>
<script>
export default {
  data() {
      return {
        num: 0,
        ratingItemsClass: [],
        ratingWidth: 165,
        step: 1,
        lock: false,
      }
    },
    props: {
      lightOnNum: {
        type: Number,
        default: 0
      },
      titles: {
        type: Array,
        default: function() {
          return [
            "很不好 ",
            "不好 ",
            "一般 ",
            "好 ",
            "很好 "
          ]
        }
      },
      mode: {
        type: String,
        default: 'lightEntire'
      },
      lockMode: {
        type: Boolean,
        default: false
      }
    },
    methods: {
      /**
       * 点亮星星
       * @param  {[type]} num [description]
       * @param  {[type]} e   [description]
       * @return {[type]}     [description]
       */
      lightStar: function(num, e) {
        if (this.mode === 'lightEntire') {
          // 整星模式
          this.step = 0;
          let wholeNum = parseInt(num);
          num = (wholeNum === num) ? wholeNum : wholeNum + 1;
          this.lightEntire(num)
        } else if (this.mode === 'lightHalf') {
          // 半星模式
          if (e) {
            if (((e.pageX - e.target.offsetLeft) < e.target.offsetWidth / 2)) {
              this.step = 0.5;
              num = num - this.step
            } else {
              this.step = 0
            }
          }
          this.lightHalf(num);
        } else {
          throw new Error('StarRating:点亮星星的方法不存在！')
        }
      },
      /**
       * 点亮所有星星
       * @param  {Number} num 点亮星星个数，整数
       */
      lightEntire: function(num) {
        num = parseInt(num);
        let newRatingItemsClass = new Array(this.titles.length).fill('lightOff');
        // 点亮全部
        for (let i = 0; i < num; i++) {
          newRatingItemsClass[i] = 'lightOn';
        }
        this.ratingItemsClass = newRatingItemsClass;
      },
      /**
       * 点亮半颗星星
       * @param  {Number} num 点亮星星个数
       */
      lightHalf: function(num) {
        let newRatingItemsClass = new Array(this.titles.length).fill('lightOff'),
          isHalf = parseInt(num) !== num;
        num = parseInt(num);
        // 点亮之前全部
        for (let i = 0; i < num; i++) {
          newRatingItemsClass[i] = 'lightOn';
        }
        // 点亮最后半颗
        if (isHalf) {
          newRatingItemsClass[num] = 'lightHalf';
        }

        this.ratingItemsClass = newRatingItemsClass;
      },
      setLightOnNum: function(index, e) {
        this.num = index - this.step;
        this.$emit('update:lightOnNum', this.num);
        this.$emit('select', this.num);
        if (this.lockMode) {
          this.lock = true;
        }
      },
      mouseOut: function() {
        this.lightStar(this.num);
      },
      unBindEvents: function() {

      }
    },
    mounted: function() {
      this.num = this.lightOnNum;
      this.ratingWidth = 33 * this.titles.length;
      this.lightStar(this.num);
    }
}
</script>
<style scoped>
ul,
li {
  margin: 0;
  padding: 0;
}

li {
  list-style-type: none;
}

.rating {
  height: 33px;
  /*border: 1px solid red;*/
}

.rating-item {
  width: 33px;
  height: 33px;
  float: left;
  background: url(../assets/img/StarRating.png) no-repeat;
  cursor: pointer;
}

.lightOn {
  background-position: -1px -40px;
}

.lightOff {
  background-position: 0 0;
}

.lightHalf {
  background-position: 0 -80px;
}
</style>
