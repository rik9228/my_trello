<template>
  <div class="list">
    <div class="listheader">
      <p class="list-title" v-on:click="doEdit" v-if="!edit">{{ title }}</p>
      <input v-else type="text" class="list-title" v-model="title" v-on:blur="edit = false" v-focus />
      <p class="list-counter">total: {{ totalCardInList }}</p>
      <div class="deletelist" @click="removeList">×</div>
    </div>
    <draggable group="cards" :list="cards" @end="$emit('change')">
      <card v-for="(item, index) in cards" :body="item.body" :key="item.id" :cardIndex="index" :listIndex="listIndex" />
    </draggable>
    <card-add :listIndex="listIndex" />
  </div>
</template>

<script>
import CardAdd from "./CardAdd";
import Card from "./Card";
import draggable from "vuedraggable";

export default {
  components: {
    CardAdd,
    Card,
    draggable,
  },
  data() {
    return {
      edit: false,
    };
  },
  directives: {
    focus: {
      // ディレクティブ定義
      inserted: function(el) {
        el.focus();
      },
    },
  },
  props: {
    title: {
      type: String,
      required: true,
    },
    cards: {
      type: Array,
      required: true,
    },
    listIndex: {
      type: Number,
      required: true,
    },
  },
  computed: {
    totalCardInList() {
      return this.cards.length;
    },
  },
  methods: {
    removeList: function() {
      if (confirm("本当にこのリストを削除しますか？")) {
        this.$store.dispatch("removelist", { listIndex: this.listIndex });
      }
    },
    doEdit() {
      this.edit = true;
    },
  },
};
</script>
