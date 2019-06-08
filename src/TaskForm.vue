<template>
  <form @submit.prevent="create">
    <div class="input-group">
      <input v-model.trim="draft" type="text" class="form-control">
      <div class="input-group-append">
        <button :disabled="draft ? false : true" class="btn btn-primary">Crear tarea</button>
      </div>
    </div>
  </form>
</template>

<script>
export default {
  data() {
    return {
      draft: ""
    };
  },
  methods: {
    create() {
      this.$emit("created", {
        description: this.draft,
        pending: true,
        editing: false
      });

      this.draft = "";
    }
  }
};
</script>