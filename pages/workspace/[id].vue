<script>
import {workspaceList} from '../../store/global.js'

export default {
  setup() {
    return {
      workspaceList
    }
  },
  data: () => ({
    newColumnName: '',
    workspaceName: '',
    board: {
      columns: [
        {
          columnName:'Luffy',
          newItemName: '',
          items: []
        },
        {
          columnName: 'Zorro',
          newItemName: '',
          items: []
        },
        {
          columnName: 'Nani',
          newItemName: '',
          items: []
        }
      ]
    }
  }),
  methods: {
    createColumn() {
      if (this.newColumnName) {
        this.board.columns.push({
          columnName: this.newColumnName,
          newItemName: '',
          items: []
        })
        this.newColumnName = ''
      }
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
      <input v-model="newColumnName" @keyup.enter="createColumn" type="text" placeholder="New column title"
             style="display: block; width: 100%; padding: 5px; font-size: 0.9rem; margin-bottom: 10px;"/>
      <button @click="createColumn" style="margin-bottom: 15px;">Create column</button>
      <div class="column-grid">
        <section class="board-column" v-for="column in board.columns" :key="column">
          <h3 style="margin-top: 5px;">{{ column.columnName }}</h3>
          <input type="text" v-model="column.newItemName" @keyup.enter="createCard(column)"
                 style="padding: 5px; font-size: 0.9rem;margin-bottom: 10px;" placeholder="New card title">
          <button @click="createCard(column)" style="margin-bottom: 15px;">Create Card</button>
          <ul style="margin: 0; padding: 0;">
            <li v-for="item in column.items" :key="item.id" class="base-card">
              <h5>{{ item.name }}</h5>
              <select name="luffy-migrate-list" id="luffy-migrate-list" style="padding: 5px; width: 100%;margin-bottom: 5px;">
                <option v-for="column in board.columns" :key="`migrate-${column.columnName}`" :value="column.columnName">{{ column.columnName }}</option>
              </select>
              <button>Migrate</button>
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
