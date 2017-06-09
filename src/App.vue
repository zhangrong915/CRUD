<template>
  <div id="app">
      <div class="form">
        <form>
            <div class="no">
                <label><input type="text" v-model="no" name="no" id="no" placeholder="请输入学号" @blur="event=>this.checkValidity(event)"></label>
                <div class="hint" v-show="Flag.noFlag1">注意：学号不能为空</div>
                <div class="hint" v-show="Flag.noFlag2">注意：学好要求唯一，你的学号重复了</div>
            </div>
            <div class="name">
                <label><input type="text" v-model="name" name="name" id="name" placeholder="请输入姓名" @blur="event=>this.checkValidity(event)"></label>
                <div class="hint" v-show="Flag.nameFlag">注意：姓名不能为空</div>
            </div>
            <button type="button" @click="submitValue">提交</button>
        </form>
      </div>

      <div class="table">
          <table>
            <thead>
              <tr>
                <th>编号</th>
                <th>学号</th>
                <th>姓名</th>
                <th>操作</th>
              </tr>
            </thead>
            <tbody>
              <template>
                  <tr v-for="(student,index) in students">
                    <td>{{index}}</td>
                    <td v-if="student.no.length<3">{{student.no}}</td>
                    <td v-else >{{student.no.split('').splice(student.no.length-3).join('')}}</td>
                    <td>{{student.name}}</td>
                    <td>
                        <button type="button" @click="deleteStudent(index)">删除</button>
                        <button type="button" @click="editButton(index)">编辑</button>
                    </td>
                    <td><label><input type="text" v-model="editName" v-show="student.editFlag" @keyup.enter="edit(index)" name="editName" id="editName" placeholder="请修改姓名" /></label></td>
                  </tr>
              </template>
             </tbody>
          </table>
      </div>
  </div>
</template>

<script>
export default {
  name: 'app',
  data () {
    return {
      Flag:{
          noFlag1:false,
          noFlag2:false,
          nameFlag:false
      },
      students:[
          {no:'201417018802001',name:'Vivi',editFlag:false},
          {no:'2014170102002',name:'张三',editFlag:false}
      ],
      no:'',
      name:'',
      editName:''
    }
  },
  methods:{
    submitValue:function(){  
      let noValue=this.no.trim();
      let nameValue=this.name.trim();
      if(!noValue||!nameValue){
        return;
      }
      this.students.push({no:noValue,name:nameValue,editFlag:false});
      this.no='';
      this.name='';
    },
    checkValidity:function(e){
      let target=e.target.name;
      let no=this.no.trim();
      let name=this.name.trim();
      if(target==='no'){
         this.Flag.noFlag1=no?false:true;
         let onlyValue=this.checkOnlyNo(no);
         this.Flag.noFlag2=onlyValue?false:true;
      }
      if(target==='name'){
          this.Flag.nameFlag=name?false:true;
      }
    },
    checkOnlyNo:function(no){
      for(let i=0,l=this.students.length;i<l;i++){ 
        if(this.students[i].no===no){//no不唯一
          return false;
        }
      }
       return true;
    },
    deleteStudent:function(index){
      this.students.splice(index,1);
    },
    editButton:function(index){
      this.students[index].editFlag=true;
    },
    edit:function(index){
      let name=this.editName;
      this.students.splice(index,1,{no:this.students[index].no,name:name,editFlag:false});
      this.editName='';
    }
  }
  
}
</script>



<style>
.hint{
  color:red; 
}
table tr th,
table tr td{
  padding:3px 10px;
}
</style>
