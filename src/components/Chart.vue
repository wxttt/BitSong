<template>
  <div id="c1"></div>
</template>

<script>
import G2 from '@antv/g2';

export default {
  data() {
    return {
      max: 30,
    };
  },
  props: {
    data: {
      type: Array,
    },
  },
  computed: {
    formattedData() {
      let data = [...this.data];
      if (data.length > this.max) {
        data = data.slice(data.length - this.max);
      }
      return data.map((count, index) => ({
        index,
        count,
      }));
    },
  },
  watch: {
    formattedData() {
      this.chart.changeData(this.formattedData);
      this.chart.render();
    },
  },
  mounted() {
    const chart = this.chart = new G2.Chart({
      container: 'c1',
      width: 800,
      height: 600,
    });

    chart.source(this.formattedData);

    chart.line().position('index*count')
      .shape('smooth')
      .animate({
        update: {
          animation: 'delayScaleInY', // 动画名称
          easing: 'easeQuartIn', // 动画缓动效果
          // delay: 100, // 动画延迟执行时间
          duration: 600 // 动画执行时间
        },
      });
    
    chart.render();
  },
}
</script>

<style>
body {
  text-align: center;
}
#c1 {
  display: inline-block;
}
</style>
