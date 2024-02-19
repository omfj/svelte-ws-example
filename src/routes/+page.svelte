<script lang="ts">
	let ws = $state<WebSocket | null>(null);
	let count = $state<number | null>(0);

	$effect(() => {
		ws = new WebSocket('ws://localhost:3000/ws/count');
	});

	$effect(() => {
		if (ws) {
			ws.onopen = () => {
				console.log('connected');
			};
			ws.onmessage = (event) => {
				console.log(event.data);

				count = Number(event.data);
			};
			ws.onclose = () => {
				console.log('disconnected');
			};
		}
	});

	const increment = () => {
		if (ws) {
			ws.send('inc');
		}
	};

	const decrement = () => {
		if (ws) {
			ws.send('dec');
		}
	};
</script>

<h1>Welcome to SvelteKit</h1>
<p>Visit <a href="https://kit.svelte.dev">kit.svelte.dev</a> to read the documentation</p>

<p>Count: {count}</p>

<button onclick={increment}>Increment</button>

<button onclick={decrement}>Decrement</button>
