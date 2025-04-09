<script lang="ts">
	import SelectComponent from '../../components/SelectComponent.svelte';

	const themes = [
		{ value: 'bea-green', label: 'Bea Test Green', color: '#92997e' },
		{ value: 'dark-green', label: 'Dark Green', color: '#006400' },
		{ value: 'svelte-orange', label: 'Svelte Orange', color: '#FF5E00' },
		{ value: 'punk-pink', label: 'Punk Pink', color: '#D4006C' },
	];

	let selectedValue: string = '';
	let selectedMultipleValues: string[] = [];

	const selected = () => {
		const selectedTheme = themes.find(theme => theme.value === selectedValue);
		return selectedTheme
			? { color: selectedTheme.color, label: selectedTheme.label }
			: { color: '#ffffff', label: 'Select a theme' };
	};

	const selectedMultipleColors = () => {
		const selectedThemes = themes.filter(theme => selectedMultipleValues.includes(theme.value));
		return selectedThemes.map(theme => theme.color);
	};

	const gradientBackground = () => {
		const colors = selectedMultipleColors();
		if (colors.length === 0) return '';
		if (colors.length === 1) return `bg-${colors[0]}`;
		return `linear-gradient(to right, ${colors.join(', ')})`;
	};

	const selectedMultipleLabels = () => {
		return selectedMultipleValues.length
			? themes
					.filter(theme => selectedMultipleValues.includes(theme.value))
					.map(theme => theme.label)
					.join(', ')
			: 'Select min 2 to create a gradient';
	};

	const minSelectionMessage = () => {
		return selectedMultipleValues.length < 2 ? 'Please select at least two themes for a gradient' : '';
	};

	function randomizeThemes() {
		const randomTheme = themes[Math.floor(Math.random() * themes.length)];
		selectedValue = randomTheme.value;
	}
</script>

<div
	class="from-medium-lila via-soft-lila flex h-screen w-screen flex-col space-y-12 bg-gradient-to-br to-pink-200 p-10"
>
	<div class="fade-in-up mx-auto flex min-w-[900px] flex-col space-y-6 rounded-xl bg-white p-8 shadow-lg">
		<h2 class="text-center text-2xl font-semibold text-gray-800">Select Theme</h2>
		<div>
			<SelectComponent type="single" items={themes} bind:selectedValue selectedLabel={selected().label} />
		</div>
		<div
			class="mt-5 mb-5 flex h-30 w-full items-center justify-center rounded-xl transition-all duration-300 ease-in-out"
			style="background-color: {selected().color};"
		>
			<h1 class="font-semibold text-white">{selected().label}</h1>
		</div>
		<div class="pt-5">
			<button
				on:click={randomizeThemes}
				class="bg-medium-lila absolute right-4 bottom-4 cursor-pointer rounded-lg px-6 py-2 font-semibold text-white transition-all duration-300 hover:scale-105"
			>
				Randomize Theme
			</button>
		</div>
	</div>

	<div class="fade-in-up mx-auto flex min-w-[900px] flex-col space-y-6 rounded-xl bg-white p-8 shadow-lg">
		<h2 class="text-center text-2xl font-semibold text-gray-800">Select Multiple Themes</h2>
		<div>
			<SelectComponent
				type="multiple"
				items={themes}
				bind:selectedValue={selectedMultipleValues}
				selectedLabel={selectedMultipleLabels()}
			/>
		</div>
		<p class="mt-2 text-sm text-pink-500">{minSelectionMessage()}</p>
		<div
			class="mt-5 mb-5 flex h-30 w-full items-center justify-center rounded-xl transition-all duration-300 ease-in-out"
			style="background: {gradientBackground()};"
		>
			<h1 class="font-semibold text-white">{selectedMultipleLabels()}</h1>
		</div>
	</div>
</div>
