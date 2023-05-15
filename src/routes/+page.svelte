<script lang="ts">
	import Form from '$lib/Form.svelte'

	let dialog: HTMLDialogElement

	function openDialog() {
		dialog.show()
	}

	function openModal() {
		dialog.showModal()
	}
</script>

<svelte:head>
	<title>1Password Dialog Test</title>
</svelte:head>

<section>
	<h1>1Password Dialog Test</h1>

	<p>
		This is a minimal reproduction of a bug in the 1password chrome extension which causes the
		auto-fill popups to render behind modals using the
		<a href="https://developer.mozilla.org/en-US/docs/Web/HTML/Element/dialog">
			<code>&lt;dialog /&gt;</code>
			element.
		</a>

		Dialogs are somewhat new, but it's approaching
		<a href="https://caniuse.com/dialog">95% global support</a> and saves a whole bunch of hassle managing
		z-indexes when building modals and improves accessibility, so they'll get more common with time.
	</p>
</section>

<section>
	<h2>On a normal html page</h2>

	<Form />
</section>

<hr />

<section>
	<h2>Inside a dialog using <code>.show()</code></h2>

	<p>Works fine because it just renders in the normal dom layer.</p>
	<button on:click={openDialog}>Open dialog</button>
</section>

<hr />

<section>
	<h2>
		Inside a dialog modal using <code>.showModal()</code>
	</h2>

	<p>
		This renders the the dialog inside the <code>#top-layer</code> section, which bypasses z-index
		entirely. As a result, the autofill dropdown renders behind the modal and
		<code>::backdrop</code>, making it impossible to use.
	</p>

	<button on:click={openModal}>Open dialog as modal</button>
</section>

<!-- 
	-- This is the dialog element in question - It can be opened as a dropdown
  -- or as a modal. The modal version is the one that breaks the 1password.
  -->
<dialog bind:this={dialog}>
	<Form on:reset={() => dialog.close()} />
</dialog>

<style>
	section {
		display: flex;
		flex-direction: column;
		align-items: center;
		flex: 0.6;
		margin: 20px 0;
	}
</style>
