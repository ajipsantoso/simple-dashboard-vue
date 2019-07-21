<template>
  <div class="container">
    <div class="layout">
      <div class="statusBar">
        <template v-for="(navitem, i) in navData">
          <div v-if="i!=2" class="statusBar__item statusBar--singleCol flex--col" :class="navitem.class" :key="i">
            <div class="statusbar__title">
              {{ navitem.value }}
            </div>
            <div class="statusbar__subTitle">
              {{ navitem.desc }}
            </div>
          </div>
          <div v-else class="statusBar__item statusBar--doubleCol light--text flex--row" :class="navitem[0].class" :key="i">
            <div class="barItem__content">
              <div class="statusbar__title">
                {{ navitem[0].value }}
              </div>
              <div class="statusbar__subTitle">
                {{ navitem[0].desc }}
              </div>
            </div>
            <div class="barItem__content">
              <percent-donut/>
            </div>
          </div>
        </template>
        <!-- <div class="statusBar__item statusBar--singleCol flex--col dark--text">
            <div class="statusbar__title">
              59
            </div>
            <div class="statusbar__subTitle">
              Elements
            </div>
        </div>
        <div class="statusBar__item statusBar--singleCol  flex--col dark--text">
            <div class="statusbar__title">
              12
            </div>
            <div class="statusbar__subTitle">
              Versions
            </div>
        </div>
        <div class="statusBar__item statusBar--doubleCol light--text flex--row">
          <div class="barItem__content">
            <div class="statusbar__title">
              425
            </div>
            <div class="statusbar__subTitle">
              Commits
            </div>
          </div>
          <div class="barItem__content">
            <percent-donut/>
          </div>
        </div>
        <div class="statusBar__item statusBar--singleCol  flex--col light--text">
            <div class="statusbar__title">
              5
            </div>
            <div class="statusbar__subTitle">
              Team Members
            </div>
        </div> -->
      </div>
      <div class="mainBody">
        <div class="mainBody__title">
          <div class="title--text">
            Data Visualization
          </div>
        </div>
        <div class="mainBody__content">
          <div class="mainBody__item">
            <div class="chart">
              <doughnut-chart class="chart__content" :chartData="chartsData" :options="chartOption"/>
            </div>
          </div>
          <div class="mainBody__item">
            <div class="table__container">
              <div class="table__search">
                <div class="searchBar">
                  <i class="fas fa-search"></i>
                  <input
                    class="searchBar__input"
                    type="text"
                    name=""
                    id=""
                    v-model="searchInput"
                    @keyup="typing = !typing"
                  >
                </div>
                <!-- <div v-if="typing">typing</div> -->
              </div>
              <div class="table__content">
                <table>
                  <thead>
                    <tr class="table__header">
                      <td class="col--lit"></td>
                      <td class="col--name">Name</td>
                      <td class="col--score">Score</td>
                    </tr>
                  </thead>
                  <tbody>
                    <tr v-for="(data, i) in selectedTableData" :key="i">
                      <td style="text-align:center;">1</td>
                      <td>{{ data.user }}</td>
                      <td>{{ data.score }}</td>
                    </tr>
                  </tbody>
                </table>
              </div>
              <div class="table__pager">
                <div class="pager light--text">
                  <div
                    class="pager__item pager__item--leftRound"
                    :class="{'pager__item--selectable': checkPageJump(currentPage-1,false)}"
                    @click="currentPage = 1"
                  >
                    <i class="fas fa-angle-double-left"></i>
                  </div>
                  <div
                    class="pager__item"
                    :class="{'pager__item--selectable': checkPageJump(1,false)}"
                    @click="movePage(-1)"
                  >
                    <i class="fas fa-angle-left"></i>
                  </div>
                  <div
                    class="pager__item pager__item--selectable"
                    :class="{'pager__item--selected': checkPage(i)}"
                    v-for="i in countPager"
                    :key="i"
                    @click="currentPage=i"
                  >
                    {{ i }}
                  </div>
                  <div
                    class="pager__item pager__item"
                    :class="{'pager__item--selectable': checkPageJump(1,true)}"
                    @click="movePage(1)"
                  >
                    <i class="fas fa-angle-right"></i>
                  </div>
                  <div
                    class="pager__item pager__item--rightRound"
                    :class="{'pager__item--selectable': checkPageJump(countPager-currentPage,true)}"
                    @click="currentPage = countPager"
                  >
                    <i class="fas fa-angle-double-right"></i>
                  </div>
                </div>
              </div>
            </div>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>
<script>
import DoughnutChart from '../components/chart.vue'
import PercentDonut from '../components/percentDonut'

export default {
  components:{
    DoughnutChart,
    PercentDonut,
  },
  data: () => ({
    searchInput: '',
    currentPage: 1,
    typing: false,
    navData:[
      {icon: '', value: '59', desc: 'Elements', class: ['statusBar--whiteBg', 'dark--text']},
      {icon: '', value: '12', desc: 'Versions', class: ['statusBar--whiteBg', 'dark--text']},
      [{icon: '', value: '425', desc: 'Comits', class: ['statusBar--redBg', 'light--text']}, {} ],
      {icon: '', value: '5', desc: 'Team Member', class: ['statusBar--blueBg', 'light--text']},
    ],
    tableData:[
      { user: 'Noelia O\'Kon', score: 13098.00 },
      { user: 'Mr. Enid Von PhD', score: 35978.00 },
      { user: 'Colton Koch', score: 26278.00 },
      { user: 'Gregory Vandervort', score: 25537.00 },
      { user: 'Miss Rahsaan Heaney IV', score: 49003.00 },
      { user: 'Ruby', score: 42003.00 },
      { user: 'Santoso', score: 13098.00 },
      { user: 'Alexander', score: 15998.00 },
      { user: 'Junifar Adam', score: 33468.00 },
      { user: 'Sutan Rahmat', score: 12008.00 },
      { user: 'Malik Yuniar', score: 10008.00 },
      { user: 'Cuaki', score: 42003.00 },
      { user: 'Bakso', score: 13098.00 },
      { user: 'Batagor', score: 15998.00 },
      { user: 'Somay Adam', score: 33468.00 },
    ],
    chartsData:{
        labels: [
          "North America",
          "South America",
          "Australia",
        ],
        datasets: [
         {
           data: [30, 60, 15],
           backgroundColor: [
                '#00A5FF',
                '#FFB056',
                '#00FF72',
            ],
            borderWidth: 1
         }
        ],
      },
    chartOption:{
      responsive: true,
      maintainAspectRatio: false,
      legend: {
          position: 'bottom',
          display: true,
          labels: {
              usePointStyle: true,
          },
        },
      }
  }),
  methods: {
    check(i) {
      alert(i);
    },
    movePage(i) {
      let movedPage = this.currentPage + i;
      if ( movedPage < 1 ) {
        movedPage = 1;
      } else if (movedPage > this.countPager) {
        movedPage = this.countPager
      }
      this.currentPage = movedPage;
    },
    checkPage(i) {
      return this.currentPage == i;
    },
    checkPageJump(i,forward){
      if (i!=0){
        if (forward) {
          return this.countPager >= this.currentPage+i;
        }else{
          return 1 <= this.currentPage-i;
        }
      }
    }
  },
  computed:{
    countPager(){
      let leftEl= (this.displayData.length % 5) != 0 ? 1 : 0;
      return Math.floor(this.displayData.length / 5) + leftEl;
    },
    selectedTableData(){
      let lastDataIdx = this.currentPage*5;
      return this.displayData.slice((this.currentPage-1)*5, lastDataIdx);
    },
    displayData(){
      this.currentPage = 1;
      let dataArr = [];
      let search = new RegExp(this.searchInput, 'ig');
      for (var i = 0; i < this.tableData.length; i++) {
        if (this.tableData[i].user.match(search)){
          dataArr.push(this.tableData[i])
        }
      }
      return dataArr;
    },
    checkPagee(i) {
      return this.currentPage == i;
    }
  }
};
</script>
<style scoped>
  .container{
    display: flex;
    justify-content: center;
    align-items: center;
  }
  .layout{
    display: flex;
    flex-direction: column;
    width: 100%;
    margin: 0 100px;
  }
  .statusBar{
    display: flex;
    flex-direction: row;
    justify-content: space-between;
    align-items: center;
  }
  .barItem__content{
    display: flex;
    flex-direction: column;
    width: 25%;
  }
  .statusBar__item{
    width: 24%;
    height: 90px;
    display: flex;
    background-color: grey;
    align-items: center;
  }
  .statusbar__subTitle, table{
    font-size: 14px;
  }
  .statusBar--doubleCol{
    flex-direction: row;
    justify-content: space-around;
  }
  .statusBar--singleCol{
    flex-direction: row;
    justify-content: center;
  }
  .mainBody{
    display: flex;
    flex-direction: column;
    margin: 20px 0 0;
    padding: 50px 20px 20px;
    background-color: white;
  }
  .mainBody__title{
    margin-left: 5%;
    width: 26%;
    border-bottom: 4px solid #00DD63;
  }
  .mainBody__content{
    display: flex;
    flex-direction: row;
    justify-content: space-between;
    align-items: center;
    height: 500px;
  }
  .mainBody__item{
    width: 44%;
    height: 100%;
    display: flex;
    flex-direction: column;
    justify-content: center;
    align-items: center;
  }
  .statusbar__title{
    font-size: 32px;
  }

  .flex--row{
    display: flex;
    flex-direction: row;
  }
  .flex--col{
    display: flex;
    flex-direction: column;
  }
  .chart{
    width: 100%;
    display: flex;
    justify-content: center;
    align-items: center;
  }
  .chart__content{
    width: 100%;
    height: 100%;
    position: relative;
  }
  table {
    width: 100%;
    height: 100%;
    text-align: left;
    border-collapse: collapse;
    font-weight: bold;
  }
  .table__content{
    width: 100%;
    margin: 2% 0px;
    min-height: 212px;
  }
  .table__pager{
    height: 8%;
    width: 100%;
    display: flex;
    justify-content: center;
  }
  .table__search {
    height: 10%;
    width: 100%;
    display: flex;
    justify-content: flex-start;
    align-items: center;
  }
  .table__container{
    width: 100%;
    height: 100%;
    display: flex;
    flex-direction: column;
    justify-content: center;
    align-items: center;
  }
  .table__pager{
    display: flex;
    flex-direction: row;
    justify-content: center;
  }
  .pager{
    display: flex;
    flex-direction: row;
    align-items: center;
    border-radius: 100px;
    background-color: rgba(0, 221, 99,0.6);
  }
  .searchBar{
    border-bottom: 1px solid black;
  }
  .searchBar__input{
    height: 30px;
    border: 0;
    outline: none;
    margin-left: 10px;
    /* border-bottom: 1px solid black; */
  }
  tr{
    height: 35px;
  }
  thead td{
    color: #00DD63;
  }
  tbody tr:nth-child(odd){
    background-color: white;
  }
  tbody tr:nth-child(even){
    background-color: rgba(126, 163, 183, 0.4);
  }
  tr.table__header td{
    border-bottom: 2px solid black;  
  } 
  .pager__item{
    width: 35px;
    height: 100%;
    display: flex;
    align-items: center;
    justify-content: center;
  }
  .statusBar--whiteBg{
    background-color: white;
    box-shadow: inset 0px 6px 0px 0px #00DD63;
  }
  .statusBar--redBg{
    background-color: #FF1C49;
  }
  .statusBar--blueBg{
    background-color: #00A5FF;
  }
  .dark--text{
    color: black;
  }
  .light--text{
    color: azure;
  }
  .title--text{
    font-size: 32px;
  }
  .col--name{
    width: 65%;
  }
  .col--score{
    width: 25%;
  }
  .col--lit{
    width: 10%;
  }
  .pager__item--leftRound{
    border-radius: 100px 0 0 100px;
  }
  .pager__item--rightRound{
    border-radius: 0 100px 100px 0;
  }
  .pager__item--selectable{
    background: #00DD63;
  }
  .pager__item--selected{
    background: #0EB75A;
  }
</style>
