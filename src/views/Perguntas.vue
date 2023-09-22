<script setup>
import {onMounted, ref, computed, watch} from 'vue'
import Messege from '../components/Messege.vue';
import { db } from '../firebase'
import { collection, getDocs, addDoc } from 'firebase/firestore'
//import globalService from 'globalService.js'
//import HomeView from './HomeView.vue';


let greeting = ref(true)
let isFinish = ref(false)
let isLast = ref(false)
const input_email = ref('')
const input_name = ref('')
let items = ref([])
let input_resposta = ref('')
let respostas = ref([])
let input_comentario = ref('')
let currentIndex = ref(0)
let enviar = ref([])
let msg = ref('')
const perguntas = ref([])
			/*{
			id: "0",
			pergunta: "Qual seu grau de satisfação com o nosso atendimento?",
			opcoes: ["Bom", "Muito Bom", "Ruim"],
			},
			{
			id: "1",
			pergunta: "Como você descreveria a experiencia de comprar conosco?", 
			opcoes: ["Bom", "Muito Bom", "Descepcionante"],
			},
			{
			id: "2",
			pergunta: "Você voltaria a comprar conosco?", 
			opcoes: ["Sim", "Talvez", "Não"],
			}	*/						
	

const nextQuestion = () =>{
     if (input_resposta.value === ('')) {
      return alert("Selecione uma resposta!")
     }
     if(currentIndex.value === perguntas.value.length -1 ){
         isLast.value = true
     }
     respostas.value.push(
        input_resposta.value
    )
    if(currentIndex.value < perguntas.value.length -1 ){   
        currentIndex.value ++
        input_resposta.value = ('')      
        loadQuestion ()      
     }  
    }

const loadQuestion = () =>{
     greeting.value = false
     items.value = perguntas.value[currentIndex.value]
     }

const finish = ()=> {
   addDoc (collection(db, 'respostas'), {
         nome: input_name.value,
			email: input_email.value,
			respostas: respostas.value,
			comentario: input_comentario.value
		})


   /*enviar.value.push({
      nome: input_name.value,
      email: input_email.value,
      respostas: respostas.value,
      comentario: input_comentario.value,   
   })*/
   isFinish.value = true
   msg.value = 'Suas respostas foram enviadas! Obrigado por participar.'
		setTimeout(() => {
		msg.value = (''),
		input_comentario.value = ('')     
      },
      3000)
}
      
onMounted(async() => {
	const querySnapshot = await getDocs(collection(db, 'perguntas'));
	const fireBasePerguntas = []
	querySnapshot.forEach((doc) => {
  	console.log(doc.id, " => ", doc.data());
	const perg = {
		id: doc.data().id,
		pergunta: doc.data().pergunta,
		opcoes: doc.data().opcoes
	}
	fireBasePerguntas.push(perg)
});
	perguntas.value = fireBasePerguntas
})
/*watch (
   items, ( newVal) =>{
   items.setItem (items(newVal))
})*/
/*const atualizarDados = (dados) =>{
      nome.value = dados.nome
      email.value = dados.email       
}*/

</script>


<template>
<!--<home-view @enviarDados="atualizarDados"/>-->   
<main class="app"> 
<div v-if="greeting">
<div class="titulo">Já vamos começar!</div>
<div class="bem-vindo">
   <span>Informe seu nome (Opcional)</span>
<input 
  type="text" 
  name="nome" 
  id="nome" 
  placeholder="seu nome aqui..."
  v-model="input_name" />

<span>Informe seu e-mail (Opcional)</span>
<input 
  type="email"
  name="email"
  id="email"		
  placeholder="seu e-mail aqui..."
  v-model="input_email" />
  
   <div class="btn">
      <input type="button" value="Começar" @click="loadQuestion()">
   </div>
   
</div>

</div>			


<div class="perguntas" v-if="!isLast && !greeting">
   <div>
        <h1>Pergunta {{ currentIndex + 1 }} de {{ perguntas.length }}</h1>
    </div>

    <div>
       <h3>{{ items.pergunta }}</h3> 
    </div>
    <div>
      <h4>Selecione uma opção abaixo (Obrigatório*)</h4>
    </div>  
    <div class="opcoes"  >        
         <label v-for="opcao, index in items.opcoes" :key="index">
           <div>
              <input
                type="radio" 
                name="category" 
                id="category"
                :value= "opcao"
                v-model="input_resposta">
                <span class="bubble"></span>
                </div> 
                
            <div>{{ opcao }} </div>                    
         </label>            
      </div>
      <div class="input-resposta">
         <h1 v-show="input_resposta">Resposta selecionada: {{ input_resposta }}</h1>
      
    </div>
   <span class="btn">
   <div>
      <input type="button" value="Próximo" @click="nextQuestion()"/>
   </div>   
   </span>
</div>
<div class="e-mail" v-if="isLast">
   <h1>Obrigado por chegar até aqui!</h1>
   <messege :msg="msg" v-show="msg"/>
   <h3>Deseja nos enviar um comentário?</h3>
   <h4>Enviar um comentário (Opcional*)</h4>
   
   <div class="cmt"> 
         <input 
         type="text" name="comentario" 
         id="cmt" 
         placeholder="Escreva seu comentário aqui..."
         v-model="input_comentario">
    </div>   

      
   <span class="btn">
   <div v-show="!isFinish">
      <input type="button" value="Finalizar" @click="finish()">
   </div>
 
   </span>
   <div v-show="isFinish">
      <button id="inicio" @click="$router.push('/')">Sair</button>
   </div>
   <div v-show="isFinish">    
      <router-link to="/Dashboard"
      style="
      display: block;
      font-size: larger; 
      color:rgb(3, 3, 179);
      text-align: center;
      margin-top: 10%;"
      >Visualizar respostas enviadas</router-link>
   </div>
</div>
</main>
</template>

<style scoped>
.titulo {
	display: block;
	text-align: center;	
	color: rgb(67, 83, 110);
	margin-bottom: 20px;
   font-weight: bold;
	font-size: 2rem;
	font-weight: 600;
   margin: 0 auto;
   width: 50% auto;
   padding: 30px 30px;

}
.bem-vindo span {
  display: block;
  flex-direction: auto;
  margin-bottom: 1.5rem;
  font-weight: bold;
  color: rgb(89, 89, 95);
  padding: 1.5rem 1.5rem;
  border-left: 4px solid #ffd21c;
  margin: 0 auto;
  width: 50%;
  margin-bottom: .5rem;
}

.bem-vindo input[type="email"] {
   display: block;
	width: 50%;
	font-size: 1.125rem;
	padding: 1rem 1.5rem;
	color: var(--escuro);
	background-color: #ffffff;
	border-radius: 0.5rem;
	box-shadow: var(--sombra);
   margin: 0 auto;
   margin-bottom: 0.5rem;
}

.bem-vindo input[type="text"] {
   display: flex;
	width: 50%;
	font-size: 1.125rem;
	padding: 1rem 1.5rem;
	color: var(--escuro);
	background-color: #ffffff;
	border-radius: 0.5rem;
	box-shadow: var(--sombra);
   margin: 0 auto;
   margin-bottom: 0.5rem;
}

h1 {
	display: block;
	text-align: center;	
	color: rgb(63, 79, 109);
	margin-bottom: 1rem;
   font-weight: bold;
	font-size: 1.8rem;
	font-weight: 550;
   margin: 0 auto;
   width: 50% auto;
   padding: 1.5rem;
}

h3 {
	color: rgb(75, 74, 74);
	font-size: auto;
	font-weight: 500;
	margin-bottom: 0.5rem;
   margin: 1rem;
   border-left: 4px solid #fdd63c;
   display: block;
   font-weight: bold;
   padding: 5px 10px;
   
}
h4 {
	color: var(--cinza);
	font-size: 0.775rem;
	font-weight: 700;
	margin-bottom: 0.5rem;
   margin: 1rem;
}
.btn input[type="button"] {
	display: block;
	width: 10% auto;
	font-size: 1.125rem;
	padding: 1rem 1.5rem;
	color: #ffffff;
	background-color: rgb(41, 41, 252);
   box-shadow: 0 0 0.5em rgba(18, 15, 175, 0.671);
	border-radius: 0.5rem;
	cursor: pointer;
	transition: 0.2s ease-in-out;
   margin: 0 auto;
   margin-top: 3.5rem;
}

.btn input[type="button"]:hover {
	opacity: 0.75;
	background-color: #96959b;
}
#inicio {
   display: block;
   text-align: center;
	width: 20% auto;
   margin: 0 auto;
	font-size: 1.225rem;
	padding: 0.8rem 2.5rem;
	color: #ffffff;
	background-color: rgb(41, 41, 252);
	border-radius: 0.5rem;
	cursor: pointer;
	transition: 0.2s ease-in-out;
	margin-top: 3.5rem;
   box-shadow: 0 0 0.5em rgba(18, 15, 175, 0.671);
   transition: box-shadow .5s;
}
 input[type="text"] {
   display: flex;
	width: 95%;
	font-size: 1.125rem;
	padding: 3rem 1.5rem;
	color: var(--escuro);
	background-color: #ffffff;
	border-radius: 1.5rem;
	box-shadow: var(--sombra);
	margin-bottom: 2rem;
   margin: 0 auto;
}
.input-resposta h1 {
   
   font-size: 15px;
   color: #01c20a;
   background-color: rgba(157, 255, 165, 0.192);
   border: 2px solid rgba(150, 253, 150, 0.644);
   width: 80%;
   padding: 2.5px;
   border-radius: 0.5rem;
   height: 30px;
   position: fixed;
   justify-content: center;
   margin-left: 50px;
   
}

</style>
