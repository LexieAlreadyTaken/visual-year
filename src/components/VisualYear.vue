<template>
  <div class="yearcont">
    <div class="monthcont" v-for="i in 12" :key="'month'+i">
      <div class="weekcont" v-for="j in 6" v-show="j<=monthWeeks(i)" :key="'week'+j">
        {{weekTotal(i,j)}}
        <div class="daycont" v-for="k in 7" :key="'day'+k"
          :class="(canShow(i,j,k)===true)?'':'wasted'"
          :style="'background-color:#'+colorstr(yearSche?yearSche[i]?yearSche[i][j]?yearSche[i][j][k]:[]:[]:[])">
          {{(j-1)*7-monthStartVar(i)+k+1}}
        </div>
      </div>
      <div class="monthLabel">
        {{i}}月
      </div>
    </div>
    <div class="undercont">
      <pre class="underleft">
一段可能的示例JSON（表示2021年12月11日和12日的日程）：
（请复制粘贴到本地，自由调整之后，复制粘贴到右边的框里）
{
  "year": "2021",
  "yearSchedule": [
    [],[],[],[],[],[],[],[],[],[],[],[
      [],[
        [],[],[],[],[],"ff0000","4c8033"
      ],[],[],[]
    ]
  ]
}
      </pre>
      <input type="textarea" class="textup" v-model="schedule"/>
    </div>
  </div>
</template>

<script>
export default {
  name: 'VisualYear',
  data(){
    return {
      base: 2000,
      baseStart: 6,
      months: [31, 28, 31, 30, 31, 30, 31, 31, 30, 31, 30, 31],
      schedule: '{   "year": "2021",   "yearSchedule": [     [],[],[],[],[],[],[],[],[],[],[],[       [],[         [],[],[],[],[],"ff0000","4c8033"       ],[],[],[]     ]   ] }'
    }
  },
  computed: {
    yearStart(){
      var s = this.baseStart
      for(var i = this.base; i < this.year; i++){
        if(this.leap(i)){
          s += 2
        }
        else{
          s += 1
        }
      }
      return s % 7
    },
    scheduleParsed(){
      return JSON.parse(this.schedule)
    },
    year(){
      return Number(this.scheduleParsed.year)
    },
    yearSche(){
      return this.scheduleParsed.yearSchedule
    }
  },
  methods:{
    leap(y){
      return (y % 4 == 0) && (y % 100 != 0) || (y % 400 == 0)
    },
    monthLength(m){
      if(m==2 && this.leap(this.year)){
        return 29
      }
      return this.months[m-1]
    },
    monthStart(m){
      var s = this.yearStart
      for(var i = 1; i<m; i++){
        s += this.monthLength(i)
      }
      return s % 7
    },
    monthStartVar(m){
      var a = this.monthStart(m)
      if(a==0){
        a=7
      }
      return a
    },
    monthWeeks(m){
      var a = this.monthStartVar(m)
      var res = 4
      if(a!=1){
        res++
      }
      return res + Number((this.monthLength(m)+a-res*7-1)>0)
    },
    weekDays(m,w){
      if(w<=4){
        return 7
      }
      if(this.monthWeeks(m)==6 && w == 5){
        return 7
      }
      var a = (this.monthLength(m)+this.monthStartVar(m)-8)%7
      return (a==0)?7:a
    },
    hasStarted(m, w, d){
      if(w>1){
        return true
      }
      else{
        return d >= this.monthStartVar(m)
      }
    },
    notEnded(m,w,d){
      if(w<=4 || w<this.monthWeeks(m)){
        return true
      }
      return d <= this.weekDays(m,w)
    },
    canShow(i,j,k){
      return this.hasStarted(i,j,k) && this.notEnded(i,j,k)
    },
    weekTotal(m,w){
      var week = 0
      for(var i = 1; i<m; i++){
        week += this.monthWeeks(i)
        if(this.monthStart(i)){
          week--
        }
      }
      return week + w
    },
    hexANum(x){
      if(x<10){
        return String.fromCharCode("0".charCodeAt(0)+x)
      }
      else{
        return String.fromCharCode("a".charCodeAt(0)+x-10)
      }
    },
    hexify(x){
      return this.hexANum(Math.floor(x*2.55)/16)+this.hexANum(Math.floor(x*2.55)%16)
    },
    colorstr(a){
      if(!a || a.length==0)
        return "eeeeee"
      return a
      /*console.log(this.hexify(a[0])+this.hexify(a[1])+this.hexify(a[2]))
      return this.hexify(a[0])+this.hexify(a[1])+this.hexify(a[2])*/
    }
  },
  mounted(){
    /*var schedule = require("../../public/example.json")
    this.year = Number(schedule.year)
    this.yearSche = schedule.yearSchedule*/
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
.monthcont{
  display: inline-block;
  margin: 0.5em;
}
.weekcont{
  display: inline-block;
  margin: 0.5em 0;
  color: #999999;
}
.daycont{
  display: block;
  width: 1em;
  height: 1em;
  background-color: #c0c0c0;
  border-radius: 0.2em;
  margin: 0.2em;
  color: white;
}
.daycont.wasted{
  background-color: transparent;
  color: transparent;
}
.textup{
  width: 500px;
  height: 300px;
  white-space:pre-wrap;
  white-space:-moz-pre-wrap;
  white-space:-o-pre-wrap;
  word-wrap:break-word;
}
.underleft{
  display: inline-block;
}
</style>
