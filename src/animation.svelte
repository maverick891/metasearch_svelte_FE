<script>
	let toggled = false;
</script>

<main>
	<div class={`transition-all duration-500 ease-in-out ${toggled ? 'mb-4 mt-20 text-2xl md:text-2xl lg:text-3xl' : 'mb-4 mt-64 text-4xl md:text-5xl lg:text-6xl'}`}>
		Your Text Here
	</div>
	<button on:click={() => toggled = !toggled}>
		Toggle Animation
	</button>
</main>

<style>
 
</style>
