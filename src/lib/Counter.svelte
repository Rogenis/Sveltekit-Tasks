<script lang="ts">
  import { onDestroy, createEventDispatcher } from 'svelte';
  import { writable } from 'svelte/store';

  export let desafioIniciado: boolean = false;
  const dispatch = createEventDispatcher();
  export const tempo = writable(15); // Exporta 'tempo' para que o pai possa se inscrever
  let interval: ReturnType<typeof setInterval>;

  export function resetCounter() {
    tempo.set(15);
    clearInterval(interval);
  }

  export function startCounter() {
    resetCounter();
    clearInterval(interval);
    tempo.set(15);
    resetCounter();

    interval = setInterval(() => {
      tempo.update(n => {
        if (n > 0) return n - 1;
        clearInterval(interval);
        dispatch('tempoAcabou');
        return 0;
      });
    }, 1000);
    // ...configura o intervalo...
  }

  onDestroy(() => {
    clearInterval(interval);
  });

  $: if (desafioIniciado) {
    startCounter();
  }

  onDestroy(() => {
    clearInterval(interval);
  });
</script>

{#if desafioIniciado}
  <p>Tempo restante: {$tempo}</p>
{/if}
