<script>
	import { DarkMode, Search, Button, Table, TableBody, TableBodyCell, TableBodyRow, TableHead, TableHeadCell, P, Spinner } from 'flowbite-svelte'
	import { fade } from 'svelte/transition'
	let toggled = false
	let query
	const BACKEND_URL = "https://metasearch-jtutwlgqma-uc.a.run.app"
	//const BACKEND_URL = "http://localhost"
	let slider_value = 75
	let message
	let result

	async function Meta_Search() {
		if (!toggled) {

			toggled = true
			result = undefined
			console.log("EventSource created..")
			const evtSource = new EventSource(BACKEND_URL+`/metaSearchStream?query=`+query+`&sim=`+slider_value/100);
			console.log("After EventSource created..")
			message = "Performing Google Search..."
			
			evtSource.addEventListener("update", function(event) {
			message = event['data'];
			console.log(event);
			});

			evtSource.addEventListener("end", function(event) {
				console.log('Handling end....');
				message = event['data'];
				message = "";
				result = JSON.parse(event['data']);
				result.sort((a,b) => b.counter - a.counter);
				console.log(event['data']);
				evtSource.close(); 
				toggled = false
			});

			return false;

		}
		
	}
	//class={`transition-all duration-500 ease-in-out ${toggled ? 'text-2xl md:text-2xl lg:text-3xl' : 'text-4xl md:text-5xl lg:text-6xl'}`}
	//class="text-4xl sm:text-6xl font-bold text-gray-800 dark:text-gray-200"
	//class={`transition-all duration-500 ease-in-out ${toggled ? 'text-2xl md:text-2xl lg:text-3xl font-bold text-gray-800' : 'text-4xl md:text-5xl lg:text-6xl font-bold text-gray-800'}`}
</script>

<main>
	<!--
	<DarkMode/>
	<Heading tag="h1">Summarize</Heading>-->
	<!-- Hero -->
<!--
	<DarkMode class="hidden"/>-->
<div class="relative overflow-hidden">
	<div class="max-w-[85rem] mx-auto px-4 sm:px-6 lg:px-8 py-10 sm:py-24 min-w-[55rem] -mt-24">
	  <div class="text-center">
		<h1 class="text-4xl sm:text-6xl font-bold text-gray-800 dark:text-gray-200">
		  Meta Search
		</h1>
  
		<p class="mt-3 text-gray-600 dark:text-gray-400">
		  Let AI go through your search and aggregate data
		</p>
  
		<div class="mt-7 sm:mt-12 mx-auto max-w-xl relative">
		  <!-- Form -->
		  <!--
		  <form>
			<div class="relative z-10 flex space-x-3 p-3 bg-white border rounded-lg shadow-lg shadow-gray-100 dark:bg-slate-900 dark:border-gray-700 dark:shadow-gray-900/[.2]">
			  <div class="flex-[1_0_0%]">
				<label for="hs-search-article-1" class="block text-sm text-gray-700 font-medium dark:text-white"><span class="sr-only">Search article</span></label>
				<input type="email" name="hs-search-article-1" id="hs-search-article-1" class="py-2.5 px-4 block w-full border-transparent rounded-lg focus:border-purple-500 focus:ring-purple-500 dark:bg-slate-900 dark:border-transparent dark:text-gray-400 dark:focus:ring-gray-600" placeholder="Search">
			  </div>
			  <div class="flex-[0_0_auto]">
				<a on:click={() => toggled = !toggled} class="size-[46px] inline-flex justify-center items-center gap-x-2 text-sm font-semibold rounded-lg border border-transparent bg-purple-600 text-white hover:bg-purple-700 disabled:opacity-50 disabled:pointer-events-none dark:focus:outline-none dark:focus:ring-1 dark:focus:ring-gray-600" href="#">
				  <svg xmlns="http://www.w3.org/2000/svg" width="16" height="16" fill="currentColor" viewBox="0 0 16 16">
					<path d="M11.742 10.344a6.5 6.5 0 1 0-1.397 1.398h-.001c.03.04.062.078.098.115l3.85 3.85a1 1 0 0 0 1.415-1.414l-3.85-3.85a1.007 1.007 0 0 0-.115-.1zM12 6.5a5.5 5.5 0 1 1-11 0 5.5 5.5 0 0 1 11 0z"/>
				  </svg>
				</a>
			  </div>
			</div>-->
			<form class="Search" method="post" on:submit|preventDefault={Meta_Search}>
			<div class="relative z-10 flex space-x-3 p-3 rounded-lg">
				<div class="flex-[1_0_0%]">
				  <label for="hero-input" class="sr-only">Search</label>
				  <!--
				  <input bind:value={query} type="text" id="hero-input" name="hero-input" class="py-3 px-4 block w-full xl:min-w-[18rem] border-gray-200 shadow-sm rounded-md focus:z-10 focus:border-purple-500 focus:ring-purple-500 dark:bg-slate-900 dark:border-gray-700 dark:text-gray-400" placeholder="Search">-->
				
				  <Search size="lg" placeholder="Search" class="dark:bg-slate-700" bind:value={query}/>
				</div>
				<div class="flex-[0_0_auto]">
					<!--
				<a on:click={signUp} class="w-full sm:w-auto inline-flex justify-center items-center gap-x-3 text-center bg-purple-600 hover:bg-purple-700 border border-transparent text-white font-medium rounded-md focus:outline-none focus:ring-2 focus:ring-purple-600 focus:ring-offset-2 focus:ring-offset-white transition py-3 px-4 dark:focus:ring-offset-gray-800" href="#">
				  Sign up
				</a>-->
				<Button size="xl" class="!p-3.5" type="submit" color="purple">
                    <svg class="w-5 h-5" fill="none" stroke="currentColor" viewBox="0 0 24 24" xmlns="http://www.w3.org/2000/svg"><path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M21 21l-6-6m2-5a7 7 0 11-14 0 7 7 0 0114 0z"/></svg>
                </Button>
			  </div>
			  </div>
		  </form>
		  <!-- End Form -->
  
		  <!-- SVG Element -->
		  <div class="hidden md:block absolute top-0 end-0 -translate-y-12 translate-x-20">
			<svg class="w-16 h-auto text-purple-500" width="121" height="135" viewBox="0 0 121 135" fill="none" xmlns="http://www.w3.org/2000/svg">
			  <path d="M5 16.4754C11.7688 27.4499 21.2452 57.3224 5 89.0164" stroke="currentColor" stroke-width="10" stroke-linecap="round"/>
			  <path d="M33.6761 112.104C44.6984 98.1239 74.2618 57.6776 83.4821 5" stroke="currentColor" stroke-width="10" stroke-linecap="round"/>
			  <path d="M50.5525 130C68.2064 127.495 110.731 117.541 116 78.0874" stroke="currentColor" stroke-width="10" stroke-linecap="round"/>
			</svg>
		  </div>
		  <!-- End SVG Element -->
  
		  <!-- SVG Element -->
		  <div class="hidden md:block absolute bottom-0 start-0 translate-y-10 -translate-x-32">
			<svg class="w-40 h-auto text-green-500" width="347" height="188" viewBox="0 0 347 188" fill="none" xmlns="http://www.w3.org/2000/svg">
			  <path d="M4 82.4591C54.7956 92.8751 30.9771 162.782 68.2065 181.385C112.642 203.59 127.943 78.57 122.161 25.5053C120.504 2.2376 93.4028 -8.11128 89.7468 25.5053C85.8633 61.2125 130.186 199.678 180.982 146.248L214.898 107.02C224.322 95.4118 242.9 79.2851 258.6 107.02C274.299 134.754 299.315 125.589 309.861 117.539L343 93.4426" stroke="currentColor" stroke-width="7" stroke-linecap="round"/>
			</svg>
		  </div>
		  <!-- End SVG Element -->
		</div>
  
		<div class="mt-10 sm:mt-20">
		  
		</div>

		{#if message}
		<div transition:fade class="flex justify-center">
			<P color="text-green-600 dark:text-green-500" justify>{message}</P>
			<Spinner class="mx-2" size={6} color="green"/>
		</div>
		{/if}

		{#if result && result.length > 0}
			<div transition:fade>
				<Table hoverable={true} noborder={true} shadow color="purple">
					<TableHead>
					<TableHeadCell>Name</TableHeadCell>
					<TableHeadCell>Mentions</TableHeadCell>
					</TableHead>
					<TableBody>
						{#each result as res}
						{#if res.counter > 1}
							<TableBodyRow>
								<TableBodyCell>{res.string}</TableBodyCell>
								<TableBodyCell>{res.counter}</TableBodyCell>
							</TableBodyRow>
						{/if}
					{/each}
					</TableBody>
				</Table>
			</div>
		{/if}

	  </div>
	</div>
  </div>
  <!-- End Hero -->
</main>

<style>
 
</style>
