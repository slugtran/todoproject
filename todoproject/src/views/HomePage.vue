<template>
  <div class="container">
    <div class="user">
      <i
        class="el-icon-user-solid"
        size="maxi"
        style="float: right; margin-right: 200px; margin-top: 15px"
      >
        {{ email }}
      </i>
    </div>
    <div class="page">
      <el-table
        :data="
          pagedListData.filter(
            (data) =>
              !search || data.title.toLowerCase().includes(search.toLowerCase())
          )
        "
      >
        <el-table-column label="MY TODO-LIST" prop="title">
          <template slot-scope="scope">
            <ValidationProvider name="title" rules="required">
              <div slot-scope="{ errors }">
                <el-input
                  class="my_input"
                  v-if="!scope.row.readonly"
                  placeholder="Enter title"
                  v-model="scope.row.title"
                >
                </el-input>
                <p style="color: red; font-size: 10px">
                  {{ errors[0] }}
                </p>
              </div>
            </ValidationProvider>
            <el-form-item
              label=""
              style="text-align: left; font-weight: bold"
              prop="newTitle"
            >
            </el-form-item>
            <div
              v-if="scope.row.readonly"
              @click="openShowTitleModal(scope.row)"
            >
              {{ scope.row.title }}
            </div>
          </template>
        </el-table-column>
        <el-table-column align="right">
          <template slot="header">
            <el-input
              v-model="search"
              size="mini"
              placeholder="Enter to search"
              style="float: left; width: 60%"
            />
            <el-button
              size="mini"
              type="primary"
              style="float: right; width: 20%"
              @click="addNewTitle()"
            >
              Add item
            </el-button>
          </template>
          <template slot-scope="scope">
            <el-button
              v-if="!scope.row.readonly"
              icon="el-icon-check"
              style="border: none"
              @click="handleSave(scope.row)"
            >
            </el-button>
            <el-button
              v-if="!scope.row.readonly"
              icon="el-icon-close"
              style="border: none; color: red"
              @click="openShowDeleteModal(scope.$index, scope.row)"
            ></el-button>
            <el-button
              v-if="scope.row.readonly"
              icon="el-icon-edit"
              style="border: none"
              type="primary"
              @click="openShowEditModal(scope.row)"
            ></el-button>
            <el-button
              v-if="scope.row.readonly"
              icon="el-icon-delete-solid"
              style="border: none"
              type="danger"
              @click="openShowDeleteModal(scope.$index, scope.row)"
            ></el-button>
          </template>
        </el-table-column>
      </el-table>
      <el-pagination
        layout="prev, pager, next"
        :total="listData.length"
        page-size="5"
        @current-change="setPage"
      >
      </el-pagination>

      <el-dialog :visible.sync="isShowTitleModal" width="400px">
        <span slot="title" style="font-size: 20px; font-weight: bold">
          {{ title.title }}
        </span>
        <span style="float: left">Todo</span>
        <br />
        <br />
        <br />
        <br />
        <div style="text-align: left">
          <el-checkbox
            v-model="radio"
            label=""
            v-for="item in listItem"
            :key="item.id"
          >
            <div>{{ item.content }}</div>
          </el-checkbox>
          <!--           <div><el-radio v-model="radio" label="1">Item 1</el-radio></div>
          <div><el-radio v-model="radio" label="2">Item 2</el-radio></div>
          <div><el-radio v-model="radio" label="3">Item 3</el-radio></div> -->
        </div>
        <span slot="footer" class="dialog-footer">
          <el-button
            type="primary"
            style="width: 80px; margin-top: 50px"
            size="mini"
            @click="closeShowTitleModal()"
            >Close</el-button
          >
        </span>
      </el-dialog>
      <el-dialog
        title="EDIT"
        :visible.sync="isShowEditModal"
        style="width-min: 150px, width-max: 200px;"
      >
        <div class="text-center">
          <span>{{ title.title }}</span>
        </div>
        <div class="text-left">
          <span style="float: left">Title</span>
          <span>
            <el-input placeholder="Enter title" v-model="newEditTitle.title">
            </el-input>
          </span>
          <br />
          <br />
          <el-table :data="listItem" style="border: none">
            <el-table-column
              label="Todo"
              prop="title"
              style="size-font: 56px; width=80%"
            >
              <template slot-scope="scope">
                <el-input
                  class="my_input"
                  v-if="!scope.row.readonly"
                  placeholder="Enter title"
                  v-model="newEditItemTitle.content"
                >
                </el-input>
                <div v-if="scope.row.readonly">
                  {{ scope.row.content }}
                </div>
              </template>
            </el-table-column>
            <el-table-column align="right">
              <template slot-scope="scope">
                <el-button
                  icon="el-icon-edit"
                  style="border: none"
                  @click="openEditInput(scope.row)"
                ></el-button>
                <el-button
                  icon="el-icon-delete-solid"
                  style="border: none"
                  @click="openShowDeleteContent((scope.$index, scope.row))"
                ></el-button>
              </template>
            </el-table-column>
          </el-table>
          <el-row>
            <el-button
              class="class_btn"
              @click="addNewItem()"
              style="
                color: blue;
                text-decoration-line: underline;
                margin-left: -10px;
              "
              >Add item</el-button
            >
          </el-row>
          <span slot="footer" class="dialog-footer">
            <el-button
              type="primary"
              style="auto: margin; width: 80%; margin-top: 50px"
              @click="handleUpdateTitle()"
              >Save</el-button
            >
          </span>
        </div>
      </el-dialog>
      <el-dialog item="" :visible.sync="isShowDeleteModal">
        <div class="text-left" style="height: 100px">
          <span style="float: left; font-weight: bold">Are you sure?</span>
          <br />
          <p style="float: left">
            Are you sure you want to delete this record?
          </p>
          <span
            slot="footer"
            class="dialog-footer"
            style="float: right; margin-top: 50px"
          >
            <el-button type="danger" size="mini" @click="closeShowDeleteModal()"
              >Cancel
            </el-button>
            <el-button type="primary" size="mini" @click="handleDeleteTitle()"
              >Accept
            </el-button>
          </span>
        </div>
      </el-dialog>
      <div class="linker">Copyright @2019</div>
    </div>
  </div>
</template>

<script>
import axios from "axios";
import {} from "vee-validate";
const _ = require("lodash");
export default {
  data() {
    return {
      errored: false,
      email: "",
      listData: [],
      listItem: [],
      page: 1,
      pageSize: 5,
      search: "",
      title: "",
      radio: "",
      item: "",
      rules: "",
      indexTitle: "",
      newEditTitle: "",
      newEditItemTitle: { item: "", readonly: false },
      newItem: { item: "", readonly: false },
      newItemContent: { itemContent: "", readonly: false },
      isShowTitleModal: false,
      isAddNewTitle: false,
      isShowEditModal: false,
      isShowDeleteModal: false,
      isAddNewItem: false,
      erroMessage: false,
      isShowDeleteContent: false,
      indexItem: "",
    };
  },
  //
  created() {
    this.getListTitle();
  },

  //
  computed: {
    pagedListData() {
      return this.listData.slice(
        this.pageSize * this.page - this.pageSize,
        this.pageSize * this.page
      );
    },
  },

  methods: {
    getEmail() {
      this.email = localStorage.getItem("email");
    },

    getListItem(todoId) {
      axios
        .get(`https://mockup-api.herokuapp.com/api/v1/todos/${todoId}/items`, {
          headers: {
            Authorization: `token ${localStorage.getItem("token")}`,
          },
        })
        .then((response) => {
          console.log(response);
          this.listItem = response.data.map((newitem) => {
            return {
              id: newitem.id,
              todoId: newitem.todo_id,
              content: newitem.content,
              readonly: true,
            };
          });
        })
        .catch((e) => {
          this.errors.push(e);
        });
    },

    async getListTitle() {
      await axios
        .get(`https://mockup-api.herokuapp.com/api/v1/todos`, {
          headers: {
            Authorization: `token ${localStorage.getItem("token")}`,
          },
        })
        .then((response) => {
          this.listData = response.data.map((item) => {
            return {
              id: item.id,
              title: item.title,
              userId: item.user_id,
              items: item.items,
              readonly: true,
            };
          });
        })
        .catch((e) => {
          this.errors.push(e);
        });
      this.getEmail();
    },

    handleEdit(index, row) {
      console.log(index, row);
      this.isShowEditModal = false;
      this.isShowDeleteModal = false;
    },
    handleTitle() {
      this.$alert("This is a message", "Title", {
        confirmButtonText: "OK",
        callback: (action) => {
          this.$message({
            type: "info",
            message: `action: ${action}`,
          });
        },
      });
    },
    async handleSave(scope) {
      await axios
        .post(
          `https://mockup-api.herokuapp.com/api/v1/todos`,
          { title: scope.title },
          {
            headers: {
              Authorization: `token ${localStorage.getItem("token")}`,
            },
          }
        )
        .then((response) => {
          console.log(response);
        })
        .catch((e) => {
          this.errors.push(e);
        });
      scope.readonly = true;
      this.isAddNewTitle = false;
    },
    async handleDeleteTitle() {
      await axios
        .delete(
          `https://mockup-api.herokuapp.com/api/v1/todos/${this.title.id}`,
          {
            headers: {
              Authorization: `token ${localStorage.getItem("token")}`,
            },
          }
        )
        .then((response) => {
          console.log(response);
        })
        .catch((e) => {
          this.errors.push(e);
        });
      if (this.isAddNewTitle && this.indexTitle == 0) {
        this.isAddNewTitle = false;
      }
      this.listData.splice(this.indexTitle, 1);
      this.isShowDeleteModal = false;
    },

    async handleDeleteItem() {
      await axios
        .delete(
          `https://mockup-api.herokuapp.com/api/v1/todos/${this.title.id}/items/${this.content.id}`,
          {
            headers: {
              Authorization: `token ${localStorage.getItem("token")}`,
            },
          }
        )
        .then((response) => {
          console.log(response);
        })
        .catch((e) => {
          this.errors.push(e);
        });
      if (this.isAddNewItem && this.indexItem == 0) {
        this.isAddNewItem = false;
      }
      this.listItem.splice(this.indexItem, 1);
      this.isShowDeleteContent = false;
    },

    async handleUpdateTitle() {
      await axios
        .put(
          `https://mockup-api.herokuapp.com/api/v1/todos/${this.newEditTitle.id}`,
          { title: this.newEditTitle.title },
          {
            headers: {
              Authorization: `token ${localStorage.getItem("token")}`,
            },
          }
        )
        .then((response) => {
          console.log(response);
        })
        .catch((e) => {
          this.errors.push(e);
        });
      this.isShowEditModal = false;
      this.getListTitle();
    },

    async openShowTitleModal(scope) {
      this.title = scope;
      await this.getListItem(scope.id);
      this.isShowTitleModal = true;
      console.log(scope);
    },
    closeShowTitleModal() {
      this.isShowTitleModal = false;
    },
    openShowEditModal(scope) {
      this.title = scope;
      this.newEditTitle = _.cloneDeep(this.title);
      this.isShowEditModal = true;
    },
    openShowDeleteModal(index, scope) {
      this.title = scope;
      this.indexTitle = index;
      this.isShowDeleteModal = true;
    },
    openShowDeleteContent(index, scope) {
      this.content = scope;
      this.indexItem = index;
      this.isShowDeleteContent = true;
    },
    closeShowDeleteContent() {
      this.isShowDeleteContent = false;
    },
    openEditInput(scope) {
      this.newEditItemTitle = _.cloneDeep(scope);
      console.log(scope);
      scope.readonly = false;
    },
    closeShowEditModal() {
      this.isShowEditModal = false;
    },
    closeShowDeleteModal() {
      this.isShowDeleteModal = false;
    },
    addNewTitle() {
      if (!this.isAddNewTitle) {
        var newTitle = _.cloneDeep(this.newItem);
        this.isAddNewTitle = true;
        this.listData.unshift(newTitle);
      }
    },
    addNewItem() {
      if (!this.isAddNewItem) {
        var newContent = _.cloneDeep(this.newItemContent);
        this.isAddNewItem = true;
        this.listItem.push(newContent);
      }
    },

    setPage(val) {
      this.page = val;
    },

    //
    onChangePage(next) {
      this.title = next;
    },
    openShowItemModal(scope) {
      this.item = scope;
      this.isShowItemModal = true;
      console.log(scope);
    },
  },
};
</script>
<style>
.page {
  background-color: white;
  width: 80%;
  margin: auto;
  padding: 20px 20px 30px 20px;
}
.linker {
  padding-top: 40px;
  text-align: center;
}
.btn-group {
  width: 95%;
  border: groove;
}
.btnEdit {
  background-color: white;
  border: none;
  height: 30px;
}
.btnDelete {
  background-color: white;
  border: none;
}
.class_btn {
  margin-top: 3px;
  margin-bottom: 15px;
  background-color: #fff;
  border: none;
  float: left;
}
.user {
  width: 100%;
  background-color: rgb(102, 197, 134);
  height: 50px;
}
</style>
