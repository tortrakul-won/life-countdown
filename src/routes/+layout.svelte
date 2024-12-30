<script>
	import { Toaster } from 'svelte-sonner';
	import '../app.css';
	import { noise } from '../lib/image/noise-light.json';
</script>

<Toaster richColors />
<div
	class="relative flex min-h-[100dvh] min-w-full items-center justify-center bg-blue-100 md:min-w-[100dvw]"
>
	<div
		class="flex min-h-screen w-full flex-col items-center justify-evenly rounded-md border-2 border-blue-400 py-6 [@media(min-height:800px)]:min-h-[75%] [@media(min-height:800px)]:w-3/4 [@media(min-width:1000px)]:min-h-[75%] [@media(min-width:1000px)]:w-3/4"
	>
		<slot />
	</div>
	<div
		class="pointer-events-none absolute left-0 top-0 z-20 h-full w-full bg-blue-500"
		style="mask-image: url({noise});"
	></div>
</div>
