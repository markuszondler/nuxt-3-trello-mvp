<script>
import {workspaceList} from '../../store/global.js'

export default {
  setup() {
    return {
      workspaceList
    }
  },
  data: () => ({
    workspaceName: '',
    board: {
      columns: []
    }
  }),
  methods: {
    createColumn() {
      this.board.columns.push({
        newItemName: '',
        items: []
      })
    },
    createCard(column) {
      if (column.newItemName) {
        column.items.push({
          id: 2,
          name: column.newItemName
        })
        column.newItemName = ''
      }
    }
  },
  mounted() {
    this.workspaceName = this.workspaceList.find(
        workspace => workspace.id === Number(this.$route.params.id)
    ).name
  }
}
</script>

<template>
  <main class="workspace-page">
    <h1>{{ workspaceName }} Workspace (#{{ $route.params.id }})</h1>
    <section>
      <button @click="createColumn" style="margin-bottom: 15px;">Create column</button>
      <div class="column-grid">
        <section class="board-column" v-for="column in board.columns" :key="column">
          <input type="text" v-model="column.newItemName" @keyup.enter="createCard(column)"
                 style="padding: 5px; font-size: 0.9rem;margin-bottom: 10px;" placeholder="New card title">
          <button @click="createCard(column)" style="margin-bottom: 15px;">Create Card</button>
          <ul style="margin: 0; padding: 0;">
            <li v-for="item in column.items" :key="item.id" class="base-card">
              {{ item.name }}
            </li>
          </ul>
        </section>
      </div>
    </section>
  </main>
</template>

<style>
.base-card {
  border: 1px solid #222;
  padding: 5px;
  list-style: none;
  background: #fff;
  margin-bottom: 10px;
}

.workspace-page {
  padding: 30px;
}

.workspace-page h1 {
  margin-top: 0;
}

.column-grid {
  display: grid;
  grid-template-columns: repeat(v-bind(board.columns.length), 1fr);
}

.board-column {
  background-color: #eee;
  border: 1px solid #222;
  height: 80vh;
  margin-right: 1rem;
  padding: 10px;
}
</style>
