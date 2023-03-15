<template>
  <div class="container w-25">
    <Header></Header>
    <List :provideItem="provideItem">
      <template v-slot:list="{todo:todoItem}" >
        <li class="list-group-item d-flex justify-content-between align-items-center" :key="todoItem.id">
          <span>{{ todoItem.content }}</span>
          <div>
            <button class="btn btn-sm btn-success me-2" @click="updateItem(todoItem)">Güncelle</button>
            <button class="btn btn-sm btn-danger" @click="deleteItem(todoItem)">Sil</button>
          </div>
        </li>
      </template>
    </List>
    <ListFooter :provideItem="provideItem" ></ListFooter>
  </div>
</template>

<script>
import Header from './components/Header.vue';
import List from './components/List.vue';
import ListFooter from './components/ListFooter.vue';

import axios from 'axios';
export default {
  name: 'App',
  components: {
    Header,
    List,
    ListFooter,
  },
  data(){
    return {
      provideItem :{
        list : [],
        updateItem : {
          content : '',
          id : null,
        }, 
      }  
    }
  },
  methods:{
    addItem(event){
      let createdObject = {content : this.provideItem.updateItem.content, isComplated : false}
      if(this.provideItem.updateItem.id === null){
        axios.post('http://localhost:3000/items/',createdObject).then(response=>{
          console.log(response );
           axios.get('http://localhost:3000/items').then(resonse =>{
            this.provideItem.list = resonse.data;
           })
        }).catch(error =>{
        console.log('Ekleme işlemi başarısız oldu, error : ',error);
        })
      }
      else{
        axios.patch(`http://localhost:3000/items/${this.provideItem.updateItem.id}`,createdObject).then(response => {
          console.log(response);
          this.provideItem.list = this.provideItem.list.map( item => {
            if(item.id === this.provideItem.updateItem.id){
              return {...item, ...createdObject};
            }
            else{
              return item;
            }
          })
          this.provideItem.updateItem.id = null;
        }).catch(error=>{
          console.log('Update sırasında hata meydana geldi, error : ',error);
        })
      }

      this.provideItem.updateItem.content = '';
      event.target.focus();
    },
    updateItem(item){
      this.provideItem.updateItem.content = item.content;
      this.provideItem.updateItem.id = item.id;
    },
    deleteItem(item){
      console.log(item);
        axios.delete(`http://localhost:3000/items/${item.id}`).then(response=>{
          console.log(response);
          this.provideItem.list = this.provideItem.list.filter(todoItem => todoItem.id !== item.id);
        }).catch( error => {
        console.log('Silme işlemi başarısız oldu, error : ',error);
      });
      
    }
  },
  provide(){
    return {
      addItem : this.addItem,
      provideItem : this.provideItem,
    }
  },
  mounted(){
    axios.get('http://localhost:3000/items').then(response =>{
        this.provideItem.list = response.data;
    });
    
  }
}
</script>
