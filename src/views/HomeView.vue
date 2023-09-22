<script setup>
import {ref, computed, onMounted} from 'vue'




</script>

<template>


<form id="teste">


<h1 class="titulo"> 
	<h5 style="
	font-size: 80px; 
	color: rgb(235, 219, 0);
	padding: 30px;
	font-family: Verdana, Geneva, Tahoma, sans-serif;
	">
	olá,</h5>
	Seja bem-vindo, queremos saber sua opnião!</h1>

    <h4>Sua opinião é muito importante para para que possamos melhorar nossos serviços, click abaixo para participar!</h4>

			
</form>
<div class="router">
 
  
<button type="button" id="perg" @click="$router.push('Perguntas')">Entrar</button>

<!--<button @click="$emit('enviarDados', {nome: input_name, email: input_email})">evento</button>-->
 
</div>
<RouterView/>

  <main>
 
  </main>
</template>
<style scoped>
h4 {
	color: rgb(124, 124, 124);
	font-size: 0.822rem;
	font-weight: 700;
	margin-bottom: 0.5rem;
    margin: 1.225rem;
	text-align: center;
}
.titulo {
	display: block;
	text-align: center;	
	color: rgb(67, 83, 110);
	margin-bottom: 20px;
  	font-weight: bold;
	font-size: 1.8rem;
	font-weight: 600;
 	margin: 0 auto;
  	width: 50% auto;
  	padding: 30px 30px;

}

#perg {
  	display: block;
  	text-align: center;
	width: 20% auto;
  	margin: 0 auto;
	font-size: 1.125rem;
	padding: 1rem 1.5rem;
	color: #ffffff;
	background-color: rgb(47, 47, 226);
	box-shadow: 0 0 0.5em rgba(18, 15, 175, 0.671);
	border-radius: 0.5rem;
	cursor: pointer;
	transition: 0.2s ease-in-out;
	margin-top: 3.5rem;

}

</style>
