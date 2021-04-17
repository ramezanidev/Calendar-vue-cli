<template>
    <div class="calendar">
        <header>
            <div>
                <div>
                    <input type="text" placeholder="Enter Date">
                    <img @click="thisMonth--" src="../assets/img/svg/arrow.svg" />
                    <span @click="setDate" @wheel="($event.deltaY / -100 == 1) ? thisMonth++ : thisMonth--">{{title}}</span>
                    <img @click="thisMonth--" src="../assets/img/svg/arrow.svg" />
                </div>
                <div>
                    <button @click="setDate"><img src="../assets/img/calendar/toDey-calendar.png">ToDey</button>
                </div>
            </div>
            <div>
                <button><img src="../assets/img/calendar/week-calendar.png">Week</button>
                <button><img src="../assets/img/calendar/month-calendar.png">Month</button>
                <button><img src="../assets/img/calendar/year-calendar.png">Year</button>
            </div>
        </header>
        <section @wheel="($event.deltaY / -100 == 1) ? thisMonth++ : thisMonth--">
            <section>
                <span v-for="i in 7" :key="i" v-text="computedDate(i,true,-1)" :title="computedFullDate(computedDate(i,true,-1))"></span>
                <span v-for="i in (getTotalDey(-1))" :key="i" v-text="computedDate(i,false,-1)" :title="computedFullDate(computedDate(i,false,-1))"></span>
            </section>

            <section>
                <span v-for="i in 7" :key="i" v-text="computedDate(i,true)" :title="computedFullDate(computedDate(i,true))"></span>
                <span v-for="i in (getTotalDey(0))" :key="i" v-text="computedDate(i,false)" :title="computedFullDate(computedDate(i,false))"></span>
            </section>

            <section>
                <span v-for="i in 7" :key="i" v-text="computedDate(i,true,1)" :title="computedFullDate(computedDate(i,true,1))"></span>
                <span v-for="i in (getTotalDey(1))" :key="i" v-text="computedDate(i,false,1)" :title="computedFullDate(computedDate(i,false,1))"></span>
            </section>
        </section>
    </div>
</template>

<script>
// import components
// import tableMenu from './tableMenu';

export default {
    name:"calendar",
     data(){
      return{
          thisYear:0,
          thisMonth:0,
          thisDate:0,
          thisDay:0,
          title:'',
          createDate:0,
      }
  },
  watch:{
      thisMonth(){
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
      getTotalDey(e = 0){
          return (new Date(this.thisYear,this.thisMonth + e + 1, 0).getDate() - (7 - new Date(this.thisYear,this.thisMonth + e).getDay()))
      },
      computedDate(e,status,num = 0){
          if(status){
              let now = new Date(this.thisYear,this.thisMonth + num, e - new Date(this.thisYear,this.thisMonth + num).getDay()).getDate()
              return (new Date(this.thisYear,this.thisMonth + num).getDay() < e) ? now : null
          }else{
              let now = new Date(this.thisYear,this.thisMonth + num, e - (new Date(this.thisYear,this.thisMonth + num).getDay() - 7)).getDate()
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
        let now = new Date(this.thisYear,this.thisMonth);
        this.title = new Intl.DateTimeFormat('en-US', {month:'long',year:"numeric"}).format(now);
      },
  },
  mounted(){
    this.setDate()
    this.changeDate()
  }
};
</script>

<style lang="scss" scoped>
.calendar{
  height: 100%;
  display: flex;
  flex: auto;
  background-color: turquoise;
  header{
      width: 100%;
      display: flex;
      height: 80px;
      background-color: #222222;
      padding: 20px 0;
      box-sizing: border-box;
      >div:nth-child(1){
        flex: auto;
        justify-content: space-between;
        border-right: 1px solid rgb(90, 90, 90);
        div{
             align-items: center;
             display: flex;
        }
        input{
            outline: none;
            font-size: 16px;
            padding: 6px 8px;
            margin: 0 10px;
            background-color: #2f2f2f;
            color: #ddd;
            border: none;
            width: 190px;
        }
        span{
            color: #ccc;
            font-size: 14px;
        }
        img{
            width: 20px;
            cursor: pointer;
            margin: 0 3px;
            &:hover {
              opacity: 0.7;
            }
            &:nth-of-type(2) {
              width: 20px;
              transform: rotate(180deg);
            }
        }
      }
      >div{
        display: flex;
        align-items: center;
        button{
            background-color: transparent;
            border: none;
            outline: none;
            display: flex;
            align-items: center;
            color: #eee;
            margin: 0 3px;
            cursor: pointer;
            font-size: 14px;
            padding: 6px;
            &:hover{
                background-color: rgba(255, 255, 255, 0.13);
            }
            >img{
               width: 22px;
               margin-right: 6px;
            }
        }
      }
  }
}
</style>
