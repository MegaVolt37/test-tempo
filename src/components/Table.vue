<template>
  <div class="table__wrapper">
    <div class="sorting__mobile">
      <Select v-if="widthWindow <= 768" :list="listTableHead" @selectItem="sortingClickedMobile" />
      <ButtonsSorting @sorting="sortingClicked" />
    </div>
    <table class="table" v-show="readSearchName.length">
      <thead class="table__head" v-for="index in readMobileHead" :key="index">
        <th class="table__head-item" v-for="(item, index) in listTableHead" :key="item.title"
          @click="sortingClickedFull(item.id, index)">{{
            item.title
          }}
          <IconArrow @click="sortingClickedFull(item.id, index)" :style="styleArrow(item.id, index)" />
        </th>
      </thead>
      <tbody class="table__body">
        <td class="table__body-item" v-for="item in readSearchName" :key="item.id">
          <tr class="table__body-item__list">
            <td>{{ item.name }}</td>
            <td>{{ item.date }}</td>
            <td class="item__list-score" v-for="(subject, index) in item.subjects" :key="index"
              :style="styleColorScore(subject.score)">{{
                subject.score }}</td>
            <td class="item__list-score" :style="styleColorSum(percent(sum(item)))">{{ sum(item) }}</td>
            <td class="item__list-percent">
              <CircleProgress v-if="widthWindow > 768" :percent="percent(sum(item), item)" />
              <LineProgress v-else :percent="percent(sum(item), item)" />
            </td>
          </tr>
        </td>
      </tbody>
    </table>
    <p class="not__results" v-show="!readSearchName.length">Результатов не найдено</p>
  </div>
</template>

<script>
import LineProgress from "@/components/LineProgress.vue";
import CircleProgress from "@/components/CircleProgress.vue";
import Select from "@/components/Select.vue";
import ButtonsSorting from "@/components/ButtonsSorting.vue";

import IconArrow from "@/components/icons/Arrow.vue";

import axios from "axios";

import 'overlayscrollbars/overlayscrollbars.css';
import {
  OverlayScrollbars,
} from 'overlayscrollbars';

const sortFunctions = {
  'name': (a, b) => a.name.localeCompare(b.name),
  '-name': (a, b) => b.name.localeCompare(a.name),
  'date': (a, b) => new Date(b.date) - new Date(a.date),
  '-date': (a, b) => new Date(a.date) - new Date(b.date),
  'russian': (a, b) => b.subjects[0].score - a.subjects[0].score,
  '-russian': (a, b) => a.subjects[0].score - b.subjects[0].score,
  'mathematics': (a, b) => b.subjects[1].score - a.subjects[1].score,
  '-mathematics': (a, b) => a.subjects[1].score - b.subjects[1].score,
  'informatics': (a, b) => b.subjects[2].score - a.subjects[2].score,
  '-informatics': (a, b) => a.subjects[2].score - b.subjects[2].score,
  'sum': (a, b) => b.sum - a.sum,
  '-sum': (a, b) => a.sum - b.sum,
  'percent': (a, b) => b.percent - a.percent,
  '-percent': (a, b) => a.percent - b.percent,
};
export default {
  data() {
    return {
      listTableHead: [
        {
          id: 'name',
          title: 'ФИО',
        },
        {
          id: 'date',
          title: 'Дата подачи заявления',
        },
        {
          id: 'russian',
          title: 'Балл по русскому',
        },
        {
          id: 'mathematics',
          title: 'Балл по математике',
        },
        {
          id: 'informatics',
          title: 'Балл по информатике',
        },
        {
          id: 'sum',
          title: 'Суммарный балл',
        },
        {
          id: 'percent',
          title: 'Процент',
        },
      ],
      listTableBody: [],
      osInstance: null,
      widthWindow: window.innerWidth,
      activeIndex: null,
    };
  },
  computed: {
    readSearchName() {
      const searchName = this.$route.query.search;
      return searchName ? this.listTableBody.filter(item =>
        item.name.toLowerCase().includes(searchName.toLowerCase())
      ) : this.listTableBody;
    },
    readMobileHead() {
      if (this.widthWindow <= 768) {
        return this.readSearchName.length
      } else {
        return 1
      }
    },
  },
  methods: {
    sortingClickedMobile(id, idx) {
      this.activeIndex = idx;
      const sortingType = sortFunctions[id];
      if (sortingType) {
        this.listTableBody = this.listTableBody.sort(sortingType);
      }
      this.$router.push({ query: { ...this.$route.query, sorting: id } });
    },
    sortingClickedFull(id, idx) {
      if (this.widthWindow > 768) {
        this.sortingClicked(id, idx)
      }
    },
    sortingClicked(id, idx) {
      this.activeIndex = idx;
      const sortingQueryParam = this.$route.query.sorting || '';
      if (sortingQueryParam.indexOf(id) != -1 && sortingQueryParam.includes('-')) {
        id = sortingQueryParam.replace('-', '');
      } else {
        id = '-' + id;
      }
      const sortingType = sortFunctions[id];
      if (sortingType) {
        this.listTableBody = this.listTableBody.sort(sortingType);
      }
      this.$router.push({ query: { ...this.$route.query, sorting: id } });
    },
    sortingMounted(id) {
      const sortingType = sortFunctions[id];
      if (sortingType) {
        this.listTableBody = this.listTableBody.sort(sortingType);
      }
    },
    async getUsers() {
      try {
        const { data } = await axios.get('/test-tempo/applicants.json');
        this.listTableBody = data;
        const queryParams = this.$route.query?.sorting;
        if (queryParams) {
          this.sortingMounted(queryParams);
        } else {
          this.sortingMounted('name');
        }
      }
      catch (e) {
        console.error(e);
      }
    },
    sum(item) {
      const sum = item.subjects.reduce((sum, subject) => sum + +subject.score, 0)
      item.sum = sum;
      return sum;
    },
    percent(sum, item) {
      const maxScore = 5;
      const totalScore = maxScore * 3;
      const totalPercent = Math.round(sum / totalScore * 100)
      if (item) {
        item.percent = totalPercent;
      }
      return totalPercent
    },
    styleColorSum(percent) {
      if (percent < 25) {
        return "color: #FF0000;";
      } else if (percent >= 25 && percent < 75) {
        return "color: #FFA200;";
      } else {
        return "color: #01AA88;";
      }
    },
    styleColorScore(score) {
      if (+score <= 2.5) {
        return "color: #FF0000";
      } else if (+score > 2.5 && +score <= 4) {
        return "color: #FFA200";
      } else {
        return "color: #01AA88";
      }
    },
    styleArrow(id, index) {
      const queryParams = this.$route.query.sorting
      if (queryParams) {
        if (this.activeIndex === index || queryParams.includes(id)) {
          return queryParams.indexOf('-') === 0 ? 'transform: rotateZ(0deg);' : 'transform: rotateZ(180deg);'
        }
      }
    },
    readSizeWindow() {
      this.widthWindow = window.innerWidth;
    }
  },
  created() {
    this.getUsers();
  },
  mounted() {
    this.osInstance = OverlayScrollbars(document.querySelector('.table__wrapper'), {});
    window.addEventListener('resize', this.readSizeWindow)
  },
  beforeDestroy() {
    window.removeEventListener('resize', this.readSizeWindow)
  },
  components: {
    LineProgress,
    CircleProgress,
    Select,
    ButtonsSorting,
    IconArrow,
  },
}
</script>

<style lang="scss">
.table__wrapper {
  margin-top: 25px;
}

.not__results {
  @extend %text-400-14;
}

.table {
  display: grid;
  grid-template-columns: 1fr 180px 140px 155px 165px 140px 90px;
  row-gap: 5px;

  &__head {
    display: contents;

    th {
      cursor: pointer;
      margin-bottom: 5px;
      display: flex;
      align-items: center;
      justify-content: center;
      gap: 2px;
      @extend %text-700-12;
      color: $blue;
      width: fit-content;

      &:hover {
        color: $blue-dark;

        svg path:first-child {
          fill: $blue-dark;
        }

        svg path:last-child {
          stroke: $blue-dark;
        }
      }

      img {
        cursor: pointer;
      }

      &:first-child {
        justify-content: start;

        padding-left: 20px;
      }
    }
  }

  &__body {
    display: contents;

    &-item {
      display: contents;
    }

    &-item__list {
      display: contents;

      &:hover>td {
        background-color: $blue-super-light;
        border-top: 1px solid $blue-light;
        border-bottom: 1px solid $blue-light;

        &:first-child {
          border-left: 1px solid $blue-light;
        }

        &:last-child {
          border-right: 1px solid $blue-light;
        }
      }

      td {
        background-color: $white;
        border-right: 1px solid $blue-light;
        height: 65px;
        display: flex;
        align-items: center;
        padding: 0 20px;
        @extend %text-400-14;
        color: $black;

        &:last-child {
          justify-content: center;
          border-right: none;
        }
      }

      .item__list-score {
        @extend %text-700-14;
      }
    }
  }
}

.sorting__mobile {
  display: none;
}

@media (max-width: 1366px) {
  .table__wrapper {
    overflow-x: auto;
    padding-bottom: 10px;

    .os-scrollbar-handle {
      background-color: $blue-light;
    }

    .os-scrollbar .os-scrollbar-handle:hover {
      background-color: $blue-light;
    }
  }

  .table {
    width: 100%;
  }
}

@media (max-width: 768px) {
  .table {
    grid-auto-flow: column;
    grid-template-columns: auto 1fr;
    row-gap: 1px;

    &__head {
      th {
        margin-bottom: 0;
        justify-content: flex-start;
        padding-left: 10px;
        grid-column: 1;
        background-color: $white;
        color: $medium;
        width: 100%;
        text-align: left;

        svg {
          display: none;
        }

        &:first-child {
          margin-top: 5px;
          padding-left: 10px;
        }

      }
    }

    &__body {
      &-item__list {
        td {
          padding: 8px 0;
          height: auto;
          grid-column: 2;
          padding-left: 20px;
          padding-right: 15px;

          &:first-child {
            margin-top: 5px;
          }

          &:last-child {
            border-right: 1px solid #D5E7FF;
            justify-content: flex-start;
          }
        }
      }
    }
  }

  .sorting__mobile {
    display: flex;
    align-items: center;
    justify-content: space-between;
    gap: 10px;
  }
}
</style>