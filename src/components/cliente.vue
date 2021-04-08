<template>
<div>
   <h1>{{ msg }}</h1>
   <form>
  <div class="form-row">

   <div class="form-group">
      <label for="id">id</label>
      <input type="int" class="form-control" id="id" >
    </div>

  <div class="form-group">
      <label for="lavaP">Lavar o Prato</label>
      <input type="text" class="form-control" id="lavaP" >
    </div>
  

 <div class="form-group">
    <label for="varreC">Varrer a casa</label>
    <input type="text" class="form-control" id="varreC" >
  </div>

  <div class="form-group">
    <label for="banheiro">banheiro </label>
    <input type="text" class="form-control" id="banheiro" >
  </div>

  <div class="form-group">
      <label for="data">data</label>
      <input type="date"  class="form-control" id="data" >
  </div>
   <br/>
  
   <div class="form-group">
      <label for="feito">Feito  </label>
      <input type="bit"  id="feito">
        <br/>
   
</div>  

     <br/>

<div>
  <button v-on:click="salvar()" type="button" class="btn btn-primary">Salvar</button>
   
</div>
<br/>

<div>
   <button  v-on:click="alterar()" type="button" class="btn btn-danger">Salvar Alterações</button>
</div>
</div>
</form> 
<br/>
<br/>
     <table  class ="table">
       <thead>
         <tr class ="table table-dark"> 

           <th scope="col">ID</th>          
          <th scope="col">FEITO</th>
          
          <th scope="col">LAVA PRATO</th>
          <th scope="col">VARRE CASA</th>
          <th scope="col">BANHEIRO</th>
          <th scope="col">DATA</th>
           <th colspan="1"></th>
         </tr>
       </thead>
       <tbody>
         <tr v-for="pessoa in pessoas" v-bind:key="pessoa.id">
            <td>{{pessoa.id}}</td>
          <th scope="row"> 

            <div v-if="pessoa.feito==true">
               <input type="checkbox" checked="checked" id="checkbox" >
            </div>
            <div v-else>
              <input type="checkbox"  id="checkbox">
            </div>
           
          </th> 
         
            <td>{{pessoa.lavPrato}}</td>
            <td>{{pessoa.varreC}}</td>
            <td>{{pessoa.banheiro}}</td>
            <td>{{pessoa.dataTask}}</td>

            <td>
              <button class = "btn btn-danger" v-on:click="editar(pessoa)">Editar</button>
            </td>
            <div>
 
</div>
         </tr>
       </tbody>
     </table>
 
</div>


    
</template>

<script>
import axios from 'axios'

let  scopoCliente =null
export default {
  name: 'pessoas',
 
  props: {
    msg: String,
    
     
  },
  data:() =>
  {
    return{
      pessoas: [],
      pessoa:undefined,
      

    }
  },
  
  methods: 
  {
    lista: () =>{
      /*traz tarefa do dia
        axios.get(`http://192.168.10.17:5000/api/pessoa/dia`).then((res)=>
     //====================================================================*/


     //traz todas as tarefas do banco
     
      axios.get(`http://192.168.10.17:5000/api/pessoa`).then((res)=>
      {
        console.log(res)
        scopoCliente.pessoas = res.data
        
      })
    },
    salvar:() =>{
    


      axios.post(`http://192.168.10.17:5000/api/pessoa`,
      {
         lavPrato: document.getElementById("lavaP").value ,
         varreC: document.getElementById("varreC").value,
         banheiro:  document.getElementById("banheiro").value,
        dataTask:  document.getElementById("data").value,
        feito:   document.getElementById("feito").value
        
      }).then(()=>{
            scopoCliente.lista()
              alert("ENVIADO")
      })
     
      
    },
    editar(pessoa){
       
       document.getElementById("lavaP").value =pessoa.lavPrato 
       document.getElementById("varreC").value=pessoa.varreC
       document.getElementById("banheiro").value=pessoa.banheiro        
       document.getElementById("data").value=pessoa.dataTask
       document.getElementById("data").valie=pessoa.feito
       document.getElementById("id").value=pessoa.id  
       this.pessoa = pessoa
    },
    alterar (){

    
     this.pessoa.lavPrato = document.getElementById("lavaP").value 
     this.pessoa.varreC =   document.getElementById("varreC").value
     this.pessoa.banheiro=  document.getElementById("banheiro").value  
      this.pessoa.dataTask= document.getElementById("data").value
      this.pessoa.feito=    document.getElementById("feito").value
      this.pessoa.id=        document.getElementById("id").value            

      axios.put(`http://192.168.10.17:5000/api/pessoa`,this.pessoa).then(()=>{
            this.lista()            
              alert("ENVIADO")
      })
        

    },
  

  }, 
  created() {
    scopoCliente = this
    this.lista(this)
  }
}

</script>