<template>
  <div class="pb-3">
      <div class="container">
        <div class="row addtodo">
          <button class="Btn btn-success" v-on:click="openForm" v-show="!isCreating">
              <i class="fa fa-plus"></i>
          </button>
        </div>
        <div class="row addtodo" v-show="isCreating">
          <div class="card" style="width: 18rem;">
            <div class="card-body">
              <div class="form-group text-center">
                <label>Title</label>
                <input type="text" class="form-control" aria-describedby="emailHelp" v-model="title" placeholder="title" ref="title">
                <p v-show="warning">The field is required</p>
              </div>
              <div class="form-group text-center">
                <label>Description</label>
                <input type="text" class="form-control" v-model="description" placeholder="description" required>
                <p v-show="warning">The field is required</p>
              </div>
              <div class="btn-group btn-block" role="group" aria-label="Basic example">
                <button type="button" class="btn btn-success" v-on:click="addTodo">Create</button>
                <button type="button" class="btn btn-danger" v-on:click="closeForm">Cancel</button>
              </div>
            </div>
          </div>
        </div>
      </div>
  </div>
</template>
<script>
import { uuid } from "vue-uuid";
export default {
  name: "AddTodo",
  data() {
    return {
      title: "",
      description: "",
      isCreating: false,
      warning : false
    };
  },
  methods: {
    openForm() {
      this.isCreating = true;
    },
    closeForm() {
      this.isCreating = false;
    },
    addTodo(e) {
      e.preventDefault();
      if(this.title.length > 0 && this.description.length > 0)
      {
        this.warning = false;
        const newTodoObj = {
          id: uuid.v4(),
          title: this.title,
          description : this.description,
          completed: false
        };
        this.$store.dispatch('todoStore/addTodo', newTodoObj);
        this.title = "";
        this.description = "";
        this.isCreating = false;
      }
      else{
        this.warning = true;
        this.focusInput();
      }
    },
    focusInput(){
      this.$refs.title.focus();
    }
  }
};
</script>
<style scoped>
.addtodo{
    display: flex;
    justify-content: center;
}
p{
  float:left;
  color: #ff0000;
}
</style>
