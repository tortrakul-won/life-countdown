<script>
	import { Toaster } from 'svelte-sonner';
	import '../app.css';
</script>

<Toaster richColors />
<div class="relative flex min-h-dvh w-dvw items-center justify-center bg-blue-100">
	<div
		class="flex min-h-[75%] w-3/4 flex-col items-center justify-evenly rounded-md border-2 border-blue-400 py-6"
	>
		<slot />
	</div>
	<div
		class="pointer-events-none absolute left-0 top-0 z-20 h-full w-full bg-blue-500"
		style="mask-image: url(/src/lib/image/noise-light.png);"
	></div>
</div>
