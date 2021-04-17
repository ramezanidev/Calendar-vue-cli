<template>
  <div class="table">
    <header>
      <span @click="setDate">{{title}}</span>
      <div>
        <img @click="thisMonth++" src="../assets/img/svg/arrow.svg" />
        <img @click="thisMonth--" src="../assets/img/svg/arrow.svg" />
      </div>
    </header>
    <div class="lables">
      <span>Su</span>
      <span>Mo</span>
      <span>To</span>
      <span>We</span>
      <span>Th</span>
      <span>Fr</span>
      <span>Sa</span>
    </div>
    <div class="calendar" ref="calendar" @wheel="($event.deltaY / -100 == 1) ? thisMonth++ : thisMonth-- ">
        <span v-for="i in 7" :key="i" v-text="computedDate(i,true)" :title="computedFullDate(computedDate(i,true))"></span>
        <span v-for="i in (totalDay)" :key="i" v-text="computedDate(i,false)" :title="computedFullDate(computedDate(i,false))"></span>
    </div>
</div>
</template>

<script>
export default {
  name: "tableMenu",
  data(){
      return{
          thisYear:0,
          thisMonth:0,
          thisDate:0,
          thisDay:0,
          firstDay:0,
          totalDay:0,
          title:'',
          createDate:0,
      }
  },
  watch:{
      thisMonth(a,b){
          const el = this.$refs.calendar.classList;
          if(a>b){
               el.add("changeCalendar","changeNext");
               setTimeout(() => {
                    el.remove("changeCalendar","changeNext");
                    el.add("changePrev");
                    setTimeout(() => el.replace("changePrev","changeCalendar") , 20);
               }, 1000);
          }else if(a<b){
              el.add("changeCalendar","changePrev");
               setTimeout(() => {
                    el.remove("changeCalendar","changePrev");
                    el.add("changeNext");
                    setTimeout(() => el.replace("changeNext","changeCalendar") , 20);
               }, 1000);
          }
          this.changeDate()
      }
  },
  methods:{
      computedFullDate(e){
        const nowDate = new Date(this.thisYear,this.thisMonth, e);
        const options = {
            day:'numeric',
            month:'long',
            year:'numeric'
        };
        const fullDate = new Intl.DateTimeFormat('en-US', options).format(nowDate);
        if(new Intl.DateTimeFormat('en-US', options).format(new Date()) === fullDate){
            return "toDay"
        }else{
            return e && fullDate
        }
      },
      computedDate(e,status){
          if(status){
              let now = new Date(this.thisYear,this.thisMonth, e - this.firstDay).getDate()
              return (this.firstDay < e) ? now : null
          }else{
              let now = new Date(this.thisYear,this.thisMonth, e - (this.firstDay - 7)).getDate()
              return now
          }
      },
      setDate(){
          let now = new Date();
          this.thisYear = now.getFullYear();
          this.thisMonth = now.getMonth();
          this.thisDate = now.getDate();
          this.thisDay = now.getDay();
      },
      changeDate(){
        let now = new Date(this.thisYear,this.thisMonth,this.thisDate);
        this.title = new Intl.DateTimeFormat('en-US', {month:'long',year:"numeric"}).format(now);
        setTimeout(() => {
            this.firstDay = new Date(this.thisYear,this.thisMonth).getDay();
            this.totalDay = new Date(this.thisYear,this.thisMonth + 1, 0).getDate() - (7 - this.firstDay);

        }, 1000);
      },
  },
  mounted(){
    this.setDate()
    this.changeDate()
  }
};
</script>

<style lang="scss" scoped>
.table {
   .changeCalendar{
        transition: 1000ms;
   }
   .changeNext{
       transform: translate(100%,0) scale(0.4);
       opacity: 0 !important;
   }
   .changePrev{
       transform: translate(-100%,0) scale(0.4);
       opacity: 0 !important;
   }
  .calendar{
    flex: auto;
    display: flex;
    flex-wrap: wrap;
    opacity: 1;
    margin: 8px 4px;
    span{
        color: #fff;
        width: 32px;
        height: 32px;
        display: flex;
        align-items: center;
        justify-content: center;
        font-size: 15px;
        cursor: pointer;
        user-select: none;
        &:hover{
            background-color: #ffffff14;
        }
        &:active{
            background-color: #0000001c;
        }
    }
    span[title='toDay']{
        background-color: #1f1f1f;
        box-sizing: border-box;
        border: 3px double #828282;
        &:hover{
            background-color: transparent;
        }
        &:active{
            background-color: #ffffff0d;
        }
    }
}
}
header {
  height: 30px;
  display: flex;
  justify-content: space-between;
  padding: 4px 11px 0;
  span {
    color: #fff;
    font-size: 16px;
    margin: auto 0;
    cursor: pointer;
  }
  div {
    height: 100%;
    width: 50px;
    display: flex;
    justify-content: space-between;
    img {
      width: 20px;
      cursor: pointer;
      &:hover {
        opacity: 0.7;
      }
      &:nth-child(2) {
        width: 20px;
        transform: rotate(180deg);
      }
    }
  }
}
.lables {
  display: flex;
  color: #fff;
  font-size: 13px;
  padding: 6px 4px 1px;
  cursor: default;
  span {
        width: 32px;
    text-align: center;
  }
}
</style>