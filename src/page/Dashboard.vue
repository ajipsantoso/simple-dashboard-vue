<template>
  <div class="container">
    <div class="layout">
      <div class="statusBar">
        <template v-for="(navitem, i) in navData">
          <div v-if="i!=2" class="statusBar__item statusBar--singleCol flex--col dark--text" :class="navitem.class" :key="i">
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
            <div class="chart__content">
              <line-chart/>
            </div>
          </div>
          <div class="mainBody__item">
            <div class="table__search">
              <input type="search" name="" id="" v-model="searchTable" @keyup="typing = !typing">
              <div v-if="typing">typing</div>
            </div>
            <div class="table__content">
              <table>
                <tr>
                  <th class="col--lit"></th>
                  <th class="col--name">Name</th>
                  <th class="col--score">Score</th>
                </tr>
                <tr v-for="(data, i) in selectedTableData" :key="i">
                  <td style="text-align:center;">1</td>
                  <td>{{ data.user }}</td>
                  <td>{{ data.score }}</td>
                </tr>
              </table>
            </div>
            <div class="table__pager">
              <div class="pager">
                <div class="pager__item" @click="currentPage = 1">
                  <i class="fas fa-angle-double-left"></i>
                </div>
                <div class="pager__item" @click="movePage(-1)">
                  <i class="fas fa-angle-left"></i>
                </div>
                <div class="pager__item" v-for="i in countPager" :key="i" @click="currentPage=i">
                  {{ i }}
                </div>
                <div class="pager__item" @click="movePage(1)">
                  <i class="fas fa-angle-right"></i>
                </div>
                <div class="pager__item" @click="currentPage = countPager">
                  <i class="fas fa-angle-double-right"></i>
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
import LineChart from '../components/chart.vue'
import PercentDonut from '../components/percentDonut'

export default {
  components:{
    LineChart,
    PercentDonut,
  },
  data: () => ({
    searchTable: '',
    currentPage: 1,
    typing: false,
    navData:[
      {icon: '', value: '59', desc: 'Elements', class: ['statusBar--whiteBg']},
      {icon: '', value: '12', desc: 'Versions', class: ['statusBar--whiteBg']},
      [{icon: '', value: '425', desc: 'Comits', class: ['statusBar--redBg']}, {} ],
      {icon: '', value: '5', desc: 'Team Member', class: ['statusBar--blueBg']},
    ],
    tableData:[
      { user: 'Noelia O\'Kon', score: 13098.00 },
      { user: 'Mr. Enid Von PhD', score: 13098.00 },
      { user: 'Colton Koch', score: 13098.00 },
      { user: 'Gregory Vandervort', score: 13098.00 },
      { user: 'Miss Rahsaan Heaney IV', score: 13098.00 },
      { user: '11111', score: 13098.00 },
      { user: '22222', score: 13098.00 },
      { user: '33333', score: 13098.00 },
      { user: '44444', score: 13098.00 },
      { user: '55555', score: 13098.00 },
      { user: '66666', score: 13098.00 },
    ],
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
    }
  },
  computed:{
    countPager(){
      let leftEl= (this.tableData.length % 5) != 0 ? 1 : 0;
      return Math.floor(this.tableData.length / 5) + leftEl;
    },
    selectedTableData(){
      let lastDataIdx = this.currentPage*5;
      return this.tableData.slice((this.currentPage-1)*5, lastDataIdx);
    },
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
    /* justify-content: space-around; */
    width: 25%;
  }
  .statusBar__item{
    width: 24%;
    height: 90px;
    display: flex;
    background-color: grey;
    align-items: center;
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
    width: 30%;
    border-bottom: 4px solid mediumspringgreen;
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
  }
  .statusbar__title{
    font-size: 32px;
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
  }
  .flex--row{
    display: flex;
    flex-direction: row;
  }
  .flex--col{
    display: flex;
    flex-direction: column;
  }
  .chart__content{
    width: 100%;
    height: 100%;
  }
  .pager__item{
    width: 35px;
  }
  .statusBar--whiteBg{
    background-color: white;
    box-shadow: inset 0px 6px 0px 0px mediumspringgreen;
  }
  .statusBar--redBg{
    background-color: #FF1C49;
  }
  .statusBar--blueBg{
    background-color: lightskyblue;
  }
  .dark--text{
    color: black;
  }
  .light--text{
    color: azure;
  }
  table{
    width: 100%;
    text-align: left;
  }
  td, th{
    border: 1px solid black;  
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
</style>
