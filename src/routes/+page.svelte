<script lang="ts">
	function gotoEndTime() {
		const birthTimeInput = document.getElementById('birth-time') as HTMLInputElement;
		const birthTimeValue = birthTimeInput ? birthTimeInput.value : null;

		const bt = birthTimeValue ? Date.parse(birthTimeValue) : null;
		if (bt) {
			document.location.href = `/${bt}`;
		}
	}
	import { onMount } from 'svelte';

	let birthTime = new Date(); // Get current date and time in 'YYYY-MM-DDTHH:MM' format
	function addDays(date: Date, days: number) {
		var result = new Date(date);
		result.setDate(result.getDate() + days);
		return result;
	}
	birthTime = addDays(birthTime, -6570);
	onMount(() => {
		// Set the default value to the current date and time
		const birthTimeInput = document.getElementById('birth-time') as HTMLInputElement;
		if (birthTimeInput) {
			birthTimeInput.value = birthTime.toISOString().slice(0, 16);
		}
	});
</script>

<div class="">โปรดใส่วันเกิดของคุณ</div>
<div>
	<input type="datetime-local" id="birth-time" />
	<button
		class="rounded-md border-2 border-blue-400 p-1 hover:bg-blue-400"
		on:click={() => gotoEndTime()}
	>
		confirm</button
	>
</div>
