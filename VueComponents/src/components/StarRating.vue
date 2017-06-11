<template>
  <ul id="rating" @mouseout="mouseOut">
    <li class="rating-item" v-for="(title,index) in titles " :title="title" @mouseover="lightOn(index+1)" @click="setLightOnNum(index+1)">
    </li>
  </ul>
</template>
<script>
import $ from 'jquery'

export default {
  data() {
      return {
        $rating: null,
        $item: null,
        num: 0
      }
    },
    props: {
      lightOnNum: {
        type: Number,
        default: 2
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

      }
    },
    methods: {
      lightOn: function(num) {
        let _this = this;
        this.$item.each(function(index) {
          if (index < num) {
            $(this).css('background-position', '-1px -40px');
          } else {
            $(this).css('background-position', '0 0');
          }
        })
      },
      setLightOnNum: function(index) {
        this.num = index;
        this.$emit('update:lightOnNum', this.num)
      },
      mouseOut: function() {
        this.lightOn(this.num);
      }
    },
    mounted: function() {
      this.$rating = $('#rating');
      this.$item = this.$rating.find('.rating-item');
      this.num = this.lightOnNum;
      this.lightOn(this.num);
      this.$rating.width(33*this.titles.length)
    }
}
</script>
<style>
ul,
li {
  margin: 0;
  padding: 0;
}

li {
  list-style-type: none;
}

#rating {
  width: 165px;
  height: 33px;
  /*  border: 1px solid red;*/
}

.rating-item {
  width: 33px;
  height: 33px;
  float: left;
  background: url(../assets/img/StarRating.png) no-repeat;
  cursor: pointer;
}
</style>
