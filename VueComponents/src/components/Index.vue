<template>
  <ul>
    <li>
      <h3>星级评分</h3>
      <!-- 第一种方法 -->
      <div class="paper">
        <StarRating :lightOnNum.sync="num" :titles="titles" :mode="starMode1" :lockMode="false" @select="select"></StarRating>
        <div style="text-align: left;margin:5px;">
          评分：<span v-text="num"></span>
          <br> 模式(lightEntire/lightHalf)：
          <input type="input" v-model="starMode1">
        </div>
      </div>
      <!-- 第二种方法 -->
      <div class="paper">
        <StarRating2 :lightOnNum.sync="num" :total="8" :lockMode="false" @select="select" :mode="starMode2">
        </StarRating2>
        <div style="text-align: left;margin:5px;">
          评分：<span v-text="num"></span>
          <br> 模式：
          <input type="radio" v-model="starMode2" name="starMode2" value="entire">完整
          <input type="radio" v-model="starMode2" name="starMode2" value="half">半星
          <input type="radio" v-model="starMode2" name="starMode2" value="quarter">1/4星
        </div>
      </div>
    </li>
  </ul>
</template>
<script>
import StarRating from './StarRating';
import StarRating2 from './StarRating2';

export default {
  data() {
      return {
        // Star Component Options
        num: 4,
        titles: [
          "很不好",
          "不好",
          "一般",
          "好",
          "很好",
          "太好了",
          "完美",
          "简直完美到家了"
        ],
        starMode1: 'lightEntire',
          // lightEntire / lightHalf
        starMode2:'entire'
      }
    },
    methods: {
      // Star Component:Star clicked
      select: function(num) {
        // console.log('Index:select');
        // console.log(num);
      }
    },
    components: {
      StarRating,
      StarRating2
    },
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

.paper {
  transition: all 0.45s cubic-bezier(0.23, 1, 0.32, 1);
  background-color: #fcfcfc;
  box-shadow: rgba(0, 0, 0, 0.117647) 0px 1px 6px, rgba(0, 0, 0, 0.117647) 0px 1px 4px;
  border-radius: 2px;
  margin: 5px 10px;
  padding: 5px;
  min-height: 50px;
}
</style>
