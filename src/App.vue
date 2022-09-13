<template>
  <div class="container">
    <div class="index">
      <ul>
        <li class="memo-list" v-for="memo in getAllMemos()" :key="memo.id">
          <a @click="edit(memo)"><li>{{ getFirstLine(memo.body) }}</li></a>
        </li>
      </ul>
      <button class="create-button" @click="create">+</button>
    </div>
    <div class="form">
      <from v-if="show()">
        <textarea class="textarea" v-model="body"></textarea>
          <div>
            <input class="update-button" @click="update" type="submit" value="編集">
            <button class="destroy-button" @click="destroy">削除</button>
          </div>
      </from>
    </div>
  </div>
</template>

<script>
export default {
  name: 'App',
  data () {
    return {
      id: '-1',
      body: ''
    }
  },

  methods: {
    getAllMemos () {
      return Object.keys(localStorage).map(key => { return { id: key, body: localStorage.getItem(key) } })
    },
    $_new () {
      this.id = new Date().getTime().toString()
      this.body = '新規メモ'
    },
    create () {
      this.$_new()
      localStorage.setItem(this.id, this.body)
    },
    show () {
      return this.id !== '-1'
    },
    edit (params) {
      this.id = params.id
      this.body = localStorage.getItem(this.id)
    },
    update () {
      localStorage.setItem(this.id, this.body)
      this.$_clear()
    },
    destroy () {
      localStorage.removeItem(this.id)
      this.$_clear()
    },
    $_clear () {
      this.id = '-1'
      this.body = ''
    },
    getFirstLine (text) {
      return text.split(/\n/)[0]
    }
  }
}
</script>

<style>
.container {
  display: flex;
  justify-content: space-evenly;
}
.memo-list {
  list-style: none;
}

.textarea {
 height: 500px;
 width: 500px;
}

.update-button {
 width: 410px;
 margin-right: 30px;
}

.destroy-button {
 width: 60px;
}

.create-button {
  margin-left: 40px;
  width: 65px;
}

.index {
 width: 200px;
}

.form {
  padding-top: 15px;
  width: 500px;
}
</style>
