<script setup>
import{ref, onMounted} from 'vue'
import { db } from '../firebase'
import { collection, getDocs, addDoc } from 'firebase/firestore'


const respostas= ref([])
   /* {
    nome: 'ismael',
    email: "ismaelrodrigoraiol@gmail.com",
    respostas: ["Bom", "Muito bom", "excelente"],
    comentario: "Muito bom"},
    {
    nome: "Maria",
    email: "maria@gmail.com",
    respostas: ["Muito bom", "excelente", "bom"],
    comentario: "ótimo serviço"},
    {
    nome: "João",
    email: "joao@gmail.com",
    respostas: ["regular", "Sim", "Talvez"],
    comentario: "Pode melhorar"
    }*/

onMounted(async() => {
	const querySnapshot = await getDocs(collection(db, 'respostas'));
	const fireBaseRespostas = []
	querySnapshot.forEach((doc) => {
  	console.log(doc.id, " => ", doc.data());
	const resp = {
		id: doc.data().id,
		nome: doc.data().nome,
        email: doc.data().email,
        respostas: doc.data().respostas,
        comentario: doc.data().comentario
		
	}
	fireBaseRespostas.push(resp)
});
	respostas.value = fireBaseRespostas
})


</script>

<template>
    <h1>Total de usuários: {{ respostas.length }}</h1>
    <div v-for="resposta, index in respostas" :key="index">
          <section class="respostas">
            <div>
            Nome: {{ resposta.nome }}
          </div>
          
          <div>
            E-mail: {{ resposta.email }}
         </div>
         <div>
            respostas: {{ resposta.respostas }}
         </div>
         <div>
            Comentário: {{ resposta.comentario }}
            </div>
       </section>    
    </div>

    <div>
      <button id="btn-sair" @click="$router.push('/')">Sair</button>
   </div>



</template>


<style scoped>
#btn-sair {
    display: flex;
    text-align: center;
    width: 20% auto;
    margin-left: 10px;
	font-size: 1.225rem;
	padding: 0.8rem 2.5rem;
	color: #ffffff;
	background-color: rgba(41, 41, 252, 0.932);
	cursor: pointer;
	margin-top: 1rem;
    transition: box-shadow .5s;
}

.respostas{
    display: flex;
    background-color: #ffffff;
    margin-top: 5px;
    font: bold;
    margin-left: 10px;
}
.respostas div{
    display: block;
   
}
</style>