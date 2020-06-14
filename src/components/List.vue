<template>
  <div class="list">
    <div class="listheader">
      <p class="list-title">{{ title }}</p>
      <p class="list-counter">total: {{ totalCardInList }}</p>
      <div class="deletelist" @click="removeList">×</div>
    </div>
    <!--    group属性を使うことで他のコンポーネントへドラッグ&ドロップさせる、-->
    <!--    または他のコンポーネントからのドラッグ&ドロップを受け付ける-->
    <!--    endイベントはドラッグ&ドロップの操作が終わったあと、最後に発生するイベント-->
    <draggable group="cards"
               :list="cards"
               @end="$emit('change')">
      <card v-for="(item, index) in cards"
            :body="item.body"
            :key="item.id"
            :cardIndex="index"
            :listIndex="listIndex"
      />
      <card-add :listIndex="listIndex"/>
    </draggable>
  </div>
</template>

<script>
import CardAdd from './CardAdd'
import Card from './Card'
import draggable from 'vuedraggable'

export default {
  components: {
    CardAdd,
    Card,
    draggable
  },
  // 親コンポーネントから受け取るデータを定義
  props: {
    title: {
      type: String,
      required: true
    },
    cards: {
      type: Array,
      required: true
    },
    listIndex: {
      type: Number,
      required: true
    }
  },
  computed: {
    totalCardInList() {
      return this.cards.length
    }
  },
  methods: {
    removeList: function() {
      if(confirm('本当にこのリストを削除しますか？')){
        this.$store.dispatch('removelist', { listIndex: this.listIndex })
      }
    },
  }
}
</script>