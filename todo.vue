<template>
  <div id="todoApp">
    <div class="add" v-show="pagechanger == 0">
      <nav>
        <div class="newList">
          <button v-bind:class="[value==1 ? '':'inNouse']" @click="toggle = !toggle">New List</button>
          <!-- form one -->
          <form @submit.prevent="onSubmit" v-show="toggle == true" ref="form">
            <label for="name">list name</label>
            <input type="text" name="name" v-model="listName" ref="list_name" required />
            <section class="btns">
              <!-- // Todo: disable submit button till onchange on input -->
              <button @click="cancle" type="button">Cancle</button>
              <button @click="onSubmit">Create list</button>
            </section>
          </form>
          <!-- end form one -->
        </div>
        <div id="todo_main_btns">
          <button @click="value = 1" v-bind:class="[value == 1 ? 'active':'']">Lists</button>

          <button @click="value=2" v-bind:class="[value == 2 ? 'active':'']">Today</button>
          <button @click="value=3" v-bind:class="[value == 3 ? 'active':'']">Schedule</button>
        </div>
        <div></div>
      </nav>
      <div id="body">
        <section v-if="value == 1" class="list_container">
          <div class="box_container">
            <!-- create reference to access elements -->
            <div v-for="(name,index) in names" class="box" ref="box_ref" :key="name.id">
              <div>
                <div @click="pagechangerfunc(name)" id="box">
                  <a v-show="show == true">{{index}}</a>
                </div>
                <div class="box_name" @click="pagechangerfunc(name)" ref="spec_title">
                  <span>{{name.name}}</span>
                </div>
              </div>
            </div>
          </div>
        </section>
        <!-- second section -->
        <section v-if="value == 2">
          <div class="body_second">
            <div class="body_second_container">
              <div class="todo_list_container">
                <div class="todos">
                  <form action>
                    <div id="displayed">
                      <div class="checked">
                        <input type="checkbox" />
                      </div>
                      <div class="nameholder">
                        <input type="text" />
                      </div>
                      <div class="time_dd">
                        <span></span>
                        <!-- <span>{{date}}</span> -->
                        <i @click="extendfunc($event,task)">
                          <svg viewBox="0 0 451 451" width="13">
                            <path
                              d="M225.923,354.706c-8.098,0-16.195-3.092-22.369-9.263L9.27,151.157c-12.359-12.359-12.359-32.397,0-44.751
		c12.354-12.354,32.388-12.354,44.748,0l171.905,171.915l171.906-171.909c12.359-12.354,32.391-12.354,44.744,0
		c12.365,12.354,12.365,32.392,0,44.751L248.292,345.449C242.115,351.621,234.018,354.706,225.923,354.706z"
                              fill="#000"
                            />
                          </svg>
                        </i>
                      </div>
                    </div>

                    <div class="toggled extension">
                      <div class="notes_container">
                        <label>Notes</label>
                        <textarea cols="35" rows="14"></textarea>
                      </div>
                      <div class="time_priority">
                        <div class="time_btns">
                          <label>Due Date</label>
                          <div id="style">
                            <button @click="datefunction($event,task)" type="button">Today</button>
                            <button @click="datefunction($event, task)" type="button">Tomorrow</button>
                            <datepicker
                              v-model="date"
                              lang="en"
                              type="date"
                              format="YYYY-MM-DD"
                              placeholder="no date set"
                              width="150"
                              @change="datefunction($event, task)"
                            ></datepicker>
                          </div>
                        </div>
                        <div class="priorities">
                          <label>Priority</label>
                          <select id="pcolor_selector" @change="checkPriority" v-model="pcolor">
                            <option value="none">None</option>
                            <option value="blue">Low</option>
                            <option value="orange">Medium</option>
                            <option value="red">High</option>
                          </select>
                        </div>
                        <div class="del">
                          <button type="button" @click="deleteTask">delete</button>
                        </div>
                      </div>
                    </div>
                  </form>
                </div>
              </div>
            </div>
            <div class="wrapper">
              <div class="todoMaker">
                <div class="adder">
                  <i @click="nameTodo(name)">plus</i>
                </div>

                <div class="namer">
                  <input
                    @keyup.enter="nameTodo(name)"
                    type="text"
                    id="todo_name"
                    v-model="taskname"
                    placeholder="New task..."
                    required
                  />
                </div>
                <div class="list_option">
                  <button type="button">
                    <span></span> Computer
                  </button>
                </div>
              </div>
            </div>
          </div>
        </section>
        <!-- third section -->
        <section v-if="value == 3">
          <div class="body_second">
            <div class="body_second_container">
              <div class="todo_list_container">
                <div class="todos">
                  <form action>
                    <div id="displayed">
                      <div class="checked">
                        <input type="checkbox" />
                      </div>
                      <div class="nameholder">
                        <input type="text" />
                      </div>
                      <div class="time_dd">
                        <span></span>
                        <!-- <span>{{date}}</span> -->
                        <i @click="extendfunc($event,task)">
                          <svg viewBox="0 0 451 451" width="13">
                            <path
                              d="M225.923,354.706c-8.098,0-16.195-3.092-22.369-9.263L9.27,151.157c-12.359-12.359-12.359-32.397,0-44.751
		c12.354-12.354,32.388-12.354,44.748,0l171.905,171.915l171.906-171.909c12.359-12.354,32.391-12.354,44.744,0
		c12.365,12.354,12.365,32.392,0,44.751L248.292,345.449C242.115,351.621,234.018,354.706,225.923,354.706z"
                              fill="#000"
                            />
                          </svg>
                        </i>
                      </div>
                    </div>

                    <div class="toggled extension">
                      <div class="notes_container">
                        <label>Notes</label>
                        <textarea cols="35" rows="14"></textarea>
                      </div>
                      <div class="time_priority">
                        <div class="time_btns">
                          <label>Due Date</label>
                          <div id="style">
                            <button @click="datefunction($event,task)" type="button">Today</button>
                            <button @click="datefunction($event, task)" type="button">Tomorrow</button>
                            <datepicker
                              v-model="date"
                              lang="en"
                              type="date"
                              format="YYYY-MM-DD"
                              placeholder="no date set"
                              width="150"
                              @change="datefunction($event, task)"
                            ></datepicker>
                          </div>
                        </div>
                        <div class="priorities">
                          <label>Priority</label>
                          <select id="pcolor_selector" @change="checkPriority" v-model="pcolor">
                            <option value="none">None</option>
                            <option value="blue">Low</option>
                            <option value="orange">Medium</option>
                            <option value="red">High</option>
                          </select>
                        </div>
                        <div class="del">
                          <button type="button" @click="deleteTask">delete</button>
                        </div>
                      </div>
                    </div>
                  </form>
                </div>
              </div>
            </div>
            <div class="wrapper">
              <div class="todoMaker">
                <div class="adder">
                  <i @click="nameTodo(name)">plus</i>
                </div>

                <div class="namer">
                  <input
                    @keyup.enter="nameTodo(name)"
                    type="text"
                    id="todo_name"
                    v-model="taskname"
                    placeholder="New task..."
                    required
                  />
                </div>
                <div class="list_option">
                  <button type="button">
                    <span></span> Computer
                  </button>
                </div>
              </div>
            </div>
          </div>
        </section>
      </div>
    </div>

    <!-- second view from the first section -->
    <div v-for="name in names" :key="name.id">
      <div class="schedule_from_list" id="list_page" v-show="name.is_in_use == true">
        <nav>
          <div class="back_btn">
            <a @click="name.is_in_use=false, pagechanger=0">back</a>
          </div>
          <div class="list-title">
            <span>
              <i>{{name.name}}</i>
            </span>
          </div>
          <div></div>
        </nav>

        <div class="todo_body_list">
          <div class="todo_list_container">
            <!-- to do item holder -->
            <div
              v-for="(task,index) in name.tasklist"
              class="todos"
              :id="'tasks' + index "
              ref="contain"
              :key="task.taskid"
            >
              <form :style="{'border-left-color':sidecolor }">
                <div id="displayed">
                  <div class="checked">
                    <input type="checkbox" />
                  </div>
                  <div class="nameholder">
                    <input type="text" :value="task.task_name" />
                  </div>
                  <div class="time_dd">
                    <span>{{task.dueDate}}</span>
                    <!-- <span>{{date}}</span> -->
                    <i @click="extendfunc($event,task)">
                      <svg viewBox="0 0 451 451" width="13">
                        <path
                          d="M225.923,354.706c-8.098,0-16.195-3.092-22.369-9.263L9.27,151.157c-12.359-12.359-12.359-32.397,0-44.751
		c12.354-12.354,32.388-12.354,44.748,0l171.905,171.915l171.906-171.909c12.359-12.354,32.391-12.354,44.744,0
		c12.365,12.354,12.365,32.392,0,44.751L248.292,345.449C242.115,351.621,234.018,354.706,225.923,354.706z"
                          fill="#000"
                        />
                      </svg>
                    </i>
                  </div>
                </div>
                <div v-show=" task.extend == true" :id="'extension_'+index " class="extension">
                  <div class="notes_container">
                    <label>Notes</label>
                    <textarea cols="35" rows="14"></textarea>
                  </div>
                  <div class="time_priority">
                    <div class="time_btns">
                      <label>Due Date</label>
                      <div id="style">
                        <button @click="datefunction($event,task)" type="button">Today</button>
                        <button @click="datefunction($event, task)" type="button">Tomorrow</button>
                        <datepicker
                          v-model="date"
                          lang="en"
                          type="date"
                          format="YYYY-MM-DD"
                          placeholder="no date set"
                          width="150"
                          @change="datefunction($event, task)"
                        ></datepicker>
                      </div>
                    </div>
                    <div class="priorities">
                      <label>Priority</label>
                      <select id="pcolor_selector" @change="checkPriority" v-model="pcolor">
                        <option value="none">None</option>
                        <option value="blue">Low</option>
                        <option value="orange">Medium</option>
                        <option value="red">High</option>
                      </select>
                    </div>
                    <div class="del">
                      <button type="button" @click="deleteTask">delete</button>
                    </div>
                  </div>
                </div>
              </form>
            </div>
            <!-- end the todo container -->
            <div class="todoMaker">
              <div class="adder">
                <i @click="nameTodo(name)">plus</i>
              </div>

              <div class="namer">
                <input
                  @keyup.enter="nameTodo(name)"
                  type="text"
                  id="todo_name"
                  v-model="taskname"
                  placeholder="New task..."
                  required
                />
              </div>
            </div>
          </div>
        </div>
      </div>
    </div>
    <!-- end section -->
  </div>
</template>

<script>
import Datepicker from "vue2-datepicker";
export default {
  data() {
    return {
      value: 1,
      toggle: false,
      listName: "",
      idForNextList: 2,
      taskname: "",
      nexttaskid: 1,
      counter: 0,
      // this stores each todo list with it's items
      names: [
        {
          id: 1,
          name: "computer",
          is_in_use: false,
          tasklist: [
            {
              id_task: 0,
              task_name: "Sample",
              extend: false,
              priority: "high",
              dateCreated: "",
              dueDate: ""
            }
          ]
        }
      ],
      pagechanger: 0,
      extended: false,
      // from here down are timing options and some styles
      time: "",
      date: "",
      pcolor: "",
      sidecolor: "",
      show: false
    };
  },
  components: {
    Datepicker
  },
  props: ["event"],
  components: {
    Datepicker
  },
  methods: {
    onSubmit() {
      //check if item is empty
      if (this.listName.trim().length == 0) {
        return;
      }
      // let holder = this.$refs.box_ref;
      let namer = this.listName;
      this.names.unshift({
        id: this.idForNextList,
        name: this.listName,
        is_in_use: false,
        tasklist: [{}]
      });
      this.toggle = false;
      this.$refs.list_name.value = "";
      this.listName = "";
      this.idForNextList++;
      // console.log(this.$refs.list_name);
    },
    cancle() {
      this.$refs.list_name.value = "";
      this.toggle = false;
    },
    pagechangerfunc(name) {
      name.is_in_use = true;
      console.log(name.id);
      if (name.is_in_use == true) {
        this.pagechanger = 1;
      } else {
        this.pagechanger = 0;
      }
    },
    nameTodo(name) {
      // if (name.tasklist.taskid == 0) {
      //   return;
      // }
      console.log(this.taskname);
      name.tasklist.push({
        id: 1,
        task_name: this.taskname,
        extend: false
      });
      console.log(this.names.tasklist);
      this.nexttaskid++;
      this.counter++;
      this.taskname = "";
      // console.log(this.myWidth);
    },

    checkPriority() {
      // let affected = this.$refs.contain;
      if (this.pcolor == "none") {
        // console.log(this.pcolor);
        this.sidecolor = "transparent";
      } else if (this.pcolor == "blue") {
        // console.log(this.pcolor);
        this.sidecolor = this.pcolor;
      } else if (this.pcolor == "orange") {
        // console.log(this.pcolor);
        this.sidecolor = this.pcolor;
      } else if (this.pcolor == "red") {
        // console.log(this.pcolor);
        this.sidecolor = this.pcolor;
      } else {
        // console.log("none");
        this.sidecolor = "none";
      }
    },
    deleteTask() {
      let namer = this.$refs.tNamer.value;
      this.taskNames.pop(namer);
    },
    closeAll() {
      // console.log(document.querySelector(".extension"));
      document.querySelector(".extension").classList.remove("dropdownClass");
    },
    extendfunc(e, task) {
      // this.closeAll();
      let others = document.querySelector(".extension");
      let parent = e.currentTarget.parentElement.parentElement;
      // console.log(others);
      // for (let i = 0; i < others.length; i++) {
      //   if (others[i] != parent.id) {
      //     others[i].classList.remove("dropdownClass");
      //   }
      // }
      parent.nextSibling.classList.toggle("dropdownClass");

      if (parent.nextSibling.classList.contains("dropdownClass")) {
        // console.log("1");
        task.extend = true;
      } else {
        // console.log("0");
        task.extend = false;
      }
    },
    datefunction(event, task) {
      // console.log(this.date);
      // console.log(Date.now);
      let any_other_day = this.date;
      // date for today
      let today = Date.now();
      let curentday = new Date(today);
      // date for tomorrow
      let day2 = new Date();
      let nextDay = day2.setDate(curentday.getDate() + 1);
      var tomorrowday = new Date(nextDay);
      //check event emitted and set date per event check
      if (event.currentTarget == undefined) {
        task.dueDate = any_other_day.toLocaleDateString();
      } else {
        if (event.currentTarget.innerHTML == "Today") {
          console.log(event.currentTarget);
          task.dueDate = curentday.toLocaleDateString();
        } else if (event.currentTarget.innerHTML == "Tomorrow") {
          console.log(event.currentTarget);
          task.dueDate = tomorrowday.toLocaleDateString();
        }
      }
    }
  }
};
</script>

<style  lang="scss">
body {
  margin: 0;
  padding: 0;
}
#todoApp {
  button {
    cursor: pointer;
    border: 1px solid transparent;
    padding: 8px 18px;
    color: #000;
    font-weight: bold;
    font-size: 14px;
    transition: all 500ms ease;
  }

  .add {
    nav {
      display: flex;
      position: relative;
      border: 1px solid rgba(0, 0, 0, 0.103);
      background-color: rgb(70, 144, 228);
      .newList {
        flex-grow: 1;
        position: relative;
        padding: 5px 0px 5px 10px;
        button {
          border-radius: 8px;
          background-color: #eee;
        }
        .inNouse {
          visibility: hidden;
        }
        form {
          background-color: #fff;
          border: 1px solid rgba(128, 128, 128, 0.2);
          border-radius: 5px;
          box-shadow: 5px 5px 8px rgba(0, 0, 0, 0.16);
          width: 300px;
          margin: auto;
          position: absolute;
          top: 43px;
          padding: 5px 15px;
          label {
            display: block;
            font-weight: 800;
            color: rgba(0, 0, 0, 0.5);
            text-transform: capitalize;
            margin-top: 5px;
          }
          input {
            width: 290px;
            padding: 5px 5px;
            border: 1px solid rgba(144, 144, 143, 0.2);
          }
          .btns {
            padding: 10px 0px;
            display: flex;
            justify-content: space-between;
            button {
              display: block;
              color: #fff;
              &:nth-child(1) {
                background-color: rgb(243, 84, 84);
              }
              &:nth-child(2) {
                background-color: rgb(70, 144, 228);
              }
            }
          }
        }
      }
      #todo_main_btns {
        flex-grow: 1;
        padding: 5px 0px 5px 10px;
        min-width: 1px;
        button {
          background-color: #eee;
          outline: none;
          &:nth-child(1) {
            border-top-left-radius: 8px;
            border-bottom-left-radius: 8px;
          }
          &:nth-child(2) {
            border-left-color: rgba(87, 85, 85, 0.335);
            border-right-color: rgba(87, 85, 85, 0.335);
          }
          &:nth-child(3) {
            border-top-right-radius: 8px;
            border-bottom-right-radius: 8px;
          }
          &:hover {
            background-color: rgb(184, 183, 183);
          }
        }
        .active {
          background-color: #666;
          color: #eee;
        }
      }
    }
  }
  #body {
    .list_container {
      .box_container {
        // background-color: red;
        border: 1px solid transparent;
        .box {
          // border: 1px solid blue;
          width: 70%;
          margin: 40px auto;
          display: flex;
          flex-wrap: wrap-reverse;

          #box {
            cursor: pointer;
            width: 180px;
            min-height: 210px;
            border: 1px solid transparent;
            margin: 10px 30px;
            padding: 10px;
            border-radius: 4px;
            background-color: rgb(128, 190, 241);
          }
          .box_name {
            cursor: pointer;
            margin: 5px 10px;
            text-align: center;
            span {
              font-size: 14px;
            }
          }
        }
      }
    }
  }
  #list_page {
    nav {
      border: 1px solid rgba(128, 128, 128, 0.1);
      display: flex;
      position: relative;
      padding: 10px 0px;
      background-color: rgb(70, 144, 228);
      .back_btn {
        padding-left: 10px;
        a {
          cursor: pointer;
        }
      }
      .list-title {
        text-align: center;
      }
      div {
        flex-grow: 1;
      }
    }
    .todo_body_list {
      width: 100%;
      .todo_list_container {
        width: 50%;
        margin: 30px auto;
        border: 1px solid transparent;
        .dropdownClass {
          background-color: red;
        }
      }
    }
  }
  .todos {
    border: 1px solid transparent;

    form {
      box-shadow: 3px 3px 8px rgba(0, 0, 0, 0.253);
      width: 70%;
      margin: 10px auto;
      border-radius: 3px;
      border: 1px solid transparent;
      border-left: 5px solid transparent;
      label {
        display: block;
        padding-bottom: 8px;
        font-weight: bold;
        color: rgb(4, 27, 4);
      }
      #displayed {
        display: flex;
        border: 1px solid transparent;
        // border-bottom-color: rgba(0, 0, 0, 0.2);
        align-items: center;
        position: relative;
        border-radius: 3px;
        .nameholder {
          padding: 10px 5px;
          flex-grow: 1;
          input {
            font-size: 14px;
            border: none;
            max-width: 400px;
          }
        }
        .time_dd {
          i {
            padding: 0px 5px;
          }
        }
      }

      .extension {
        display: flex;
        padding: 10px 15px;
        .notes_container {
          margin-right: 10px;
          textarea {
            resize: none;
            border: 1px solid rgba(0, 0, 0, 0.2);
          }
        }
        .time_priority {
          display: grid;
          .time_btns {
            // border: 1px solid salmon;
            #style {
              border: 1px solid rgba(0, 0, 0, 0.164);
              border-radius: 8px;
              button {
                padding: 3px 10px;
                background-color: rgb(255, 255, 255);
                color: black;
                display: inline;
                font-weight: lighter;
                outline: none;
                &:first-child {
                  border-top-left-radius: 8px;
                  border-bottom-left-radius: 8px;
                }
              }
              input {
                padding: 3px 0px;
                display: inline;
                border: 1px solid transparent;
                border-top-right-radius: 8px;
                border-bottom-right-radius: 8px;
                :read-write &::placeholder {
                  content: "hee";
                }
              }
            }
          }
          .priorities {
            // border: 1px solid salmon;
            text-align: left;
            justify-self: flex-end;
            select {
              width: 300px;
              background-color: #fff;
              padding: 3px 0px;
              border: 1px solid rgba(128, 128, 128, 0.253);
              border-radius: 8px;
              cursor: pointer;
              outline: none;
            }
          }
          .del {
            // border: 1px solid salmon;
            margin-bottom: 5px;
            align-self: end;
            justify-self: flex-end;
            button {
              width: 100px;
              padding: 5px 6px;
              border-radius: 10px;
              background-color: orangered;
            }
          }
        }
      }
    }
  }
  .todoMaker {
    display: flex;
    width: 70%;
    margin: 10px auto;
    border: 1px solid rgba(0, 0, 0, 0.2);
    box-shadow: 3px 3px 5px rgba(0, 0, 0, 0.253);
    align-items: center;
    position: relative;
    border-radius: 3px;
    .adder {
      i {
        color: purple;
        padding: 10px 1px;
      }
    }
    .namer {
      width: 99%;
      position: relative;
      object-fit: fill;
      input {
        border: 1px solid transparent;
        padding: 10px 0px 10px 5px;
        outline: none;
        width: 99%;
        font-size: 16px;
      }
    }
  }

  .body_second {
    // background: #000;
    width: 100%;
    .body_second_container {
      width: 50%;
      // background-color: red;
      margin: 30px auto;
    }
    .wrapper {
      width: 50%;
      margin: 10px auto;
    }
  }
}
</style>