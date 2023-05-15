<script lang="ts">
	function submit() {
		alert('Yuss')
	}
</script>

<form on:reset>
	<input type="text" name="username" placeholder="Username" autocomplete="username" />
	<input type="email" name="email" placeholder="Email" autocomplete="email" />
	<input type="password" name="password" placeholder="Password" autocomplete="current-password" />

	<button type="submit" on:click|preventDefault={submit}>Submit</button>
	<button formmethod="dialog" type="reset">Cancel</button>
</form>

<style>
	form {
		display: flex;
		flex-direction: column;
		justify-content: center;
		align-items: center;
		gap: 4px;
	}
</style>
