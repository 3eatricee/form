<script lang="ts">
	import { z } from 'zod';
	import { setError, setMessage, superForm } from 'sveltekit-superforms';
	import { zod } from 'sveltekit-superforms/adapters';
	import "tailwindcss";
  
	const dummyLoginData = {
	  email: 'testmail@test.de',
	  password: '12345678?',
	};
  
	const schema = z.object({
	  email: z
		.string()
		.email('Ungültige E-Mail-Adresse')
		.min(1, 'E-Mail ist erforderlich'),
	  password: z
		.string()
		.min(8, 'Passwort muss mindestens 8 Zeichen lang sein')
		.regex(/[!@#$%^&*(),.?":{}|<>]/, 'Passwort muss mindestens ein Sonderzeichen enthalten'),
	});
  
	const { form, errors, constraints, enhance } = superForm(
	  {
		email: '',
		password: '',
	  },
	  {
		SPA: true,
		validators: zod(schema),
		onUpdate({ form }) {
		  if (form.valid) {
			console.log('form validaiton');
			if (
			  form.data.email === dummyLoginData.email &&
			  form.data.password === dummyLoginData.password
			) {
			  console.log('Daten sind OK');
			  setMessage(form, 'Login erfolgreich!');
			} else {
			  console.log('Falsche E-Mail-Adresse oder Passwort');
			  setError(form, 'email', 'Falsche E-Mail-Adresse oder Passwort');
			}
		  }
		},
	  }
	);

</script>
    
  <!-- {#if $message}
	<h3 >{$message}</h3>
  {/if} -->

  <div class="flex min-h-screen min-w-screen flex-col items-center justify-center bg-slate-100">
	<div class="max-w-lg min-w-lg bg-sky-950 rounded-4xl p-10">
	  <form method="POST" use:enhance class="flex flex-col space-y-6">
		
		<label>
			<span class="text-slate-200">
				E-Mail
			</span>
			<input
			  name="email"
			  aria-invalid="{$errors.email ? 'true' : undefined}"
			  bind:value="{$form.email}"
			  {...$constraints.email}
			  class="w-full invalid:border-pink-500 invalid:text-pink-600 rounded-xl"
			/>
			{#if $errors.email}
			  <span class="text-pink-600">{$errors.email}</span>
			{/if}
		</label>
		
		<label>
			<span class="text-slate-200">
				Passwort
			</span>
			<input
			  type="password"
			  name="password"
			  bind:value="{$form.password}"
			  aria-invalid="{$errors.password ? 'true' : undefined}"
			  {...$constraints.password}
			  class="w-full invalid:border-pink-500 invalid:text-pink-600 rounded-xl text-black"
			/>
			{#if $errors.password}
			  <span class="text-pink-600">{$errors.password}</span>
			{/if}
		</label>
  
		<button class="bg-sky-500 hover:bg-sky-700 p-2 rounded-xl" type="submit">Login</button>
	  </form>
	</div>
  </div>
  