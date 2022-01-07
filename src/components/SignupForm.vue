<template>
    <form @submit.prevent="handleSubmit" ref="theform">
    <label>Email:</label>
    <input type="text" ref="email" v-model="email" @keypress="resetForm">
    <div class="errorMessage" v-if="error.email">Missing or wrong email address</div>
    <label>Password:</label>
    <input type="password" ref="password" v-model="password" @keypress="resetForm">
     <div class="errorMessage" v-if="error.password">Missing or wrong password, min 5 letters and numbers</div>
<label>Role</label>
<select v-model="role" ref="role" @click="resetForm">
    <option disabled value="">Select role</option>
    <option v-for="role in roleOptions" :value="role.value" :key="role.value">{{role.name}}</option> 
</select>
 <div class="errorMessage" v-if="error.role">Select a role</div>
<label>Skills</label>
<input type="text" ref="skills" placeholder="Enter a skill and press enter" v-model="tempskill" @keydown.enter="editskills">
 <div class="errorMessage" v-if="error.skills">Add your skills</div>
<div class="theskills" v-if="showSkills">
    <a href="#" class="skill" v-for="skill in skills" :key="skill" @click="removeskill">{{skill}}</a> 
</div>
<div class="terms" ref="terms">
    <input type="checkbox" v-model="terms" @click="resetForm">
    <label>Accept terms and conditions</label>
</div>
 <div class="errorMessage" v-if="error.terms">You must comply to terms and conditions</div>
<div class="submit">
    <button>Create an account</button>
</div>
  </form>

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
            error:{
                email:false,
                password:false,
                role:false,
                skills:false,
                terms:false,

            }
     
        }
    },
    methods:{
        handleSubmit(){
            const Vemail =  /^[\w\.]+@[a-zA-Z_]+?\.[a-zA-Z]{2,3}$/
          
            if(!Vemail.test(this.email)){
               this.handleErrors('email')
            }else if(this.password.length<6){
               this.handleErrors('password')
            }else if(!this.role || this.role===0){
                this.handleErrors('role')
            }else if(this.skills.length ===0){
                 this.handleErrors('skills')
            }else if(!this.terms){
                 this.handleErrors('terms')
            }else{
                const output = 'email: '+this.email+'\npassword: '+this.password+'\nrole: '+this.role+'\nskills: '+this.skills 
                alert(output)
            }

        },
        handleErrors(fieldID){
             this.$refs[fieldID].classList.add('error')
                this.$refs[fieldID].focus()
                this.error[fieldID] = true;
        },

        editskills(e){
            e.preventDefault()
            if(this.tempskill==='') return
            let check = this.skills.find((title)=>{
               return title ===this.tempskill
            })
          
           if(check) return
           this.resetForm()
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
        },
        resetForm(){
            let theform = this.$refs.theform;
           
            let i=0
            while(i<theform.length){
                console.log(theform[i])
                theform[i].classList.remove('error')
                i++
            }
            Object.keys(this.error).forEach(key => {
                this.error[key] = false
            });
            this.$refs.terms.classList.remove('error')
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
  input,select{
   outline:none;
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
  button{
      background: #0b6dff;
      border:0;
      padding:10px 20px;
      margin-top:20px;
      color:#fff;
      border-radius: 20px;
  }
  .submit{
      text-align: center;
  }
  .error{
      border:1px solid red;
  }
  .errorMessage{
      margin-top:3px;
      color: red;
       font-size: 0.9em;
  }
  
</style>