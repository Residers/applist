<template>
  <div>
    <v-btn
      color="primary"
      @click="showModal = true"
      v-if="!showModal"
      class="ma-5"
      >Добавить</v-btn
    >

    <v-row v-else>
      <v-col cols="4">
        <v-card>
          <v-toolbar elevation="0" color="primary" dark>Справочник</v-toolbar>
          <v-card-text class="pa-2">
            <div class="table">
              <div class="table__name" @click="sortName">Имя</div>
              <div class="table__telephone" @click="sortNumber">Телефон</div>
            </div>
            <span class="table" v-html="printHtml"></span>
          </v-card-text>
        </v-card>
      </v-col>
      <v-spacer></v-spacer>
      <v-col cols="4">
        <ModalWindow v-if="showModal" :modalNames="newNames" @close="close" />
      </v-col>
    </v-row>
  </div>
</template>

<script>
import ModalWindow from "@/components/ModalWindow";
export default {
  name: "tableData",
  components: { ModalWindow },

  data() {
    return {
      qhtml: "",
      counter: 0,
      printHtml: "",
      showModal: false,
      newNames: [
        {
          firstName: "first father",
          number: "+7 918 00 00 001",
        },
      ],
    };
  },
  mounted() {
    let strJSON = window.localStorage.getItem("newNames");
    this.newNames = this.isJSON(strJSON) ? JSON.parse(strJSON) : [];
  },

  methods: {
    isJSON(str) {
      // console.log(typeof str);
      if (parseInt(str) == 0) {
        return false;
      }

      if (!str || typeof str !== "string") {
        console.log("::isJSON false str=", str);
        return false;
      }
      if (/^\s*$/.test(str)) return false;
      str = str.replace(/\\(?:["\\/bfnrt]|u[0-9a-fA-F]{4})/g, "@");
      str = str.replace(
        /"[^"\\\n\r]*"|true|false|null|-?\d+(?:\.\d*)?(?:[eE][+-]?\d+)?/g,
        "]"
      );
      str = str.replace(/(?:^|:|,)(?:\s*\[)+/g, "");
      return /^[\],:{}\s]*$/.test(str);
    },
    close() {
      this.showModal = false;
    },
    //рекурсивный метод поиска потомков
    showChilds(item, counter) {
      let html = "";
      if (!counter) {
        counter = 0;
      }
      let childs = this.newNames.filter((elem) => {
        return elem["parent"] == item["firstName"];
      });
      //проверка наличия потомков
      if (Object.keys(childs).length > 0) {
        counter++;
        for (let one of childs) {
          html +=
            `<div style="margin-left:${counter * 10}px;min-width:${
              150 - counter * 10
            }px " class='table__name'>` +
            one["firstName"] +
            "( " +
            one["parent"] +
            " )" +
            "</div>" +
            `<div  class='table__telephone'>` +
            one["number"] +
            "</div>";

          let html_1 = this.showChilds(one, counter); //вызов рекурсии для проверки внучков

          if (html_1) {
            counter++;
            html += html_1;
          }
        }
      }

      this.qhtml = html;
      return this.qhtml;
    },
    sortName() {
      return this.newNames.sort((a, b) => {
        if (a["firstName"] > b["firstName"]) return 1;
        if (a["firstName"] == b["firstName"]) return 0;
        if (a["firstName"] < b["firstName"]) return -1;
      });
    },
    sortNumber() {
      return this.newNames.sort((a, b) => {
        if (a["number"] > b["number"]) return 1;
        if (a["number"] == b["number"]) return 0;
        if (a["number"] < b["number"]) return -1;
      });
    },
    printNames() {
      console.log("this.newNames =", this.newNames);
      let newArr = [];
      this.printHtml = "";

      for (let elem of this.newNames) {
        if (!elem["parent"]) {
          newArr.push(
            `<div class="table__name"> ${elem["firstName"]} </div> <div class="table__telephone">${elem["number"]}</div>`
          );
          //поиск индекса родителя
          let index = this.newNames.findIndex(function (item) {
            return item["parent"] == elem["firstName"];
          });

          let childs = this.showChilds(elem, this.counter); //поиск потомков
          newArr.splice(index, 0, childs);
        }
      }
      for (let one of newArr) {
        this.printHtml += `${one}`;
      }
    },
  },
};
</script>

<style>
.btn__showForm {
  width: 120px;
  height: 36px;
  background-color: #dddddd;
  border: 1px solid black;
  border-radius: 18px;
  position: relative;
  right: 0px;
}
.table {
  display: flex;
  flex-wrap: wrap;
  width: 300px;
}
.table__name {
  border: 1px solid black;
  min-width: 150px;
  text-align: start;
  padding: 5px;
}
.table__name-child {
  border: 1px solid black;
  min-width: 140px;
  text-align: start;
  padding: 5px;
  margin-left: 10px;
}
.table__telephone {
  border: 1px solid black;
  min-width: 150px;
  text-align: start;
  padding: 5px;
}
.col {
  position: relative;
  width: 50%;
  padding-right: 15px;
  padding-left: 15px;
}
</style>
