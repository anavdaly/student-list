<script setup>
	import { ref } from 'vue';
	import search from './components/searchButton.vue'

	let students = localStorage.getItem("students");
	let backup;

	if (!students)
	{
		students = [];
		backup = [];
	}
	else
	{
		backup = JSON.parse(students);
		students = JSON.parse(students);
	}

	for (let i = 0; i < students.length; ++i) {
		let { password, ...new_obj } = students[i];

		students[i] = new_obj;
		backup[i] = new_obj;
	}

	let student_arr = ref(students);

	function search_students(searchValue)
	{
		if (searchValue == "")
		{
			restore();
			return ;
		}

		let values = searchValue.split(' ');
	
		for (let i = 0; i < student_arr.value.length; i++)
		{
			student_arr.value[i] = {};
		}

		for (const i in backup)
		{
			let name = backup[i].name.toLowerCase();
			let lastname = backup[i].lastname.toLowerCase();

			if (!values[1])
			{
				if (name.includes(values[0].toLowerCase()))
				{
					student_arr.value[i] = JSON.parse(JSON.stringify(backup[i]));
				}
			}
			else
			{
				if (name.includes(values[0].toLowerCase()) && lastname.includes(values[1].toLowerCase()))
				{
					student_arr.value[i] = JSON.parse(JSON.stringify(backup[i]));
				}
			}
		}
	}

	function restore()
	{
		for (const i in backup)
		{
			student_arr.value[i] = backup[i];
		}
	}
</script>

<template>
	<div class="container">
		<div class="student_list">
			<a href="add/">Create</a>
			<a href="update/">Update</a>
		</div>

		<div class="list">
			<table>
				<tr>
					<th>Name</th>
					<th>Last Name</th>
					<th>Username</th>
					<th>Email</th>
				</tr>
				<tr v-for="item in student_arr" :key="item">
					<td v-for="i in item" :key="i">
						{{ i }}
					</td>
				</tr>
			</table>
		</div>
		<div class="search_button">
			<search @search-pressed="search_students"/>
		</div>
	</div>	
</template>

<style scoped>
	.container {
		display: flex;
		justify-content: center;
		align-items: center;
		flex-direction: column;
	}

	.search_button {
		margin-top: 20px;
	}

	.student_list a {
		padding: 5px;
		margin: 5px;
	}

	th {
		font-weight: bold;
		color: beige;
	}

	td,th {
		padding: 10px;
		text-align: center;
	}

	.list table,td,th {
		border: 0.5px solid rgb(184, 167, 184);
	}

</style>
