<script>
	import Footer from '$lib/components/footer/footer.svelte';
</script>

<div class="flex min-h-[calc(100vh-56px)] flex-col">
	<div
		class="absolute inset-0 -z-10 bg-white bg-grid-black/[0.2] dark:bg-black dark:bg-grid-white/[0.2]"
	>
		<div
			class="pointer-events-none absolute inset-0 flex items-center justify-center bg-white [mask-image:radial-gradient(ellipse_at_center,transparent_20%,black)] dark:bg-black"
		/>
	</div>
	<div class="flex-grow">
		<slot />
	</div>
	<!-- <Footer /> -->
</div>
