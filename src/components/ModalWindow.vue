<template>
  <v-card width="400">
    <v-toolbar elevation="0" color="primary" dark>
      <v-row justify="space-between">
        <v-card-title>Добавление пользователя</v-card-title>

        <v-btn color="white" @click="$emit('close')" icon
          ><v-icon>mdi-close</v-icon></v-btn
        ></v-row
      >
    </v-toolbar>
    <v-card-text>
      <v-text-field v-model="firstNameValue" label="Имя" />
      <v-text-field v-model="numberValue" label="Телефон" />
      <v-select
        v-model="parentValue"
        label="Начальник"
        :items="newNames"
        item-text="firstName"
      ></v-select>
    </v-card-text>
    <v-card-actions
      ><v-btn color="primary" @click="submitClick"
        >Сохранить</v-btn
      ></v-card-actions
    >
  </v-card>
</template>

<script>
export default {
  name: "modalWindow",
  props: {
    modalNames: Array,
  },
  data() {
    return {
      firstNameValue: "",
      numberValue: "",
      parentValue: "",
      newNames: [],
    };
  },
  mounted() {
    this.newNames = this.modalNames;
    // window.localStorage.setItem("newNames", ""); //Зачистка localStorage
  },
  methods: {
    showForm() {
      return;
    },
    submitClick() {
      if (this.firstNameValue != "") {
        this.newNames.push({
          firstName: this.firstNameValue,
          number: this.numberValue,
          parent: this.parentValue,
        });
        this.firstNameValue = "";
        this.numberValue = "";
        this.parentValue = "";
      }
      let valueNames = JSON.stringify(this.newNames);
      console.log("value names", valueNames);
      window.localStorage.setItem("newNames", valueNames);
      this.$parent.printNames();
    },
  },
};
</script>

<style>
*,
*::before,
*::after {
  box-sizing: border-box;
}

html {
  font-family: sans-serif;
  line-height: 1.15;
}

.modal__container {
  width: 100%;
  max-width: 400px;
  padding-right: 15px;
  padding-left: 15px;
  margin-left: auto;
  margin-right: auto;
  padding-bottom: 10px;
  /* background-color: #fff8e1; */
  border: 1px solid black;
}

.row {
  display: flex;
  align-items: center;
  flex-wrap: wrap;
}

.col__modal {
  position: relative;
  width: 100%;
  padding-right: 15px;
  padding-left: 15px;
}

input {
  height: 30px;
}

.btn__modal {
  width: 120px;
  height: 36px;
  background-color: #dddddd;
  border: 1px solid black;
  border-radius: 18px;
  position: relative;
}
.btn__close {
  position: absolute;
  right: 10px;
  top: 10px;
  border: none;
  background-color: transparent;
  font-size: 14px;
}
</style>
