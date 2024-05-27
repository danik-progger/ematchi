<div class="modal-background">
	<div class="modal">
		<slot />
	</div>
</div>

<style>
	.modal-background {
		position: fixed;
		width: 100%;
		height: 100%;
        top: 0;
        left: 0;
		backdrop-filter: blur(10px);
		display: grid;
		place-items: center;
		z-index: 2;
	}
</style>
