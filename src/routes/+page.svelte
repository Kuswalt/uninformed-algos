<script lang="ts">
	const algorithms = [
		{
			name: 'Breadth-First Search (BFS)',
			path: '/bfs',
			description: 'Explores all nodes at the present depth before moving to nodes at the next depth level. Guarantees the shortest path in unweighted graphs.'
		},
		{
			name: 'Depth-First Search (DFS)',
			path: '/dfs',
			description: 'Explores as far as possible along each branch before backtracking. Memory efficient but does not guarantee shortest path.'
		},
		{
			name: 'Uniform Cost Search (UCS)',
			path: '/ucs',
			description: 'Finds the path with the lowest total cost between nodes in a weighted graph. Guarantees optimal path when edge costs are non-negative.'
		},
		{
			name: 'Depth Limited Search (DLS)',
			path: '/dls',
			description: 'A variant of DFS with a depth limit to prevent infinite exploration in deep or cyclic graphs.'
		},
		{
			name: 'Iterative Deepening DFS (IDDFS)',
			path: '/iddfs',
			description: 'Combines DFS space-efficiency with BFS completeness by repeatedly performing DLS with increasing depth limits.'
		},
		{
			name: 'Bidirectional Search',
			path: '/bi-s',
			description: 'Runs two simultaneous searches from both start and goal states, potentially reducing the search space significantly.'
		}
	];
</script>

<div class="min-h-screen bg-gray-100 py-12">

		<div class="grid md:grid-cols-2 gap-6">
			{#each algorithms as algo}
				<a
					href={algo.path}
					class="block bg-white rounded-lg shadow-md hover:shadow-lg transition-shadow duration-300"
				>
					<div class="p-6">
						<h2 class="text-2xl font-semibold text-gray-900 mb-3">
							{algo.name}
						</h2>
						<p class="text-gray-600">
							{algo.description}
						</p>
					</div>
				</a>
			{/each}
		</div>

	</div>

