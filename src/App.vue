<template>
  <div class="todoListBox">
    <div class="header-box">
      <div class="header-left">
        <h2>Todo List</h2>
      </div>
      <div class="header-right">
        <button class="headerAdd" @click="handleAdd">+</button>
      </div>
    </div>
    <div class="content">
      <div class="content-item"
           v-for="(item,index) in todoList" :key="item.id">
        <!-- 选中 , checkBox -->
        <div class="content-left" @click="handleSelect(index,item.id)">
          <span
              :style="item.isCheck ? 'opacity : 1' : 'opacity:0'"
          ></span>
        </div>
        <!--   input 输入框-->
        <input
            type="text"
            class="content-input"
            v-model="item.text"
            :disabled="item.isCheck"
            :class="item.isCheck ? 'line-through' : ''"
            @blur="handleBlur"
            ref="inputBox"
        >
        <!--  info -->
        <div class="content-right">
          <p>{{ item.time }}</p>
          <button @click="handleDelItem(index,item.id)">×</button>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import dayjs from "dayjs"
export default {
  name: 'App',
  data() {
    return {
      todoList: []
    }
  },
  created() {
    let getList = JSON.parse(window.localStorage.getItem("listTodo1"))
    if (getList === null) {
      this.todoList = [{
        id: this.randomID(),
        text: "请点击上方的添加按钮添加事件",
        isSelected: false,
        time: dayjs(new Date).format('YY-MM-DD HH:mm')
      }]
    } else {
      this.todoList = getList
    }
  },
  methods: {
    // 添加功能
    handleAdd() {
      this.todoList.unshift({
        id: this.randomID(),
        isCheck: false, // 是否选中
        text: "", // input 框内容
        time: dayjs(new Date).format("YY-MM-DD HH:mm"), // 时间
      })

      const inputLength = this.todoList.length - 1
      // 获取焦点
      this.$nextTick(() => {
        this.$refs.inputBox[inputLength].focus()
      })


    },
    //删除功能
    handleDelItem(index, id) {
      // console.log(index);
      // console.log(id);
      if (this.todoList[index].id === id) {
        this.todoList.splice(index, 1)
        this.storage()

      }
    },
    // 选中功能
    handleSelect(index, id) {
      // console.log(index);
      // console.log(id);
      if (this.todoList[index].id === id) {
        this.todoList[index].isCheck = !this.todoList[index].isCheck
        this.storage()

      }
    },
    //全选功能
    handleAllSelect() {
      this.todoList.forEach(item => {
        // console.log(item);
        // if (item.isCheck) return

        item.isCheck = !item.isCheck
        this.storage()

      })
    },
    // 判断是否输入完成
    handleBlur() {
      this.storage()
    },

    // 生成随机ID
    randomID() {
      return Number(Math.random().toString().substr(2, 0) + Date.now()).toString(10)
    },
    // 本地存储
    storage() {
      window.localStorage.setItem('listTodo1', JSON.stringify(this.todoList))
    }
  }
}
</script>



<style lang="less">
* {
  box-sizing: border-box;
  margin: 0;
  padding: 0;
  font-family: Helvetica,"PingFang SC","Microsoft Yahei",sans-serif;
}

h2 {
  margin: 24px 0;
  font-size: 28px;
  color: #414873;


}
// /* 整个页面 */
// main {
//   width: 100vw;
//   min-height: 100vh;
//   padding: 10vh 0;
//   display: grid;
//   align-items: start;
//   justify-items: center;
//   background: #d8dfff;
// }

body {
  background: rgb(203,210,240);
}

button {
  padding: 5px 10px;
  border: none;
  border-radius: 4px;
  color: #fff;
  margin-left: 10px;
}

.todoListBox {
  width: 60%;
  max-width: 400px;
  box-shadow: 0px 0px 24px rgba(0, 0, 0, 0.15);
  border-radius: 24px;
  padding: 48px 28px;
  background-color: rgb(245, 246, 252);



  // width: 300px;
  // height: 600px;
  // background: #e9ccd3;
  // border-radius: 10px;
  position: absolute;
  top: 50%;
  left: 50%;
  transform: translate(-50%, -50%);
  padding: 20px;
  box-sizing: border-box;
  // color: #fff;

  .header-box {
    display: flex;
    justify-content: space-between;
    align-items: center;
    border-bottom: #ccc 1px solid;

    .header-left {
      display: flex;
      align-items: center;

      div {
        width: 50px;
        height: 50px;
        border-radius: 50%;
        background: #d1c2d3;
        font-size: 30px;
        text-align: center;
        margin-right: 15px;
      }
    }

    .header-right {
      .headerAllSelect {
        background: #8076a3;
      }

      .headerAdd {
        width: 46px;
        height: 46px;
        border-radius: 50%;
        background: linear-gradient(#c0a5f3,#7f95f7);
        font-size: 25px;
      }
    }
  }

  .content {
    margin-top: 20px;
    height: 450px;
    overflow-y: scroll;

    .content-item {
      display: flex;
      justify-content: space-between;
      align-items: center;
      background: white;
      border-radius: 8px;
      padding: 10px 20px;
      box-sizing: border-box;
      margin-top: 20px;
      border-radius: 8px;
      color: #626262;

      .content-left {
        width: 30px;
        height: 30px;
        border: 1px solid #6b729c;
        border-radius: 50%;
        position: relative;

        span {
          display: inline-block;
          width: 20px;
          height: 20px;
          background: #6b729c;
          border-radius: 50%;
          position: absolute;
          top: 50%;
          left: 50%;
          transform: translate(-50%, -50%);
        }
      }


      .content-input {
        flex: 1;
        margin: 0 10px;
        outline: none;
        background: transparent;
        border: none;
        border-bottom: 1px solid rgb(132, 118, 118);
        padding: 5px 10px;
        color: #626262;
      }

      .line-through {
        color: #626262; // 文字颜色
        text-decoration: line-through #626262; // 横线颜色
      }

      .content-right {
        display: flex;
        align-items: center;
        font-size: 10px;

        button {
          background: #6b729c;
          font-size: 15px;
        }
      }
    }
  }
}
</style>
