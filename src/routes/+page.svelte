<script lang="ts">
	import { z } from 'zod';
	import 'tailwindcss';
	import { superForm } from 'sveltekit-superforms';
  
	let email: string = '';
	let password: string = '';

	const schema = z.object({
	  email: z.string().email('unvalid mail adress'),
	  password: z.string().min(8, 'password needs min a length from 8').regex(/[!@#$%^&*(),.?":{}|<>]/, "password needs special characters"),
	});
  
	// const { form, errors } = superForm(
	// 	{ schema, }
	// )

	const handleSubmit = (event: Event) => {
	  event.preventDefault();
	  const result = schema.safeParse({ email, password });
	  console.log(result)
	};
  </script>
  
  <form on:submit={handleSubmit}>
	<label>
	  Email
	  <input name="email" type="email" bind:value={email}>
	</label>
	<label>
		Password
		<input name="password" bind:value="{password}">
	</label>
	<button type="submit">Login</button>
  </form>
  