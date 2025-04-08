<script lang="ts">
	import { z } from 'zod';
	import { superForm } from 'sveltekit-superforms';
	import { zod } from 'sveltekit-superforms/adapters';

	const dummyLoginData = {
		email: 'testmail@test.de',
		password: '12345678?',
	};

	const schema = z.object({
		email: z.string().email('Ungültige E-Mail-Adresse').min(1, 'E-Mail ist erforderlich'),
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
					if (form.data.email === dummyLoginData.email && form.data.password === dummyLoginData.password) {
						console.log('Daten sind OK');

						const fadeOutElement = document.getElementById('formContainer');
						if (fadeOutElement) {
							fadeOutElement.classList.add('opacity-0', 'transition-opacity', 'duration-1000');
						}

						setTimeout(() => {
							window.location.href = '/select';
						}, 1000);
					} else {
						console.log('Falsche E-Mail-Adresse oder Passwort');
					}
				}
			},
		}
	);
</script>

<div
	class="from-medium-lila via-soft-lila flex min-h-screen min-w-screen items-center justify-center bg-gradient-to-br to-pink-200"
>
	<div
		id="formContainer"
		class="w-full max-w-md transform rounded-3xl bg-white p-8 shadow-lg transition-opacity duration-1000"
	>
		<h2 class="mb-6 text-center text-3xl font-bold text-gray-800">Login</h2>
		<form method="POST" use:enhance class="flex flex-col space-y-6">
			<label class="flex flex-col">
				<span class="mb-2 text-lg text-gray-700">E-Mail</span>
				<input
					name="email"
					aria-invalid={$errors.email ? 'true' : undefined}
					bind:value={$form.email}
					{...$constraints.email}
					placeholder="you@example.com"
					class="focus:ring-medium-lila rounded-xl border-2 border-gray-300 p-3 text-black transition-all duration-300 ease-in-out focus:ring-2 focus:outline-none
						{$errors.email ? 'border-pink-500 text-pink-600' : 'border-gray-300'}"
				/>
				{#if $errors.email}
					<span class="mt-1 text-sm text-pink-600">{$errors.email}</span>
				{/if}
			</label>

			<label class="flex flex-col">
				<span class="mb-2 text-lg text-gray-700">Passwort</span>
				<input
					type="password"
					name="password"
					bind:value={$form.password}
					aria-invalid={$errors.password ? 'true' : undefined}
					{...$constraints.password}
					class="focus:ring-medium-lila rounded-xl border-2 border-gray-300 p-3 text-black transition-all duration-300 ease-in-out focus:ring-2 focus:outline-none
						{$errors.password ? 'border-pink-500 text-pink-600' : 'border-gray-300'}"
				/>
				{#if $errors.password}
					<span class="mt-1 text-sm text-pink-600">{$errors.password}</span>
				{/if}
			</label>

			<button
				class="bg-medium-lila hover:bg-login-lila mt-4 w-full transform cursor-pointer rounded-xl py-3 font-semibold text-white transition-all duration-300 ease-in-out hover:scale-105"
				type="submit"
			>
				Login
			</button>
		</form>
	</div>
</div>
