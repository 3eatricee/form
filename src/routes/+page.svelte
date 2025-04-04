<script lang="ts">
	import { z } from 'zod';
	import { superForm } from 'sveltekit-superforms';
	import { zod } from 'sveltekit-superforms/adapters';
  
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
			if (
			  form.data.email === dummyLoginData.email &&
			  form.data.password === dummyLoginData.password
			) {
			  console.log('Daten sind OK');
			} else {
			  console.log('Falsche E-Mail-Adresse oder Passwort');
			}
		  }
		},
	  }
	);
</script>
    
<div class="flex min-h-screen min-w-screen flex-col items-center justify-center bg-slate-100">
	<div class="max-w-lg min-w-lg bg-beagreen rounded-4xl p-10 flex flex-col justify-between">
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
				placeholder="you@example.com"
				class="w-full rounded-xl text-black 
					{$errors.email ? 'border-pink-500 text-pink-600' : 'border-slate-300'}"
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
			  class="w-full rounded-xl text-black 
			  {$errors.email ? 'border-pink-500 text-pink-600' : 'border-slate-300'}"
			/>
			{#if $errors.password}
			  <span class="text-pink-600">{$errors.password}</span>
			{/if}
		</label>
		<button class="cursor-pointer bg-sky-500 hover:bg-sky-700 min-w-24 p-2 rounded-xl self-center" type="submit">Login</button>
	  </form>
	</div>
</div>

  