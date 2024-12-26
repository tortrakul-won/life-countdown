<script>
	import { Toaster } from 'svelte-sonner';
	import '../app.css';
</script>

<Toaster richColors />
<div class="relative flex h-dvh w-full items-center justify-center bg-blue-100 md:w-dvw">
	<div
		class="flex min-h-full w-full flex-col items-center justify-evenly rounded-md border-2 border-blue-400 md:min-h-[75%] md:w-3/4 [@media(min-height:340px)]:py-6"
	>
		<slot />
	</div>
	<div
		class="pointer-events-none absolute left-0 top-0 z-20 h-full w-full bg-blue-500"
		style="mask-image: url(/src/lib/image/noise-light.png);"
	></div>
</div>
