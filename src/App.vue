<template>
    <div class="container">
      <div class="row mt-5">
        <h1 class="display-4 m-auto font-weight-bold">To Do List</h1>
      </div>
      <form @submit.prevent="submitTodoItem">
        <div class="row mt-4">
            <input v-model="newTodo" type="text" class="form-control m-auto" placeholder="What to do?" autofocus> 
        </div>
      </form>

        <button class="btn btn-danger mt-3 float-right" @click.prevent="clearList">Clear List</button> 
      
      <div class="row pt-5">
        <ul class="list">
          <li class="item" v-for="item in listItems" :key="item.id">
            
              <input class="checkmark" type="checkbox"  :checked=item.done @change="checkmark(item)" />
              <a class="icon editNote" @click.prevent="updateListItem(item)">
                  <i class="fas fa-edit"></i>
                </a>
                <a class="icon removeNote" @click.prevent="removeListItem(item)">
                  <i class="fas fa-trash"></i>
                </a>

                <span>{{item.note}}</span>
                
                
          </li>
        </ul>
      </div>
    </div>
</template>

<script>


export default {
  name: 'App',
  data(){
    return{
        listItems:[],
        newTodo:'',
    }
  },
  mounted(){
    if(localStorage.listItems)
    {
      this.listItems = JSON.parse(localStorage.listItems);
    }
  },
  methods:{
    setList(){
      localStorage.listItems = JSON.stringify(this.listItems);
    },
    submitTodoItem(){
        this.listItems.push({
          note:this.newTodo,
          done:false
        });
        this.setList();
        this.newTodo = '';
    },
    removeListItem(item)
    {
        if(confirm("Are you sure to remove note?"))
        {
          const itemIndex = this.listItems.indexOf(item);
          this.listItems.splice(itemIndex,1);
          this.setList();
        }
    },
    updateListItem(item)
    {
      this.newTodo = item.note;
      // this.listItems.push({
      //     note:this.newTodo,
      //     done:false
      // });
      const itemIndex = this.listItems.indexOf(item);
      this.listItems.splice(itemIndex,1);
      this.setList();
    },
    checkmark(item){
      item.done = !item.done;
      this.setList();
    },
    clearList(){
      if(confirm("Do you want to clear list?"))
      {
        localStorage.clear(this.listItems);
        this.listItems = [];
        this.setList();
      }
    },
    
    
  }
}
</script>

<style>
*{
  padding:0;
  margin:0;
  font-family: montserrat;
}
.list{
  margin: auto;
  width: 100%;
}
.item{
  border: none;
  padding:30px;
  box-shadow: 1px 1px 5px 2px rgb(88, 70, 37);;
  list-style-type: none;
  margin-top: 20px;
  background: rgb(235, 222, 196);
}
.item span{
  text-align: center;
  margin-left: 20px;
  font-size: 20px;
}
.icon{
  cursor: pointer;
  float: right;
  font-size: 25px;
}
.removeNote, .removeNote:hover{
    color: rgb(219, 72, 72);
}
.editNote, .editNote:hover{
    color: rgb(47, 79, 221);
}
.editNote{
  color: rgb(47, 79, 221);
  margin-left: 10px;
}
.checkmark{
  width: 30px;
  height: 30px;
  color: green;
}
</style>
