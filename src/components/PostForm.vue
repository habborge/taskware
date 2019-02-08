<template>
  <form>
    <div class="form-group">
      <label>Description:</label>
      <textarea class="form-control" rows="3" v-model="description"></textarea>
    </div>
    <div class="form-group">
      <label>Author:</label>
      <select class="form-control" v-model="userId">
        <option v-for="user in users" :key="user._id" :value="user._id">{{user.fullname}}</option>
      </select>
    </div>
    <button type="button" class="btn btn-primary" @click="save">Add Task</button>
  </form>
</template>

<script>
export default {
  name: "post-form",
  data() {
    return {
      description: "",
      userId: "",
      status: "",
      users: []
    };
  },
  created() {
    fetch("http://localhost:3000/api/users")
      .then(response => {
        return response.json();
      })
      .then(data => {
        const { items = [] } = data;
        const users = items.map(item => {
          const { _id, firstname = "", lastname = "" } = item;
          return {
            _id,
            fullname: `${firstname} ${lastname}`
          };
        });
        this.users = users;
      });
  },
  methods: {
    save() {
      this.$emit("save", {
        description: this.description,
        authorId: this.userId,
        status: 0
      });
    }
  }
};
</script>
