<template>
    <form @submit.prevent="submit">
    <label>Email:</label>
    <input type="email" name="x_email" required v-model="email">
    <label>Password:</label>
    <input type="password" name="x_password" required v-model="password">
<label>Role</label>
<select v-bind="role">
    <option value="0">Select role</option>
    <option v-for="role in roleOptions" :value="role.value" :key="role.value">{{role.name}}</option> 
</select>
<label>Skills</label>
<input type="text" placeholder="Enter a skill and press enter" v-model="tempskill" @keydown.enter="editskills">
<div class="theskills" v-if="showSkills">
    <a href="#" class="skill" v-for="skill in skills" :key="skill" @click="removeskill">{{skill}}</a> 
</div>
<div class="terms">
    <input type="checkbox" v-model="terms" required>
    <label>Accept terms and conditions</label>
</div>
  </form>
  <p>{{skills}}</p>
</template>

<script>
export default {
    name:'SignupForm',
    data(){
        return{
             email:'',
             password:'',
            roleOptions:[{value:'webdesigner',name:'Web Designer'},{value:'webdeveloper',name:'Web Developer'}],
            role:'',
            terms:false,
            tempskill:'',
            skills:[],
            showSkills:false,
        }
    },
    methods:{
        submit(){
            return false
        },
        editskills(){
            if(this.tempskill==='') return
            let check = this.skills.find((title)=>{
               return title ===this.tempskill
            })
          
           if(check) return
            this.skills.push(this.tempskill)
            this.tempskill=''
            this.showSkills=true
        },
        removeskill(e){
               
            this.skills = this.skills.filter((i)=>{
                return i !==e.target.innerText;
            })
            if(this.skills.length===0){
                this.showSkills=false
            }    
        }
    }
}
</script>

<style>
form {
    max-width: 420px;
    margin: 30px auto;
    background: white;
    text-align: left;
    padding: 40px;
    border-radius: 10px;
  }
  label {
    color: #aaa;
    display: inline-block;
    margin: 25px 0 15px;
    font-size: 0.6em;
    text-transform: uppercase;
    letter-spacing: 1px;
    font-weight: bold;
  }
  input,select {
    display: block;
    padding: 10px 6px;
    width: 100%;
    box-sizing: border-box;
    border: none;
    border-bottom: 1px solid #ddd;
    color: #555;
  }
  input[type='checkbox']{
      position: relative;
      top:2px;
      margin:0 10px 0 0;
      width:16px;
      display: inline-block;
  }
  .theskills{
      background: #fff;
      border:1px solid #ddd;
      padding:10px;
      width:100%;
      height:100px;
  }
  .skill{
      margin-right:10px;
      text-decoration: none;
      border:1px solid #ddd;
      padding:5px;
      color:#333;
  }
  .skill::before{
      content:'X';
      color:red;
     margin-right:3px;   
  }
</style>