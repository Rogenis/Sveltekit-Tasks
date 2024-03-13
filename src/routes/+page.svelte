<script lang="ts">
	import './tailwind.css'; // Ou o caminho correto para o seu arquivo CSS Tailwind
	import { writable } from 'svelte/store';
	import Counter from '../lib/Counter.svelte';
	import { goto } from '$app/navigation';

	let nome = '';
  let telefone = '';
  let email = '';
  let desafioIniciado = false;
  const desafioSucesso = writable(false);

	let showModal = false;
  let modalMessage = '';
  let counterComponent: any;
	
	function iniciarDesafio() {
    desafioIniciado = true;
    counterComponent.startCounter();
  }

  function handleSubmit() {
    desafioSucesso.set(true);
    desafioIniciado = false;
    counterComponent.resetCounter();
    resetForm();
    modalMessage = 'Desafio finalizado com sucesso!';
    showModal = true; // Agora o modal aparecerá aqui
  }

	function resetForm() {
		nome = '';
		telefone = '';
		email = '';
	}

	// Função para lidar com o evento de tempo acabou
	function handleTempoAcabou() {
		desafioIniciado = false;
    resetForm();
    modalMessage = 'Desafio finalizado com falha!';
    showModal = true; // e aqui quando o tempo acabar
	}

	function closeModal() {
    showModal = false;
  }

</script>

<div class="min-h-screen bg-gray-100 flex items-center justify-center">
  <div class="container max-w-md shadow-md hover:shadow-lg transition duration-300 bg-white p-8 rounded-xl">
		{#if showModal}
			<div class="modal modal-open" on:click={closeModal}>
				<div class="modal-box relative" on:click|stopPropagation>
					<button on:click={closeModal} class="btn btn-sm absolute right-2 top-2">✕</button>
					<p>{modalMessage}</p>
				</div>
			</div>
		{/if}

    <Counter bind:this={counterComponent} bind:desafioIniciado on:tempoAcabou={handleTempoAcabou} />

    <form on:submit|preventDefault={handleSubmit} class="space-y-4">
      <div class="mb-4">
        <label for="nome" class="block text-sm font-medium text-gray-700">Nome:</label>
        <input type="text" id="nome" bind:value={nome} class="input input-bordered w-full" required disabled={!desafioIniciado}>
      </div>

      <div class="mb-4">
        <label for="telefone" class="block text-sm font-medium text-gray-700">Telefone:</label>
        <input type="tel" id="telefone" bind:value={telefone} class="input input-bordered w-full" required disabled={!desafioIniciado}>
      </div>

      <div class="mb-4">
        <label for="email" class="block text-sm font-medium text-gray-700">Email:</label>
        <input type="email" id="email" bind:value={email} class="input input-bordered w-full" required disabled={!desafioIniciado}>
      </div>

      {#if !desafioIniciado}
        <button on:click={iniciarDesafio} class="w-full btn bg-green-200 text-white cursor-pointer">Iniciar Desafio</button>
      {/if}

      {#if desafioIniciado}
        <button type="submit" class="w-full btn bg-green-200 text-white cursor-pointer">Enviar</button>
      {/if}
    </form>
  </div>
</div>

