<script lang="ts">
	const WEBSOCKET_URL = 'ws://localhost:3000/ws/count';

	let ws = $state<WebSocket>();
	let count = $state<number | null>(null);

	$effect(() => {
		ws = new WebSocket(WEBSOCKET_URL);

		return () => ws?.close();
	});

	$effect(() => {
		const handleMessage = (event: MessageEvent) => {
			count = Number(event.data);
		};

		ws?.addEventListener('message', handleMessage);

		return () => ws?.removeEventListener('message', handleMessage);
	});

	$effect(() => {
		const fetchCount = async () => {
			const response = await fetch('http://localhost:3000/count');
			const data = await response.text();
			count = Number(data);
		};

		void fetchCount();
	});

	const increment = () => {
		if (!ws) return;
		ws.send('inc');
	};

	const decrement = () => {
		if (!ws) return;
		ws.send('dec');
	};
</script>

<h1>Welcome to SvelteKit</h1>
<p>Visit <a href="https://kit.svelte.dev">kit.svelte.dev</a> to read the documentation</p>

<p>Count: {count}</p>

<button type="button" onclick={increment}>Increment</button>

<button type="button" onclick={decrement}>Decrement</button>
