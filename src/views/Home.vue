<template>
  <v-card elevation="5" max-width="90%" class="mx-auto my-3">
    <v-toolbar>
      PC Management System
      <v-spacer></v-spacer>
      <v-text-field
        v-model="search"
        append-icon="mdi-magnify"
        label="Search"
        single-line
        hide-details
      ></v-text-field>

      <v-dialog v-model="dialogDetail"> aa</v-dialog>

      <v-dialog v-model="dialogDelete" max-width="500px">
        <v-card>
          <v-card-title>削除してよろしいですか？</v-card-title>
          <v-card-actions>
            <v-spacer></v-spacer>
            <v-btn color="blue darken-1" text @click="dialogDelete = false"
              >キャンセル</v-btn
            >
            <v-btn color="blue darken-1" text @click="deleteItem">削除</v-btn>
            <v-spacer></v-spacer>
          </v-card-actions>
        </v-card>
      </v-dialog>
    </v-toolbar>

    <v-data-table
      :headers="headers"
      :items="items"
      :items-per-page="5"
      :search="search"
      sort-by="Computer_Name"
      class="elevation-1"
    >
      <template v-slot:item.LoginTime="{ item }"></template>

      <template v-slot:item.actions="{ item }">
        <v-icon small class="mr-2" @click="showDetail(item)">mdi-pencil</v-icon>
        <v-icon small @click="deleteItem(item)"> mdi-delete </v-icon>
      </template>
    </v-data-table>
  </v-card>
</template>

<script>
import axios from "axios";

export default {
  name: "Home",
  data() {
    return {
      test: "",
      dialogDetail: false,
      dialogDelete: false,
      search: "",
      headers: [
        { text: "PC Name", value: "Computer_Name" },
        { text: "User Name", value: "User_Name" },
        { text: "Model", value: "Model" },
        { text: "CPU", value: "CPU" },
        { text: "RAM", value: "RAM" },
        { text: "Type", value: "Type" },
        { text: "OS", value: "OS" },
        { text: "Office", value: "Office" },
        { text: "Last Login", value: "LoginTime" },
        { text: "IP", value: "IP_Address" },
        { text: "Sirial#", value: "PC_Sirial" },
        { text: "Remarks", value: "Remarks" },
        { text: "Purchase Date", value: "Purchase_Date" },
        { text: "Disposial Date", value: "Disposial_Date" },
        { text: "Actions", value: "actions", sortable: false },
      ],
      items: [],
    };
  },
  created() {
    axios
      .get("http://lejnet/API/accdb?db=ISNet/PCM/PCM.accdb&table=PCList")
      .then((res) => {
        this.items = res.data;
      });
  },
  methods: {
    showDetail: function (item) {
      this.dialogDetail = true;
    },
    deleteItem: function (item) {
      this.dialogDelete = true;
    },
  },
};
</script>