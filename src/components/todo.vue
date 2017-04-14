<template>
  <div>
    <div class="header wraper">
      <h2>todos</h2>
    </div>
    <div class="main wraper">
      <div class="main-input">
        <input type="checkbox" v-model="all">
        <input type="text" autofocus placeholder="What needs to be done?"
               v-model="msg"
               @keyup.enter="addTodo">
      </div>
      <ul>
        <li class="task"
            v-for="(val,index) in filteredList"
            @mouseover="getIndex(index)"
            @mouseout="clearIndex"
        >
          <input type="checkbox" v-model="val.isChecked">
          <label class="content" :class="{ complete : val.isChecked }">{{val.message}}</label>
          <button v-show="iNow === index"
                  @click="removeTodo(index)">X</button>
        </li>
      </ul>
    </div>
    <div class="footer wraper clearFix" v-show="list.length">
      <span class="fl">{{activeTodo.length}} item left</span>
      <ul class="fl">
        <li v-for="(val,index) in status" @click="changeVisibility(val,index)"><a href="javascript:;" :class="{ pick : index === statusIndex }">{{val}}</a></li>
      </ul>
    </div>
  </div>
</template>

<script>
let todos = JSON.parse(localStorage.getItem('list') || '[]');
export default {
  data () {
    return {
      list: todos,
      msg: '',
      iNow: -1,
      statusIndex: 0,
      status: ['All','Active','Completed'],
      visibility: 'All',
      all: false
    }
  },
  methods: {
    addTodo () {
      this.list.push(
        {message: this.msg,isChecked: false}
      );
      this.msg = '';
    },
    getIndex (index) {
      this.iNow = index;
    },
    clearIndex () {
      this.iNow = -1
    },
    removeTodo (index) {
      this.list.splice(index,1)
    },
    changeVisibility (val,index) {
      this.statusIndex = index;
      this.visibility = val;
    }
  },
  computed: {
    filteredList () {
      if(this.visibility === 'All'){
        return this.list.filter(function (val) {
          return true;
        });
      }
      if(this.visibility === 'Active'){
        return this.list.filter(function (val) {
          return !val.isChecked;
        });
      }
      if(this.visibility === 'Completed'){
        return this.list.filter(function (val) {
          return val.isChecked;
        });
      }
    },
    activeTodo () {
      return this.list.filter(function (val) {
        return !val.isChecked;
      })
    }
  },
  watch : {
    all: function (val) {
      if(val === true){
        this.list.map(function (val) {
          val.isChecked = true;
        })
      }
      if(val === false){
        this.list.map(function (val) {
          val.isChecked = false;
        })
      }
    },
    list: function () {
      localStorage.setItem('list',JSON.stringify(this.list));
    }
  }
}
</script>

<style>
  * {
    margin: 0;
    padding: 0;
  }
  .clearFix:after {
    display: block;
    content: '';
    clear: both;
  }
  .clearFix {
    zoom: 1;
  }
  li {
    list-style: none;
  }
  .fl {
    float: left;
  }
  .wraper {
    width: 550px;
    margin: 0 auto;
  }
  .header h2 {
    text-align: center;
    color: #f00;
    font-size: 100px;
    font-weight: 100;
  }
  .main {
    background: #fff;
  }
  .main-input {
    display: flex;
    align-items: center;
    width: 100%;
    height: 65px;
    border: 1px solid #ccc;
    padding-left: 10px;
  }
  .main-input input[type="text"] {
    flex: 1;
    padding: 16px 16px 16px 60px;
    border: none;
    outline: none;
    font-size: 24px;
  }
  .task {
    width: 100%;
    display: flex;
    align-items: center;
    border: 1px solid #ccc;
    padding-left: 10px;
  }
  .task .content {
    flex: 1;
  }
  .task label {
    display: block;
    line-height: 1.2;
    padding: 15px 15px 15px 15px;
    transition: color 0.4s;
  }
  .task .complete {
    text-decoration: line-through;
    color: #ccc;
  }
  .task button {
    width: 40px;
    height: 40px;
    color: #cc9a9a;
    border: none;
    background-color: #fff;
    outline: none;
    cursor: pointer;
  }
  .footer ul {
    margin-left: 50px;
  }
  .footer ul li {
    float: left;
    margin-left: 20px;
  }
  .footer ul .pick {
    border: 1px solid orange;
    border-radius: 30%;
  }
  .footer ul li a {
    text-decoration: none;
    color: #ccc;
    cursor: pointer;
  }
</style>
