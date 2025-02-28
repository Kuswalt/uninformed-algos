<script lang="ts">
	export let graph: Record<string, any[]>;
	export let path: string[] | null = null;
	
	// Calculate positions for nodes
	let width = 600;
	let height = 400;
	let radius = 20;
	
	// Generate positions for nodes in a force-directed-like layout
	function generateNodePositions(nodes: string[]) {
		const positions: Record<string, {x: number, y: number}> = {};
		const levels: Record<string, number> = {};
		
		// Simple level assignment (BFS)
		let queue = [{node: nodes[0], level: 0}];
		let visited = new Set([nodes[0]]);
		
		while (queue.length > 0) {
			const {node, level} = queue.shift()!;
			levels[node] = level;
			
			for (const neighbor of graph[node] || []) {
				const nextNode = Array.isArray(neighbor) ? neighbor[0] : neighbor;
				if (!visited.has(nextNode)) {
					visited.add(nextNode);
					queue.push({node: nextNode, level: level + 1});
				}
			}
		}
		
		// Assign positions based on levels
		const levelGroups: Record<number, string[]> = {};
		Object.entries(levels).forEach(([node, level]) => {
			if (!levelGroups[level]) levelGroups[level] = [];
			levelGroups[level].push(node);
		});
		
		const maxLevel = Math.max(...Object.values(levels));
		Object.entries(levelGroups).forEach(([level, nodes]) => {
			const numNodes = nodes.length;
			nodes.forEach((node, i) => {
				positions[node] = {
					x: (Number(level) + 1) * (width / (maxLevel + 2)),
					y: (i + 1) * (height / (numNodes + 1))
				};
			});
		});
		
		return positions;
	}
	
	$: nodePositions = generateNodePositions(Object.keys(graph));
	
	// Check if an edge is part of the path
	function isEdgeInPath(from: string, to: string) {
		if (!path) return false;
		for (let i = 0; i < path.length - 1; i++) {
			if (path[i] === from && path[i + 1] === to) return true;
		}
		return false;
	}
</script>

<svg {width} {height} class="border rounded bg-white">
	<!-- Edges -->
	{#each Object.entries(graph) as [from, neighbors]}
		{#each neighbors as neighbor}
			{@const to = Array.isArray(neighbor) ? neighbor[0] : neighbor}
			{@const weight = Array.isArray(neighbor) ? neighbor[1] : null}
			{#if nodePositions[from] && nodePositions[to]}
				<g>
					<line
						x1={nodePositions[from].x}
						y1={nodePositions[from].y}
						x2={nodePositions[to].x}
						y2={nodePositions[to].y}
						class="stroke-2 {isEdgeInPath(from, to) ? 'stroke-blue-500' : 'stroke-gray-300'}"
					/>
					{#if weight !== null}
						<text
							x={(nodePositions[from].x + nodePositions[to].x) / 2}
							y={(nodePositions[from].y + nodePositions[to].y) / 2}
							class="text-sm fill-gray-600"
							dy="-5"
						>
							{weight}
						</text>
					{/if}
				</g>
			{/if}
		{/each}
	{/each}
	
	<!-- Nodes -->
	{#each Object.keys(graph) as node}
		{#if nodePositions[node]}
			<g>
				<circle
					cx={nodePositions[node].x}
					cy={nodePositions[node].y}
					r={radius}
					class="
						fill-white stroke-2
						{path?.includes(node) ? 'stroke-blue-500' : 'stroke-gray-400'}
						{node === path?.[0] ? 'fill-green-100' : ''}
						{node === path?.[path.length - 1] ? 'fill-red-100' : ''}
					"
				/>
				<text
					x={nodePositions[node].x}
					y={nodePositions[node].y}
					text-anchor="middle"
					dominant-baseline="middle"
					class="text-sm font-semibold"
				>
					{node}
				</text>
			</g>
		{/if}
	{/each}
</svg> 