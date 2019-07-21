<template>
  <div class="container">
    <div class="layout">
      <div class="statusBar">
        <template v-for="(navitem, i) in navData">
          <div v-if="i!=2"
            class="statusBar__item statusBar--singleCol flex--col"
            :class="navitem.class"
            :key="i"
          >
            <div class="statusBar__title">
              <i
                v-if="navitem.icon"
                class="fas"
                :class="[navitem.icon]"
                style="font-size:20px; margin-right:5px;"
              ></i>
              {{ navitem.value }}
            </div>
            <div class="statusBar__subTitle">
              {{ navitem.desc }}
            </div>
          </div>
          <div v-else
            class="statusBar__item statusBar--doubleCol text--light flex--row"
            :class="navitem[0].class"
            :key="i"
          >
            <div class="barItem__content">
              <div class="statusBar__title">
                {{ navitem[0].value }}
              </div>
              <div class="statusB ar__subTitle">
                {{ navitem[0].desc }}
              </div>
            </div>
            <div class="barItem__content">
              <percent-donut/>
            </div>
          </div>
        </template>
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
              <doughnut-chart
                class="chart__content"
                :chartData="chartsData"
                :options="chartOption"
              />
            </div>
          </div>
          <div class="mainBody__item">
            <div class="table__container">
              <div class="table__search">
                <div class="searchBar" :class="{'searchBar--focus': focusSearch}">
                  <i class="fas fa-search" :class="{'text--green': focusSearch}"></i>
                  <input
                    class="searchBar__input"
                    type="text"
                    placeholder="Search"
                    v-model="searchInput"
                    @focus="focusSearch = true"
                    @blur="focusSearch = false"
                  >
                </div>
              </div>
              <div class="table__content">
                <table>
                  <thead>
                    <tr class="table__header">
                      <td class="col--lit"></td>
                      <td class="col--name" @click="sort('name')">Name</td>
                      <td class="col--score" @click="sort('score')">Score</td>
                    </tr>
                  </thead>
                  <tbody>
                    <tr v-for="(data, i) in selectedTableData" :key="i">
                      <td
                        style="text-align:center; font-size:9px;"
                        :style="{color:'red'}"
                      ><i class="fas fa-circle"></i></td>
                      <td>{{ data.name }}</td>
                      <td>{{ data.score }}</td>
                    </tr>
                  </tbody>
                </table>
              </div>
              <div class="table__pager">
                <div class="pager text--light">
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
import DoughnutChart from '../components/donutChart.vue';
import PercentDonut from '../components/percentDonut.vue';

export default {
  components: {
    DoughnutChart,
    PercentDonut,
  },
  data: () => ({
    searchInput: '',
    currentPage: 1,
    focusSearch: false,
    currentSort: '',
    currentSortDir: 'asc',
    navData: [
      {
        icon: ['fa-arrow-up', 'text--green'],
        value: '59',
        desc: 'Elements',
        class: ['statusBar--whiteBg', 'text--dark'],
      },
      {
        icon: ['fa-arrow-down', 'text--red'],
        value: '12',
        desc: 'Versions',
        class: ['statusBar--whiteBg', 'text--dark'],
      },
      [
        {
          icon: '',
          value: '425',
          desc: 'Comits',
          class: ['statusBar--redBg', 'text--light'],
        },
        {},
      ],
      {
        icon: 'fa-user-friends',
        value: '5',
        desc: 'Team Member',
        class: ['statusBar--blueBg', 'text--light'],
      },
    ],
    tableData: [
      { name: 'Noelia O\'Kon', score: 13098.00 },
      { name: 'Mr. Enid Von PhD', score: 35978.00 },
      { name: 'Colton Koch', score: 26278.00 },
      { name: 'Gregory Vandervort', score: 25537.00 },
      { name: 'Miss Rahsaan Heaney IV', score: 49003.00 },
      { name: 'Ruby', score: 42003.00 },
      { name: 'Santoso', score: 13098.00 },
      { name: 'Alexander', score: 15998.00 },
      { name: 'Junifar Adam', score: 33468.00 },
      { name: 'Sutan Rahmat', score: 12008.00 },
      { name: 'Malik Yuniar', score: 10008.00 },
      { name: 'Cuaki', score: 42003.00 },
      { name: 'Bakso', score: 13098.00 },
      { name: 'Batagor', score: 15998.00 },
      { name: 'Somay Adam', score: 33468.00 },
    ],
    chartsData: {
      labels: [
        'North America',
        'South America',
        'Australia',
      ],
      datasets: [
        {
          data: [30, 60, 15],
          backgroundColor: [
            '#00A5FF',
            '#FFB056',
            '#00FF72',
          ],
          borderWidth: 1,
        },
      ],
    },
    chartOption: {
      responsive: true,
      maintainAspectRatio: false,
      legend: {
        position: 'bottom',
        display: true,
        labels: {
          usePointStyle: true,
        },
      },
    },
  }),
  methods: {
    sort(s) {
      if (s === this.currentSort) {
        this.currentSortDir = this.currentSortDir === 'asc' ? 'desc' : 'asc';
      }
      this.currentSort = s;
    },
    movePage(i) {
      let movedPage = this.currentPage + i;
      if (movedPage < 1) {
        movedPage = 1;
      } else if (movedPage > this.countPager) {
        movedPage = this.countPager;
      }
      this.currentPage = movedPage;
    },
    checkPage(i) {
      return this.currentPage === i;
    },
    checkPageJump(i, forward) {
      let answer = false;
      if (i !== 0) {
        if (forward) {
          answer = this.countPager >= this.currentPage + i;
        } else {
          answer = this.currentPage - i >= 1;
        }
      }
      return answer;
    },
  },
  computed: {
    countPager() {
      const leftEl = (this.displayData.length % 5) !== 0 ? 1 : 0;
      return Math.floor(this.displayData.length / 5) + leftEl;
    },
    selectedTableData() {
      const lastDataIdx = this.currentPage * 5;
      return this.displayData.slice((this.currentPage - 1) * 5, lastDataIdx);
    },
    displayData() {
      const dataArr = [];
      const search = new RegExp(this.searchInput, 'ig');
      for (let i = 0; i < this.tableData.length; i += 1) {
        if (this.tableData[i].name.match(search)) {
          dataArr.push(this.tableData[i]);
        }
      }
      return dataArr.sort((a, b) => {
        let modifier = 1;
        if (this.currentSortDir === 'desc') modifier = -1;
        if (a[this.currentSort] < b[this.currentSort]) return -1 * modifier;
        if (a[this.currentSort] > b[this.currentSort]) return 1 * modifier;
        return 0;
      });
    },
    checkPagee(i) {
      return this.currentPage === i;
    },
  },
  watch: {
    searchInput() {
      this.currentPage = 1;
    },
  },
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
    flex-basis: 1%;
    display: flex;
    flex-wrap: wrap;
    flex-direction: row;
    justify-content: space-between;
    align-content: center;
    align-items: center;
  }
  .barItem__content{
    display: flex;
    flex-direction: column;
    width: 25%;
  }
  .statusBar__item{
    width : 300px;
    height: 90px;
    display: flex;
    background-color: grey;
    align-items: center;
  }
  .statusBar__title{
    display: flex;
    justify-content: center;
    align-items: center;
    font-size: 32px;
  }
  .statusBar__subTitle, table{
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
    flex-wrap: wrap;
    justify-content: space-between;
    align-items: center;
    height: 100%;
  }
  .mainBody__item{
    width: 44%;
    min-width: 250px;
    height: 500px;
    display: flex;
    flex-direction: column;
    justify-content: center;
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
    transition: .3s border ease-out;
  }
  .searchBar__input{
    height: 30px;
    border: 0;
    outline: none;
    margin-left: 10px;
    /* border-bottom: 1px solid black; */
  }
  .searchBar--focus{
    border-bottom: 1px solid #00DD63;
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
  .text--dark{
    color: black;
  }
  .text--light{
    color: azure;
  }
  .text--red{
    color: #FF1C49
  }
  .text--green{
    transition: .3s color ease-out;
    color: #00DD63;
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
