<script lang="ts">
	import "../app.css";
	let { children } = $props();

	const routes = [
		{ path: '/bfs', name: 'Breadth-first Search' },
		{ path: '/dfs', name: 'Depth-first Search' },
		{ path: '/ucs', name: 'Uniform Cost Search' },
		{ path: '/dls', name: 'Depth Limited Search' },
		{ path: '/iddfs', name: 'Iterative Deepening DFS' },
		{ path: '/bi-s', name: 'Bidirectional Search' }
	];
</script>

<div class="min-h-screen bg-gray-100">
	<nav class="bg-white shadow-lg">
		<div class="max-w-7xl mx-auto px-4">
			<div class="flex justify-between">
				<div class="flex space-x-4">
					<a href="/" class="flex items-center py-4 px-2">
						<span class="font-semibold text-gray-500 text-lg">Search Algorithms</span>
					</a>
				</div>
				<div class="flex space-x-4">
					{#each routes as route}
						<a 
							href={route.path}
							class="py-4 px-2 text-gray-500 hover:text-gray-900 transition duration-300"
						>
							{route.name}
						</a>
					{/each}
				</div>
			</div>
		</div>
	</nav>

	<main class="max-w-7xl mx-auto px-4 py-8">
		{@render children()}
	</main>
</div>
