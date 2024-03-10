<script>
	import Input from './Input.svelte';
</script>

<svelte:head>
	<title>Eenie Menie Minee</title>
	<meta name="description" content="Pick the option" />
</svelte:head>

<section>
	<Input />
</section>

<style>
	section {
		display: flex;
		flex-direction: column;
		justify-content: center;
		align-items: center;
		flex: 0.6;
	}
</style>
