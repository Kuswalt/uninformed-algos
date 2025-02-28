<script lang="ts">
	import { bidirectionalSearch } from '$lib/uninformedAlgorithms';
	import GraphVisualizer from '$lib/components/GraphVisualizer.svelte';
	
	let graph: Record<string, string[]> = {
		'A': ['B', 'C'],
		'B': ['D', 'E'],
		'C': ['F', 'G'],
		'D': ['H'],
		'E': ['H'],
		'F': ['I'],
		'G': ['I'],
		'H': ['I'],
		'I': []
	};

	let start = 'A';
	let goal = 'I';
	let path: string[] | null = null;
	
	function findPath() {
		path = bidirectionalSearch(graph, start.toUpperCase(), goal.toUpperCase());
	}

	// For adding new nodes
	let newNode = '';
	let connectFrom = '';
	let connectTo = '';

	function addNode() {
		const newNodeUpper = newNode.toUpperCase();
		if (newNodeUpper && !graph[newNodeUpper]) {
			graph[newNodeUpper] = [];
			graph = graph;
			newNode = '';
		}
	}

	function addConnection() {
		const fromUpper = connectFrom.toUpperCase();
		const toUpper = connectTo.toUpperCase();
		if (fromUpper && toUpper && graph[fromUpper] && graph[toUpper]) {
			graph[fromUpper] = [...graph[fromUpper], toUpper];
			graph = graph;
			connectFrom = '';
			connectTo = '';
		}
	}

	let showGraph = true;
</script>

<div class="space-y-8">
	<div class="bg-white p-6 rounded-lg shadow">
		<h2 class="text-2xl font-bold mb-4">Bidirectional Search</h2>
		<p class="mb-4">
			Bidirectional search runs two simultaneous searches: one forward from the start state and one backward
			from the goal state, stopping when the two searches meet in the middle. This can be much faster than
			searching from just one direction.
		</p>

		<div class="grid grid-cols-2 gap-4 mb-4">
			<div>
				<label class="block mb-2">Start Node:</label>
				<input
					type="text"
					bind:value={start}
					class="border p-2 rounded w-full"
				/>
			</div>
			<div>
				<label class="block mb-2">Goal Node:</label>
				<input
					type="text"
					bind:value={goal}
					class="border p-2 rounded w-full"
				/>
			</div>
		</div>

		<button
			on:click={findPath}
			class="bg-blue-500 text-white px-4 py-2 rounded hover:bg-blue-600"
		>
			Find Path
		</button>

		{#if path}
			<div class="mt-4">
				<h3 class="font-bold">Path found:</h3>
				<p class="mt-2">{path.join(' → ')}</p>
			</div>
		{:else if path === null}
			<div class="mt-4 text-red-600">
				<h3 class="font-bold">No path found</h3>
			</div>
		{/if}
	</div>

	<div class="bg-white p-6 rounded-lg shadow">
		<h3 class="text-xl font-bold mb-4">Modify Graph</h3>
		
		<div class="space-y-4">
			<div>
				<h4 class="font-semibold mb-2">Add New Node</h4>
				<div class="flex gap-2">
					<input
						type="text"
						bind:value={newNode}
						placeholder="Node name"
						class="border p-2 rounded"
					/>
					<button
						on:click={addNode}
						class="bg-green-500 text-white px-4 py-2 rounded hover:bg-green-600"
					>
						Add Node
					</button>
				</div>
			</div>

			<div>
				<h4 class="font-semibold mb-2">Add Connection</h4>
				<div class="flex gap-2">
					<input
						type="text"
						bind:value={connectFrom}
						placeholder="From node"
						class="border p-2 rounded"
					/>
					<input
						type="text"
						bind:value={connectTo}
						placeholder="To node"
						class="border p-2 rounded"
					/>
					<button
						on:click={addConnection}
						class="bg-green-500 text-white px-4 py-2 rounded hover:bg-green-600"
					>
						Add Connection
					</button>
				</div>
			</div>
		</div>
	</div>

	<div class="bg-white p-6 rounded-lg shadow">
		<h3 class="text-xl font-bold mb-4">Current Graph</h3>
		<div class="flex justify-center">
			<GraphVisualizer {graph} {path} />
		</div>
	</div>
</div>
