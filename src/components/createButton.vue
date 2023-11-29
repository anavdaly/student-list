<script setup>

import { ref } from 'vue';
// import crypto from 'crypto-js';

let visible = ref(false);

function showInputs(params)
{
	// let inputDiv = document.getElementById("inputs");
	console.log(name.value);
	visible.value = !visible.value;
	// inputDiv.style.display = "block";
}

async function storeData()
{
	successMessage.value = "";
	errorMessage.value = "";
	if (!name.value || !lastname.value || !username.value || !password.value || !email.value) {
		errorMessage.value = 'Please fill in all required fields';
		return ;
	}

	// Check if password meets criteria
	if (password.value.length < 8) {
		errorMessage.value = 'Password should be at least 8 characters long';
		return ;
	}

	// Check if email is in correct format
		const emailRegex = /^\w+@[a-zA-Z_]+?\.[a-zA-Z]{2,3}$/;
		if (!emailRegex.test(email.value)) {
		  errorMessage.value = 'Invalid email format';
		  return;
		}

	let students = localStorage.getItem("students");

	if (!students)
		students = []
	else
		students = JSON.parse(students);

	// password = await bcrypt.hash(password, 10);
	// Store data in local storage
	// password = bcrypt.hashSync(password, salt);

	for (const i of students) {
		if (i.username == username.value)
		{
			errorMessage.value = "Username must be unique";
			return ;
		}
	}

	students.push({ name: name.value, lastname: lastname.value, username: username.value, password: password.value, email: email.value});
	localStorage.setItem('students', JSON.stringify(students));

	console.log("success");
	// Display success message
	successMessage.value = 'Profile created successfully';
	name.value = ""
	lastname.value = ""
	password.value = ""
	username.value = ""
	email.value = ""
}

// let inputDiv = ref(null);

let name = ref("");
let lastname = ref("");
let password = ref("");
let username = ref("");
let email = ref("")
let errorMessage = ref("");
let successMessage= ref("");

</script>

<template>

	<div class="container">
		<div class="create_student">
			<!-- <button class="button create" @click="showInputs()"> -->
				
				<h1>Create new profile</h1>
				
			<!-- </button> -->
		</div>
	
		<div class="inputs" id="inputs" ref="inputDiv">
			<input v-model="name" placeholder="Name" type="text" name="name" id="name">
			<input v-model="lastname" placeholder="Last Name" type="text" name="lastname" id="lastname">
			<input v-model="password" placeholder="Password" type="password" name="password" id="password">
			<input v-model="username" placeholder="Username" type="text" name="username" id="username">
			<input v-model="email" placeholder="email" type="text" name="email" id="email">
			
			<button class="button create_student" @click="storeData()">Create</button>
		</div>

		<div class="message"><h3>{{ errorMessage || successMessage }}</h3></div>
	</div>


</template>

<style scoped>

	.container {
		min-width: 80vh;
		min-height: 80vh;
		display: flex;
		flex-direction: column;
		align-items: center;
		justify-content: center;
	}

	 .button {
		color: white;
		padding: 5px;
		border: 2px solid white;
		border-radius: 10px;
		background-color: rgb(110, 140, 200);
	}

	.create:hover {
		background-color: rgb(144, 132, 212);
		cursor: pointer;
	}

	.inputs {
		display: flex;
		flex-direction: column;
	}

	.inputs .button {
		margin: 5px;
	}

	.inputs input {
		margin: 5px;
		padding: 5px;
		border-radius: 10px;
	}
</style>