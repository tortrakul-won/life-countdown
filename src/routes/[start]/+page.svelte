<script lang="ts">
	import { page } from '$app/stores';
	import moment from 'moment';
	import { toast } from 'svelte-sonner';

	const { start } = $page.params;

	$: startMoment = moment(Number(start));
	$: currentAge = moment().year() - startMoment.get('year');

	let deathTimeCustom: string;

	function handleMouseLeaveCustom(value: string) {
		deathTimeCustom = value
			? Math.min(Math.max(Number(value), currentAge + 1), 120).toString()
			: value;
	}

	function gotoCountdown() {
		const deathTimeOption = document.querySelector(
			'[name="death-time-opt"]:checked'
		) as HTMLInputElement;

		let deathTimeValue: string;
		if (deathTimeOption) {
			deathTimeValue = deathTimeOption.value;
		} else if (deathTimeCustom) {
			deathTimeValue = deathTimeCustom;
		} else {
			toast.error('โปรดใส่อายุขัยของคุณให้ถูกต้อง');
			return;
		}

		if (deathTimeValue) {
			const endMoment = startMoment.add(deathTimeValue, 'years');
			const dt = endMoment.valueOf();
			document.location.href = `/${start}/${dt}`;
		}
	}

	const yearOpts = [30, 40, 50, 60, 70, 80, 90, 100];

	function handleClickCustomDeathTime() {
		const deathTimeInput = document.querySelector(
			'[name="death-time-opt"]:checked'
		) as HTMLInputElement | null;

		deathTimeInput && (deathTimeInput.checked = false);
	}

	function handleClickOption() {
		const deathTimeInput = document.getElementById('dt-custom') as HTMLInputElement;

		deathTimeInput.value = '';
	}
</script>

<div class="w-3/5">
	<div class="bai-jamjuree-semibold pb-4 text-center text-lg">คุณคิดว่าจะมีชีวิตถึงอายุกี่ปี</div>
	<div id="death-time-radio" class="text-md flex flex-col flex-wrap gap-3 self-auto text-center">
		{#each yearOpts as year, index}
			<div>
				<input
					class="w-1/5 md:w-3/5"
					type="radio"
					name="death-time-opt"
					on:click={handleClickOption}
					id="dt-{year}"
					value={year}
					checked={index === 0}
				/>
				<label class="dt-label" for="dt-{year}">{year}</label>
			</div>
		{/each}
	</div>
	<div class="mt-3 flex flex-col items-center gap-3">
		<span class="text-center">หรือ</span>
		<form class="contents" on:submit|preventDefault={gotoCountdown}>
			<input
				id="dt-custom"
				type="number"
				min="1"
				max="120"
				placeholder="ระบุอายุขัย"
				bind:value={deathTimeCustom}
				on:blur={() => handleMouseLeaveCustom(deathTimeCustom)}
				on:click={handleClickCustomDeathTime}
			/>
		</form>
		<button
			class="w-fit rounded-md border-2 border-blue-400 p-1 hover:bg-blue-400"
			on:click={gotoCountdown}
		>
			ต่อไป</button
		>
	</div>
</div>

<style>
	input[name='death-time-opt'] {
		display: none;
	}

	#death-time-radio > div > input[type='radio']:checked + .dt-label {
		border-color: #60a5fa; /* Tailwind's blue-400 */
		background-color: #60a5fabb;
		color: white;
	}

	.dt-label {
		cursor: pointer;
		padding: 0.25rem 0.5rem;
		flex: 1 1 auto;
		display: block;
		border-width: 2px;
		border-color: #60a5fa; /* Tailwind's blue-400 */
		background-color: inherit;
		border-style: solid;
		border-radius: 10px;
	}

	.dt-label:hover {
		border-color: #60a5fa; /* Tailwind's blue-400 */
		background-color: #60a5fabb;
		color: white;
	}
	#dt-custom {
		width: 100%;
		cursor: pointer;
		padding: 0.25rem 0.5rem;
		flex: 1 1 auto;
		display: block;
		border-width: 2px;
		border-color: #60a5fa; /* Tailwind's blue-400 */
		background-color: inherit;
		border-style: solid;
		border-radius: 10px;
	}

	#dt-custom:not(:placeholder-shown) {
		box-shadow: 0px 0px 5px 5px #9dc9fe;
	}

	#dt-custom:not(:placeholder-shown):user-invalid {
		box-shadow: 0px 0px 5px 5px #fe9d9d;
	}
</style>
