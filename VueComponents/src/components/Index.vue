<template>
  <ul>
    <li>
      <h3>星级评分</h3>
      <div class="paper">
        <StarRating :lightOnNum.sync="num" :titles="titles" :mode="mode" @select="select"></StarRating>
        <div style="text-align: left;margin:5px;">
          评分：<span v-text="num"></span>
          <br> 模式(lightEntire/lightHalf)：
          <input type="input" v-model="mode">
        </div>
      </div>
    </li>
  </ul>
</template>
<script>
import StarRating from './StarRating';

export default {
  data() {
      return {
        // Star Component Options
        num: 4.5,
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
        mode: 'lightEntire'
          // 
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
      StarRating
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
