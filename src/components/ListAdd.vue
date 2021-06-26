<template>
  <form :class="classList" @submit.prevent="addList">
    <input v-model="title" class="text-input" placeholder="Add new list" type="text" @focusin="startEditing" @focusout="finishEditing" />
    <button v-if="isEditing || titleExists" class="add-button" type="submit">
      Add
    </button>
  </form>
</template>

<script>
export default {
  data() {
    return {
      title: "",
      isEditing: false,
    };
  },
  computed: {
    classList() {
      const classList = ["addlist"];
      if (this.isEditing) {
        classList.push("active");
      }

      if (this.titleExists) {
        classList.push("addable");
      }
      return classList;
    },

    titleExists() {
      return this.title.length > 0; //trueを返す?
    },
  },
  methods: {
    addList() {
      // actionsを呼び出し
      this.$store.dispatch("addlist", { title: this.title });
      this.title = "";
    },
    startEditing() {
      this.isEditing = true;
    },
    finishEditing() {
      this.isEditing = false;
    },
  },
};
</script>

<style></style>
