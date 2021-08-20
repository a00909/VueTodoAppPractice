<template>
  <div class="todo-list">
    <h1 v-text="title"/>
    <input
          class="new-todo"
          autofocus
          autocomplete="off"
          placeholder="What needs to be done?"
          v-model="newTodo"
          @keyup.enter="addTodo"
    />
    <div v-for="(card, cardIndex) in cards" :key="card.id" class="todo-cards">
      <div class="todo-card">
        <div v-if="!card.editTitle" v-text="card.title" class="todo-title" @dblclick="editTodoTitle(card)"/>
        <input
          v-else
          class="todo-title-edit"
          type="text"
          v-model="card.title"
          @keyup.enter="finishEditTodoTitle(card)"
          @blur="finishEditTodoTitle(card)"
          @keyup.esc="cancelEditTodoTitle(card)"
          v-focus/>
        <div class='remove-card' @click="removeCard(cardIndex)">
            &times;
        </div>
        <div v-for="(item, itemIndex) in card.items" :key="item.id" class="todo-contents">
          <input type="checkbox" v-model="item.completed"/>
          <div
            v-if="!item.editContent"
            v-text="item.content"
            class="todo-content"
            :class="{ completed:item.completed }"
            @dblclick="editTodoContent(item)"
            @click="toggleContentCompleted(item)"/>
          <input
            v-else
            class="todo-content-edit"
            type="text"
            v-model="item.content"
            @keyup.enter="finishEditTodoContent(item)"
            @blur="finishEditTodoContent(item)"
            @keyup.esc="cancelEditTodoContent(item)"
            v-focus/>
          <div class='remove-item' @click="removeContent(cardIndex,itemIndex)">
            &times;
          </div>
        </div>
        <input
            class="new-content"
            placeholder="Add new todos"
            v-model="cards[cardIndex].newItemContent"
            @keyup.enter="addTodoContent(cardIndex)"/>
      </div>
    </div>
  </div>
</template>

<script>
var STORAGE_KEY = 'todos-vuejs-2.0'
var todoStorage = {
  fetch: function () {
    var todos = JSON.parse(localStorage.getItem(STORAGE_KEY) || '[]')
    todos.forEach(function (todo, index) {
      todo.id = index
    })
    todoStorage.uid = todos.length
    return todos
  },
  save: function (todos) {
    localStorage.setItem(STORAGE_KEY, JSON.stringify(todos))
  }
}
export default {
  name: 'TodoList',
  data () {
    return {
      cardTitleBeforeEdit: '',
      cardContentBeforeEdit: '',
      todoCardKey: 2,
      newTodo: '',
      title: 'TodoList',
      cards: [
        {
          id: 0,
          title: '標題1',
          editTitle: false,
          newItemContent: '',
          itemKeyCounter: 2,
          items: [
            {
              id: 0,
              content: '內容1',
              editContent: false,
              completed: false
            },
            {
              id: 1,
              content: '內容2',
              editContent: false,
              completed: false
            }
          ]
        },
        {
          id: 1,
          title: '標題2',
          editTitle: false,
          newItemContent: '',
          itemKeyCounter: 2,
          items: [
            {
              id: 0,
              content: '內容1',
              editContent: false,
              completed: false
            },
            {
              id: 1,
              content: '內容2',
              editContent: false,
              completed: false
            }
          ]
        }
      ]
    }
  },
  directives: {
    focus: {
      inserted: function (el) {
        el.focus()
      }
    }
  },
  watch: {// watch todos change for localStorage persistence
    cards: {
      handler: function (cards) {
        todoStorage.save(cards)
      },
      deep: true
    }
  },
  methods: {
    addTodo () {
      if (this.newTodo.trim().length === 0) {
        return
      }
      this.cards.push({
        id: this.todoCardKey,
        title: this.newTodo,
        items: [],
        itemKeyCounter: 0,
        newitemContent: '',
        editTitle: false
      })
      this.todoCardKey++
      this.newTodo = ''
    },
    removeContent (cardIndex, itemIndex) {
      this.cards[cardIndex].items.splice(itemIndex, 1)
    },
    removeCard (cardIndex) {
      this.cards.splice(cardIndex, 1)
    },
    addTodoContent (cardIndex) {
      if (this.cards[cardIndex].newItemContent.trim().length === 0) {
        return
      }
      this.cards[cardIndex].items.push({
        id: this.cards[cardIndex].itemKeyCounter,
        content: this.cards[cardIndex].newItemContent,
        editContent: false,
        completed: false
      })
      this.cards[cardIndex].itemKeyCounter++
      this.cards[cardIndex].newItemContent = ''
    },
    editTodoTitle (card) {
      card.editTitle = true
      this.cardTitleBeforeEdit = card.title
    },
    finishEditTodoTitle (card) {
      card.editTitle = false
    },
    cancelEditTodoTitle (card) {
      card.title = this.cardTitleBeforeEdit
      this.finishEditTodoTitle(card)
    },
    editTodoContent (item) {
      item.editContent = true
      this.cardContentBeforeEdit = item.content
    },
    finishEditTodoContent (item) {
      item.editContent = false
    },
    cancelEditTodoContent (item) {
      item.content = this.cardContentBeforeEdit
      this.finishEditTodoContent(item)
    },
    toggleContentCompleted (item) {
      item.completed = !item.completed
    }
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped lang='scss'>
.todo-list {
  margin: auto;
  width: 60%;
  border: 3px solid rgba(198, 216, 193, 0.788);
  border-radius: 25px;
}
.new-todo {
  width: 90%;
  padding: 10px 18px;
  font-size: 18px;
  margin-bottom: 16px;

  &:focus {
    outline: 0;
  }
}
.todo-list-cards {
  outline: 1px;
}
.todo-cards {
  text-align: left;
}
input{
  box-sizing:border-box
}
.todo-card {
  border: 3px solid rgba(84, 214, 106, 0.74);
  border-radius: 15px;
  margin: 5px;
}
.todo-title {
  display: inline-block;
  font-size: 20px;
  margin: 7px;
}
.todo-title-edit {
  font-size: 20px;
  margin: 7px;
}
.remove-card {
  float: right;
  margin-right: 10px;
  margin-top: 8px;
  cursor: pointer;
  &:hover {
    color: black;
  }
}
.todo-contents {
  margin-left: 15px;
}
.todo-content {
  display: inline-block;
}
.todo-content-edit {
  display: inline-block;
}
.remove-item {
  float: right;
  margin-right: 5px;
  cursor: pointer;
  &:hover {
    color: black;
  }
}
.new-content {
  font-size: 16px;
  margin-left: 13px;
}
.completed {
  text-decoration: line-through;
  color: gray;
}
</style>
