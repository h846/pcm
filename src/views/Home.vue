<template>
  <v-card max-width="90%" class="mx-auto mt-10">
    <v-toolbar class="blue-grey lighten-5">
      PC Management System
      <v-spacer></v-spacer>
      <v-text-field
        v-model="search"
        append-icon="mdi-magnify"
        label="Search"
        single-line
        hide-details
      ></v-text-field>
    </v-toolbar>

    <v-data-table
      :headers="headers"
      :items="items"
      :items-per-page="10"
      :search="search"
      sort-by="Computer_Name"
      class="elevation-1"
    >
      <template v-slot:item.actions="{ item }">
        <v-icon small class="mr-2" @click="showDetail(item)"
          >mdi-format-list-bulleted</v-icon
        >
        <v-icon small @click="deleteItem(item)"> mdi-delete </v-icon>
      </template>
    </v-data-table>
    <!-- Deatail Dialog -->
    <v-dialog v-model="dialogDetail" max-width="500px">
      <v-card>
        <v-card-title><span class="headline">PC Detail</span></v-card-title>

        <v-card-text>
          <v-container>
            <v-text-field
              v-for="(val, key) in item"
              :key="val"
              :label="key"
              :value="val"
            >
            </v-text-field>
          </v-container>
        </v-card-text>

        <v-card-actions>
          <v-spacer></v-spacer>
          <v-btn color="blue darken-1" text @click="dialogDetail = flase">
            Close
          </v-btn>
        </v-card-actions>
      </v-card>
    </v-dialog>
    <!-- Delete Dialog -->
    <v-dialog v-model="dialogDelete" max-width="500px">
      <v-card>
        <v-card-title>削除してよろしいですか？</v-card-title>
        <v-card-actions>
          <v-spacer></v-spacer>
          <v-btn color="blue darken-1" text @click="dialogDelete = false"
            >キャンセル</v-btn
          >
          <v-btn color="blue darken-1" text @click="delRecord(item)"
            >削除</v-btn
          >
          <v-spacer></v-spacer>
        </v-card-actions>
      </v-card>
    </v-dialog>

    <!--loading-->
    <v-overlay :value="overlay">
      <v-progress-circular indeterminate size="64"></v-progress-circular>
    </v-overlay>
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
      overlay: false,
      item: {},
      loginTime: "",
      search: "",
      headers: [
        { text: "PC Name", value: "Computer_Name" },
        { text: "User Name", value: "User_Name" },
        { text: "Model", value: "Model" },
        //{ text: "CPU", value: "CPU" },
        //{ text: "RAM", value: "RAM" },
        //{ text: "Type", value: "Type" },
        { text: "OS", value: "OS" },
        //{ text: "Office", value: "Office" },
        { text: "Last Login", value: "LoginTime" },
        //{ text: "IP", value: "IP_Address" },
        //{ text: "Sirial#", value: "PC_Sirial" },
        //{ text: "Remarks", value: "Remarks" },
        //{ text: "Purchase Date", value: "Purchase_Date" },
        //{ text: "Disposial Date", value: "Disposial_Date" },
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
      this.item = Object.assign({}, item);
    },
    deleteItem: function (item) {
      this.dialogDelete = true;
    },
    delRecord: function () {
      let pcName = this.item["Computer_Name"];
      axios
        .post("http://lejnet/API/accdb", {
          db: "ISNet/PCM/PCM.accdb",
          sql: `DELETE FROM PCList WHERE Computer_Name = '${pcName}'`,
        })
        .then((res) => {
          console.log(res);
          this.overlay = true;
          //再取得
          axios
            .get("http://lejnet/API/accdb?db=ISNet/PCM/PCM.accdb&table=PCList")
            .then((res) => {
              this.items = res.data;
            });
          this.dialogDelete = false;
          this.overlay = false;
        });
    },
  },
};
</script>