<template>
  <div id="app">
  <div class="body">
   <div class="search">
    <input
        v-model="searchValue"
        type="text"
    >
    <button @click="handleSearch">
      search
    </button>
    <div
         v-if="coincidencesCount !== null"
         class="search__result">
          {{coincidencesCount > 0 ?  `Совпадений найдено: ${coincidencesCount}` : 'Ничего не найдено'}}
     </div>
   </div>
    <div class="table">
      <div
          v-for="(content, i) in templateArr"
          :key="content.id"
          class="cell"
      >
        <div
            class="header"
            :class="{'header_visible' : i < 5}"
        >
          {{headers[content.headerId - 1].title}}
        </div>
        <div
            class="content"
            :class="{'content_red' : content.isFinded}"
        >
          {{content.title}}
        </div>
      </div>
    </div>
   </div>
  </div>
</template>

<script>
export default {
  name: 'cftTest',
  data() {
    return {
      searchValue: '',
      coincidencesCount: null,
      headers: [
        {
          id: 1,
          title: 'Города',
          modif: 'red',
        },
        {
          id: 2,
          title: 'Страны',
        },
        {
          id: 3,
          title: 'Моря',
        },
        {
          id: 4,
          title: 'Океаны',
        },
        {
          id: 5,
          title: 'Горы',
        },
      ],
      templateArr: [],

    };
  },
  async mounted() {
    await this.getTemplateArr();
  },
  methods: {
    handleSearch() {
      let coincidences = 0;
      this.templateArr.forEach((content) => {
        if (content.headerId !== 5) return;

        if (this.searchValue && content.title.toLowerCase().includes(this.searchValue.toLowerCase())) {

          content.isFinded = true;
          coincidences++;
          return;
        }

        content.isFinded = false;
      })

      this.coincidencesCount = this.searchValue ? coincidences: null;
      this.$forceUpdate();
      },



    getTemplateArr() {
      let headerId = 0;
      let circle = 0;

      const titleArr = [
        [
          "Томск", "Москва", "Санкт-Петербург", "Казань", "Новосибирск"
        ],
        [
          "Россия", "Китай", "Франция", "Испания", "Монако"
        ],
        [
          "Карибское море", "Азовское море", "Балтийское море", "Эгейское море", "Японское море"
        ],
        [
          "Тихий", "Атлантический", "Северный Ледовитый", "Индийский", "Южный"
        ],
        [
          "Эльбрус", "Эверест", "Чогори", "Канченджанга", "Лхоцзе"
        ],
      ];

      for (let i = 1; i < 26; i++) {
        headerId++;
        if (headerId > 5) {
          circle++;
          headerId = 1;
        }

        this.templateArr.push({
          id: i,
          title: titleArr[headerId - 1][circle],
          headerId,
        });
      }
      console.log(this.templateArr);
    },
  },
}
</script>

<style scoped lang="scss">
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  display: flex;
  justify-content: center;

  .body {
    max-width: 1600px;
    min-width: 421px;
    width: 100%;
  }

  .header {
    display: none;
    padding: 10px;
    border: 1px solid;
    font-size: x-large;
    font-weight: bold;
    background-color: lightblue;

    &_visible {
      display: block;
    }
  }
  .search{
    margin-top: 1%;
    margin-bottom: 1%;
   display: grid;
    grid-template-columns: repeat(2, max-content);
    justify-content: center;
    row-gap: 3px;

}
  .content {
    padding: 10px;
    border: 1px solid;
    font-size: large;
    &:hover {
      opacity: 0.5;
      background-color: burlywood;
    }

    &_red {
      background-color: #de878a;
    }
  }

  .table{
    display: grid;
    margin: 0 auto;
    text-align: center;
    grid-template-columns:  repeat(5,1fr);
       }

  @media screen and (max-width: 500px) {
    .header {
      display: block;
    }
    .table {
      grid-template-columns: 1fr;
    }
  }
}

</style>