<template>
  <div class="rating" :style="ratingStyle" @mouseout="lightStar(lightOnNum*ratio)">
    <div class="rating-display" :style="ratingDisplayStyle"></div>
    <ul class="rating-mask">
      <li class="rating-item" :style="ratingItemStyle" v-for="index in total*ratio" @mouseover="!lock && lightStar(index)" @click="!lock && chooseStar(index)"></li>
    </ul>
  </div>
</template>
<script>
export default {
  data() {
      return {
        lock: false,
        defaultRatingItemWidth: 32.7,
        ratingStyle: {
          width: '162.5px'
        },
        ratingDisplayStyle: {
          width: 0
        },
        ratingItemStyle: {
          width: '32.7px'
        },
        strategy: {
          'entire': 1,
          'half': 2,
          'quarter': 4
        }
      }
    },
    props: {
      lightOnNum: {
        type: Number,
        default: 0
      },
      total: {
        type: Number,
        default: 5
      },
      lockMode: {
        type: Boolean,
        default: false
      },
      mode: {
        type: String,
        default: 'entire'
      }
    },
    methods: {
      lightStar: function(num) {
        this.ratingDisplayStyle.width = num * parseFloat(this.ratingItemStyle.width) + 'px';
      },
      chooseStar: function(num) {
        if (this.lock) {
          return
        }

        this.lightStar(this.num);
        this.$emit('update:lightOnNum', num / this.ratio);

        if (this.lockMode) {
          this.lock = true;
        }
      }
    },
    watch: {
      total: function(newValue) {
        this.ratingStyle.width = this.total * this.ratio * parseFloat(this.ratingItemStyle.width) + 'px';
      },
      ratio: function(val) {
        this.ratingItemStyle.width = (this.defaultRatingItemWidth / val) + 'px';
      }
    },
    computed: {
      ratio: function() {
        return (this.strategy[this.mode]) ? this.strategy[this.mode] : 1;
      }
    },
    mounted: function() {
      console.log(parseFloat(this.ratingItemStyle.width));
      this.ratingItemStyle.width = (parseFloat(this.ratingItemStyle.width) / this.ratio) + 'px';
      this.ratingStyle.width = this.total * this.ratio * parseFloat(this.ratingItemStyle.width) + 'px';
      this.lightStar(this.lightOnNum)
    }
}
</script>
<style scoped>
body,
ul,
li {
  padding: 0;
  margin: 0;
}

li {
  list-style-type: none;
}

.rating,
.rating-display {
  height: 32.7px;
  background: url(../assets/img/StarRating.png) repeat-x;
}

.rating {
  position: relative;
}

.rating-display {
  background-position: -1px -40px;
}

.rating-mask {
  position: absolute;
  left: 0px;
  top: 0px;
  width: 100%;
}

.rating-item {
  float: left;
  height: 32.7px;
  cursor: pointer;
}
</style>
