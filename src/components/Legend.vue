<template>
  <div class="legendcont">
    <div class="label">图例</div>
    <div class="innercont">
      小方格颜色的r代表玩的程度，b代表学习的程度，g代表做其他事情的程度（0~255）。
      <div><div class="daycont" style="background-color:#ff0000">1</div>完全在玩的一天</div>
      <div><div class="daycont" style="background-color:#0000ff">2</div>完全在学习的一天</div>
      <div><div class="daycont" style="background-color:#00ff00">3</div>完全在做其他事情的一天</div>
      <div><div class="daycont" style="background-color:#555555">4</div>完全均衡的一天</div>
    </div>
  </div>
</template>

<script>
export default {
  name: 'Legend',
  data(){
    return {
      base: 2000,
      baseStart: 6,
      months: [31, 28, 31, 30, 31, 30, 31, 31, 30, 31, 30, 31]
    }
  },
  computed: {
  },
  methods:{
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
.legendcont{
  display: inline-block;
}
.label{
  font-size: 2em;
}
.innercont{
  margin: 0.5em;
}
.daycont{
  display: inline-block;
  width: 1em;
  height: 1em;
  background-color: silver;
  border-radius: 0.2em;
  margin: 0.2em;
  color: white;
}
</style>
