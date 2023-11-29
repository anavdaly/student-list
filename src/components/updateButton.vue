<script setup>
import { ref } from "vue";

function showInputs(selectedStudent) {
  student.value = selectedStudent;
  visible.value = !visible.value;
}

function updateData() {
  successMessage.value = "";
  errorMessage.value = "";
  if (
    !name.value ||
    !lastname.value ||
    !username.value ||
    !password.value ||
    !email.value
  ) {
    errorMessage.value = "Please fill in all required fields";
    return;
  }

  // Check if password meets criteria
  if (password.value.length < 8) {
    errorMessage.value = "Password should be at least 8 characters long";
    return;
  }

  // Check if email is in correct format
  const emailRegex = /^\w+@[a-zA-Z_]+?\.[a-zA-Z]{2,3}$/;
  if (!emailRegex.test(email.value)) {
    errorMessage.value = "Invalid email format";
    return;
  }

  let students = localStorage.getItem("students");

  if (!students) {
    errorMessage.value = "No students found";
    return;
  }

  students = JSON.parse(students);

  // Find the index of the student to update
  const index = students.findIndex(
    (student) => student.username === username.value
  );
  if (index === -1) {
    errorMessage.value = "Student not found";
    return;
  }

  // Update the student's data
  students[index].name = name.value;
  students[index].lastname = lastname.value;
  students[index].password = password.value;
  students[index].email = email.value;

  // Save the updated students data back to local storage
  localStorage.setItem("students", JSON.stringify(students));

  console.log("updated");
  // Display success message
  successMessage.value = "Profile updated successfully";
  name.value = "";
  lastname.value = "";
  password.value = "";
  username.value = "";
  email.value = "";
}

let name = ref("");
let lastname = ref("");
let password = ref("");
let username = ref("");
let email = ref("");
let errorMessage = ref("");
let successMessage = ref("");
</script>

<template>
  <div class="container">
    <div class="create_student">
      <h1>Update profile</h1>
    </div>
    <div class="inputs update" id="update" ref="updateDiv">
      <input
        v-model="name"
        placeholder="Name"
        type="text"
        name="name"
        id="name"
      />
      <input
        v-model="lastname"
        placeholder="Last Name"
        type="text"
        name="lastname"
        id="lastname"
      />
      <input
        v-model="password"
        placeholder="Password"
        type="password"
        name="password"
        id="password"
      />
      <input
        v-model="username"
        placeholder="Username"
        type="text"
        name="username"
        id="username"
      />
      <input
        v-model="email"
        placeholder="email"
        type="text"
        name="email"
        id="email"
      />

      <button class="button update_student" @click="updateData()">
        Update
      </button>
    </div>

    <div class="message">
      <h3>{{ errorMessage || successMessage }}</h3>
    </div>
  </div>
</template>

<style scoped>

	.container {
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
