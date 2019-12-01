<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped lang="less">
h1{
  text-align: center;
  font-size:30px;
}
.condition_wrap{
  h5{
    margin:10px;
  }
  ul{
    margin:10px;
    display: flex;
    width:500px;
    flex-wrap: wrap;
    li{
        width:250px;
        display: flex;
        height:30px;
        align-items:center;
        margin:10px 0;
        h6{
          margin:5px;
          width:80px;
        }
        input{
          height:20px;
          padding:2px;
        }
    }
  }
}

.show_wrap{
  display: flex;
  .show_inner{
    margin:10px;
    width:300px;
    padding:10px;
    border:1px solid green;
    dt{
      margin:5px 0;
    }
    dd{
      margin:10px;
    }
  }
}
</style>

<template>
  <div class="hello">
    <h1>{{ msg }}</h1>
    <div class="condition_wrap">
     <h5>相关变量</h5>
     <ul>
       <li>
         <h6>本金</h6>
         <input type="text" :value="PV">
       </li>
       <li>
         <h6>年期金</h6>
         <input type="text" :value="PMT">
       </li>
       <li>
         <h6>年复利</h6>
         <input type="text" :value="RATE">
       </li>
       <li>
         <h6>年通货膨胀</h6>
         <input type="text" :value="CPI">
       </li>
       <li>
         <h6>年消费</h6>
         <input type="text" :value="yearCost">
       </li>
    </ul> 
    </div>
    <h5>相关演示</h5>
    <div class="show_wrap">
      <!-- 年支出 -->
      <dl class="show_inner">
        <dt>
          年支出
        </dt>
        <dd>
          一共花费{{sumCost/10000}} w
        </dd>
        <dd v-for="(item,index) in list" :key="index">
          {{item.year}} 岁，花 {{item.cost/10000}} w
        </dd>
      </dl>
      <!-- 年结余 -->
      <dl class="show_inner">
        <dt>
          年资产
        </dt>
        <dd>
          <!-- 一共有{{assets/10000}} w -->
        </dd>
        <dd v-for="(item,index) in list" :key="index">
          {{item.year}} 岁，有 {{item.money/10000}} w
        </dd>
      </dl>
    </div>
    
  </div>
</template>

 <script>
export default {
  name: 'HelloWorld',
  props: {
    
  },
  data(){
    return {
      msg: '单身财务模型',
      PV: 80000, //本金
      PMT: 100000, // 期金
      RATE: .06, // 理财年化收益
      NPER: 30, // 时间
      FV:0, // 终值
      CPI: .04, // 通货膨胀
      yearCost: 120000,  // 年消费
      sumCost: 0,      // 总花费
      assets: 0,    // 资产
      list: [
        // {
        //   year:28,
        //   money:80000,
        //   cost: 120000
        // }
      ]
    }
  },
  created(){
    this.init()

  },
  methods: {
    init(){
      for(let i=28; i < 86; i += 1){
        const cur = {

        }
        cur.year = i;
        cur.cost = Math.ceil(120000 * (Math.pow((1 + this.CPI),i-28)));

        
        this.sumCost += cur.cost;




        if( i === 28){
         cur.money = this.PV;
          // leftMoney = money;
        } else if(i < 61){
          cur.money = Math.ceil(this.list[i-29].money * (1 + this.RATE) + this.PMT);
          // leftMoney = money;
        } else{
          // console.log()
          cur.money = Math.ceil(this.list[i-29].money - cur.cost ) * (1 + this.RATE) ;
          // money = Math.ceil((money-cost)*1.06);
          // leftMoney = money;
          
        }

        // this.assets += cur.money;




        this.list.push(cur)

        // if( i === 29){
        //   money = PV*(1+RATE) + PMT;
        //   leftMoney = money;
        // } else if(i < 60){
        //   money = Math.ceil(money*(1+RATE) + PMT);
        //   leftMoney = money;
        // } else {
        //   money = Math.ceil((money-cost)*1.06);
        //   // leftMoney = money;  
        // }


        // sumCost += cost;
        // leftMoney = money - cost;
        // leftMoneySum += leftMoney;

        // console.log(`${year}岁有 ${money / 10000}万元 花 ${cost/10000}万元  剩余资产 ${leftMoney / 10000}万元`);

        // console.log(`${year}岁 有 ${money} 花 ${cost} 当年结余 ${leftMoney} 累计结余 ${leftMoneySum}`)
        // console.log(`${i}岁 花 ${cost}元`)
        // console.log(`${year}岁 有 ${money} `)
      }
    }
  }
}
</script>

