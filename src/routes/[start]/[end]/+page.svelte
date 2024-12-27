<script lang="ts">
	import { page } from '$app/stores';
	import moment, { type unitOfTime } from 'moment';
	import { onMount } from 'svelte';
	import Birth from '../../../birth.svelte';
	import Layout from '../../+layout.svelte';

	let start, end;

	$: ({ start, end } = $page.params);

	let now = new Date();

	let nowText;
	$: nowText =
		now.toLocaleDateString('th-TH', {
			weekday: 'long',
			year: 'numeric',
			month: 'long',
			day: 'numeric'
		}) +
		' ' +
		now.toLocaleTimeString('th-TH');

	$: startMoment = moment(Number(start));
	$: endMoment = moment(Number(end));
	$: nowMoment = moment(now);
	$: tmpMoment = moment(nowMoment);
	$: years = endMoment.diff(tmpMoment, 'years');
	$: tmpMoment.add(years, 'years');

	$: months = endMoment.diff(tmpMoment, 'months');
	$: tmpMoment.add(months, 'months');

	$: days = endMoment.diff(tmpMoment, 'days');
	$: tmpMoment.add(days, 'days');

	$: hours = endMoment.diff(tmpMoment, 'hours');
	$: tmpMoment.add(hours, 'hours');

	$: minutes = endMoment.diff(tmpMoment, 'minutes');
	$: tmpMoment.add(minutes, 'minutes');

	$: seconds = endMoment.diff(tmpMoment, 'seconds');

	$: innerHeight = 0;
	let properUnit: unitOfTime.Diff;
	$: properUnit =
		endMoment.diff(startMoment, 'months') > 300 && innerHeight < 400 ? 'years' : 'months';

	$: allTime = endMoment.diff(startMoment, properUnit);
	$: remainingTime = endMoment.diff(nowMoment, properUnit);
	$: timePassed = nowMoment.diff(startMoment, properUnit);

	let unitMemo: Map<unitOfTime.Diff, number[]> = new Map();
	let numList: number[] = [];
	$: if (properUnit) {
		if (!unitMemo.has(properUnit)) {
			let tmp = [];
			for (let i = 0; i <= allTime; i++) {
				tmp.push(i);
			}

			numList = tmp;
		} else {
			numList = unitMemo.get(properUnit)!;
		}
	}

	let lifePercent;
	$: lifePercent = ((now.valueOf() - Number(start)) * 100) / (Number(end) - Number(start));

	onMount(() => {
		const interval = setInterval(() => {
			now = new Date();
		}, 1000);

		return () => {
			clearInterval(interval);
		};
	});
</script>

<svelte:window bind:innerHeight />

<div class="w-[80%]">
	<p class="static text-gray-500 [@media(max-height:400px)]:hidden">{nowText}</p>
	<p class="bai-jamjuree-bold max-w-[70ch] text-wrap text-xl sm:text-2xl">
		<ph>ฉันมีเวลาเหลืออีก</ph> <ph>{years} ปี {months} เดือน {days} วัน</ph>
		<ph> {hours} ชั่วโมง {minutes} นาที {seconds} วินาที</ph>
	</p>
	<!-- <div class="relative h-20 bg-gray-500">
		<div class="absolute left-0 h-full bg-green-400" style="width: {lifePercent}%;"></div>
	</div> -->
	<div>ผ่านไปแล้ว {lifePercent.toFixed(1)}%</div>
	<div class="w-1/2 fill-orange-200"></div>
	<div
		class="grid grid-cols-[repeat(auto-fill,8px)] gap-[3px] sm:grid-cols-[repeat(auto-fill,10px)] sm:gap-1"
	>
		{#each numList as num}
			{#if num < timePassed}
				<div class="aspect-square bg-gray-400"></div>
			{:else}
				<div class="aspect-square bg-blue-800"></div>
			{/if}
		{/each}
	</div>
	<div class="mt-4 flex w-full items-center justify-center gap-5">
		<div>
			<div class="inline-block aspect-square w-[10px] bg-gray-400"></div>
			<span class="text-gray-700">
				= 1 {properUnit === 'months' ? 'เดือน' : 'ปี'} ที่ผ่านไปแล้ว</span
			>
		</div>
		<div>
			<div class="inline-block aspect-square w-[10px] bg-blue-800"></div>
			<span class="text-gray-700"> = 1 {properUnit === 'months' ? 'เดือน' : 'ปี'} ที่ยังเหลือ</span>
		</div>
	</div>
</div>

<style>
	ph {
		display: inline-block;
	}
</style>
